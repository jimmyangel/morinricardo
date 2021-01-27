---
title: "Serverless Geoportal: where is the data?"
tags:
  - geoportal
  - netlify
  - aws
  - vue
  - gridsome
  - serverless
  - provita
date: 2021-01-10T00:00:00.000Z
---


This is a follow up to my [earlier post](https://morinricardo.com/post/2020-10-25-geoportal/) discussing the design of a Geoportal for [Provita](https://www.provita.org.ve/). In that post, I mentioned that we use the [serverless](https://en.wikipedia.org/wiki/Serverless_computing) [JAMStack](https://jamstack.org/) approach, with no servers and no databases to monitor and maintain.

So, "without" servers or databases, how are we organizing and managing the data of the site?

In this post, I answer the question by describing the data organization and the storage aspects of the Geoportal.

<!--more-->

## Data categories

Site data are organized in the following four categories: site content, uploaded Geographical Information System (GIS) data sets, generated GIS tiles, and user content.

### Site content

Site content data is used by [Gridsome](https://gridsome.org/), the site generator, to pre-render the site's static pages at build time.

The data in this category are stored in [JSON](https://www.json.org/json-en.html) and [markdown](https://en.wikipedia.org/wiki/Markdown) formats, and organized by data type. Data for each type are loaded at build time using [Gridsome file loaders](https://gridsome.org/plugins/@gridsome/source-filesystem) combined with [Markdown](https://gridsome.org/plugins/@gridsome/transformer-remark) and [JSON](https://gridsome.org/plugins/@gridsome/transformer-json) transformers and made available to the pre-render functionality as an in-memory database accessible via [GraphQL queries](https://gridsome.org/docs/data-layer/#the-graphql-data-layer).

The following data types are defined in the site content category:

| **Type** |**Description** |**Format** | **Gridsome<br> Type Name** |
|----|----|-----|----|
| GIS Metadata |Describes the content of uploaded GIS files (e.g., description, keywords, date) |JSON<br>Embedded&nbsp;markdown|MetaData|
|About|Text and images of the "About" page|Markdown<br>JPG & PNG Images|AboutData|
|Contact|Contact information|JSON|ContactData|
|FAQ|Questions and answers presented in the FAQ page|JSON<br>Embedded markdown|FAQData|
|News|News stories text and images|JSON<br>JPG & PNG Images|NewsData|
|User Survey Template|Definition of fields to capture in the user survey|JSON|UserSurveyTemplate|

The data in this category are stored in a dedicated Github repository ([geoportal-data](https://github.com/jimmyangel/geoportal-data)), separate from the [main repository](https://github.com/jimmyangel/geoportal), but referenced as a [Git submodule](https://github.blog/2016-02-01-working-with-submodules/). The submodule is updated every time the site is generated via the "build" command, which is triggered by the "publish" button on the Admin page.

The Github repository used to hold the site content data is public, but it could be made private if desired. However, since all of the data stored on this repository are published onto a publicly accessible web site, there is no benefit in making the repository private. It is important to note that Admin users must be defined as "[collaborators](https://docs.github.com/en/free-pro-team@latest/github/setting-up-and-managing-your-github-user-account/permission-levels-for-a-user-account-repository#collaborator-access-for-a-repository-owned-by-a-user-account)" in this repository to be able to update the data.

One big advantage of storing the content data in Github, is that all changes are tracked and recorded. So if necessary, it is really easy to audit the data to figure out who made changes and it would not be too hard to revert changes, if desired.

<p align="center">
  <img src="/images/uploads/octocat-1.png"/>
  <br>
  <small>Image credit <a href="https://myoctocat.com/">My Octocat</a></small>
</p>

An additional benefit of using Github is that we can easily leverage  Github identities. This, in combination with the repository "collaborator" concept, allows us to implement authentication and authorization capabilities in the Admin module. All without having to build our own identity sub&#8209;system!

<p align="center">
  <img width="70%" src="/images/uploads/github-connect.png"/>
  <br>
  <small>Geoportal authorization dialog</small>
</p>

### Uploaded GIS data sets

These are the actual GIS files published on the Geoportal. Two types of files are supported: .ZIP and .TIF. The .ZIP files are used to package [Shapefile](https://en.wikipedia.org/wiki/Shapefile) vector files. The .TIF files are [GeoTIFF](https://en.wikipedia.org/wiki/GeoTIFF) raster files.

These files are stored using the [Amazon Web Services (AWS)](https://aws.amazon.com/) [Simple Storage Service (S3)](https://aws.amazon.com/s3/), and they are configured with public-read access.

<p align="center">
  <img width="30%" src="/images/uploads/AWS-S3-01.png"/>
  <br>
  <small>AWS S3</small>
</p>

In order to upload these files, Admin users must be defined as "collaborators" in the [geoportal-data](https://github.com/jimmyangel/geoportal-data) Github repository. Since this is not a capability that exists in AWS, the rule is enforced by a [Lambda function](https://aws.amazon.com/lambda/) ([Netlify function](https://www.netlify.com/products/functions/)).

For better performance, files are uploaded directly to AWS S3 from the Admin user's browser using AWS S3's [Presigned Post](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#createPresignedPost-property) links.

We are using AWS S3 instead of Github for these files because: 1) there are no file size limitations, and 2) uploads can be initiated directly from the Admin user's browser, without any intermediaries and their associated overhead.

### Generated GIS tiles

These files are used by the site’s interactive map data set pre-view functionality, using standard map tiling schemes.

<p align="center">
  <img src="/images/uploads/tiles.png"/>
  <br>
  <small><small>Image credit: <a href="https://www.ogc.org/standards/tms">OGC</a></small></small>
</p>

The files are generated using [AWS Batch](https://aws.amazon.com/batch/) jobs which are triggered automatically by the site's Admin function. They are stored in AWS S3, have public-read access, and as a side benefit, can be accessed by any GIS user using a standard map tiling scheme url.

[Vector tiles](https://en.wikipedia.org/wiki/Vector_tiles) are stored in compressed (gzip) [PBF](https://docs.mapbox.com/vector-tiles/specification/) format. [Raster tiles](https://en.wikipedia.org/wiki/Tiled_web_map) are stored in PNG format.

Pre-generating map tiles is analogous to pre-rendering the site itself: instead of using a GIS server to generate (and cache) tiles on the fly, we generate all the tiles once (and whenever GIS files are replaced) and deploy them as static files.

This data category is best stored in AWS S3 versus Github, because it is comprised of thousands of small files which do not need to be tracked for changes. Therefore, the associated Github performance overhead would not be justified.

### User content

When end users fill out and submit the download survey, the data is saved in JSON format in AWS S3. These files can only be read by Admin users which are defined as "collaborators" in the [geoportal-data](https://github.com/jimmyangel/geoportal-data) Github repository. As with uploaded GIS data sets, this rule is enforced by a [Lambda function](https://aws.amazon.com/lambda/) ([Netlify function](https://www.netlify.com/products/functions/)).

Storing user content in AWS S3 has the advantage of simplicity for implementing a public-write, private-read policy without the need of requiring any kind of identify or credentials from end-users. It works well in our use case, where the data consist of anonymous survey submissions.

## Summary

As a handy reference, here is a quick summary of the discussion above.

| **Category** | **Data types** | **Format** | **Access** | **Storage Platform** |
|----|----|-----|----|---|
| Site content |<ul><li>GIS Metadata</li><br><li>About</li><li>Contact</li><li>FAQ</li><li>News</li><li>Survey template</li><br><li>Images</li></ul> |<ul><li>JSON</li><br><li>Markdown</li><li>JSON</li><li>JSON</li><li>JSON</li><li>JSON</li><br><li>JPG, PNG</li></ul>| Public-read<br>Admin-write | Github |
| Uploaded GIS data sets |<ul><li>Shapefile bundles</li><li>GeoTIFF files</li></ul>| <ul><li>ZIP</li><li>TIF</li></ul> | Public-read<br>Admin-write | AWS S3 |
| Generated GIS tiles |<ul><li>Vector tiles</li><li>Raster tiles</li></ul>|<ul><li>PBF</li><li>PNG</li></ul>| Public-read<br>Admin&#8209;write | AWS S3 |
| User content | <ul><li>Survey responses</li><ul> | <ul><li>JSON</li><ul> | Public-write<br>Admin-read | AWS S3 |

---

I hope this post provides valuable insights into how the data is managed in the Provita Geoportal.

In [upcoming](/post/2021-01-25-geoportal-auth/) posts I will continue to dig into various implementation details of the site.

Au revoir !
