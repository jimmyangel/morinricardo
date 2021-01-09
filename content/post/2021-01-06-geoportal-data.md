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
date: 2021-01-06T00:00:00.000Z
---


This is a follow up to my [earlier post](https://morinricardo.com/post/2020-10-25-geoportal/) discussing the design of a Geoportal for [Provita](https://www.provita.org.ve/).

In this post, I will focus on the data organization behind the Geoportal.

<!--more-->

## Data categories

| **Category** | **Data entities** | **Format** | **Access** | **Storage Platform** |
|----|----|-----|----|---|
| GIS Files |<ul><li>Shapefile bundles</li><li>GeoTIFF files</li></ul>| <ul><li>ZIP</li><li>TIF</li></ul> | Public | AWS S3 |
| GIS Tiles |<ul><li>Vector tiles</li><li>Raster tiles</li></ul>|<ul><li>[PFB](https://docs.mapbox.com/vector-tiles/specification/)</li><li>PNG</li></ul>| Public | AWS S3 |
| Site content |<ul><li>GIS Metadata</li><br><li>About</li><li>Contact</li><li>FAQ</li><li>News</li><li>Survey template</li><br><li>Images</li></ul> |<ul><li>JSON</li><br><li>Markdown</li><li>JSON</li><li>JSON</li><li>JSON</li><li>JSON</li><br><li>JPG, PNG</li></ul>| Public | Github |
| User content | Survey responses | JSON | Private | AWS S3 |

### Site content

Site content data feed the site generator when the site is built. Site content data are stored on a dedicated Github repository (geoportal-data) which is referenced in the main repository as a Git submodule, and is updated every time the site is generated via the "build" command. The data in this category are stored in JSON and/or markdown formats.

The following data types are included in this category:

| **Type** |**Description** |**Format** | **Gridsome<br> Type Name** |
|----|----|-----|----|
| GIS Metadata |Describes the content of uploaded GIS files (e.g., description, keywords, |JSON<br>Embedded&nbsp;markdown|MetaData|
|About|Text and images of the "About" page|Markdown<br>JPG & PNG Images|AboutData|
|Contact|Contact information|JSON|ContactData|
|FAQ|Questions and answers presented in the FAQ page|JSON<br>Embedded markdown|FAQData|
|News|News stories text and images|JSON<br>JPG & PNG Images|NewsData|
|User Survey Template|Definition of fields to capture in the user survey|JSON|UserSurveyTemplate|

The Github repository used to hold the site content data is public, but it could be made private if desired. However, since all of the data stored on this repository are published onto a public web site, there is no benefit in making the repository private. In order to be able to update the data, Admin users must be defined as collaborators in this repository.

### Uploaded GIS files

These are the actual Geographical Information System (GIS) data sets being shared on the Geoportal. Two types of files are supported: ZIP and TIF. The ZIP files are used to package Shapefile-formatted vector files. The TIF files are GEoTIFF-formatted raster files.

These files are stored using the Amazon Web Services (AWS) Simple Storage Service (S3). These files have public read access. In order to upload these files, Admin users must be defines as collaborators in the geoportal-data Github respository. This rule is enforced by a Lambda function (Netlify function).

For performance reasons, files are uploaded directly to AWS S3 from the Admin user's browser using AWS S3's Presigned Post links.

### Generated GIS files

### User content

### Key requirements

* **Web Application** - Accessible from standard, modern Web browsers.

* **Publish data sets** - Provide the ability for authorized Provita users to upload GIS vector data sets in [shapefile](https://en.wikipedia.org/wiki/Shapefile) format (packaged in .zip files) and raster GIS data sets in geotiff format; data sets of arbitrary size shall be supported; data sets shall be available for download using a simple interface including the submission of an optional anonymous survey.

* **Publish metadata** - Provide the ability to associate metadata about the published data sets, including description, date, format, source, and keywords (or tags).

* **Interactive maps** - Provide the ability to pre-view the published data sets directly on the site using an interactive map.

* **Publish news updates** -- Provide the ability to publish brief news updates with an associated image and reference link.

* **Additional information sections** -- The site shall include additional pages such as FAQ, About, and Contact.

* **Multilingual** -- The site must be available in both Spanish and English languages.

* **Hands-off maintenance** - After deployment, the site should be as easy as possible to keep up, regardless of the number of users hitting the site.

* **Low cost deployment** - As close to free as possible!

## Design considerations

To develop this site, I looked around for some existing solutions, both commercial and open source, that I could adapt to meet our requirements. However, I was not able to find anything that was truly affordable. I looked at several solutions like [Open Geoportal](http://opengeoportal.io/), [Esri Geoportal Server](https://www.esri.com/en-us/arcgis/products/geoportal-server/overview), [GeoNode](http://geonode.org/), as well as the possibility of setting up a custom solution using [Geoserver](http://geoserver.org/).

First, each one of these solutions would require a hosting service to run the server components or signing up for a 3rd party proprietary GIS service. That means $$$. In addition, server components need to be monitored and maintained. More $$$.

In addition, each solution comes with a pretty heavy technology stack that needs to be mastered in order to implement meaningful customizations. Django, Spring, Tomcat, JQuery (among others), Oh my! Who is going to support and maintain that?

Bottomline, the functionality generally provided by these solutions seems to be overkill for the capabilities required by this site.

<p align="center">
  <img width="500" src="/images/uploads/cannon.svg"/>
  <br><small><i>A cannon to kill a fly?</i></small>
</p>

Here is my rough estimate of expected usage patterns:

* There will be dozens of geospatial data sets (more than twenty but less than one thousand)
* The sizes of the shared data set sizes will vary widely from a few megabytes all the way to a few hundred megabytes each
* The content of the site will be updated a few times a year, but sometimes it may be updated once a day during busy periods
* There will be a handful of admin users with update privileges
* There will be a few news releases per year, possibly with several releases in a month during busy periods
* The maximum number of concurrent end-users is unknown and difficult to predict, but given the specialized nature of the site, the traffic will likely be modest.

<p align="center">
  <img width="100" src="/images/uploads/swatter.svg"/>
  <br><small><i>What is the right tool for the job?</i></small>
</p>

Given the requirements and expected usage of the site, I believe that adopting a [serverless](https://en.wikipedia.org/wiki/Serverless_computing) [JAMStack](https://jamstack.org/) approach for implementation is the most appropriate for this situation.

* Pre-generated pages provide great performance and excellent Search Engine Optimization (SEO) capability
* Scalability does not become an issue regardless of the number of users, given that the content is pre-rendered and cached
* We pay only for what we use -- and with the expected usage patterns, this could be as low as under $1 per month
* No babysitting -- with no servers or databases to monitor and tune, there is close to zero upkeep and maintenance needed.

## Design overview

And here is the obligatory diagram depicting the high level design of the system. It shows the main system components, grouped by the cloud service used to host them.

<p align="center">
  <img src="/images/uploads/geoportal-high-level.svg"/>
</p>

### User types

|Service|Description|
|-----|-----|
|End-users|The target audience of the site. Includes researchers, students, and the general public. End-users consume the content published on the site by viewing data set tiles on an interactive map, and downloading vector and raster data sets.|
|Admin users|Members of the Provita team who have the privileges to add, edit and remove content from the site. We take advantage of the Github identity system to validate user permissions to modify the site's content. A user has admin privileges if she or he as been added as a collaborator to the content repository.|

### Services used

|Service|Description|
|-----|-----|
|[Github](https://github.com/)|We store the [application code](https://github.com/jimmyangel/geoportal) and the <u>public</u> [data](https://github.com/jimmyangel/geoportal-data) in separate repositories. The data repo is defined as a Git submodule within the application which integrates with the site generator's data layer. We also take advantage of Github's identity service and OAuth to provide authorization capabilities to the site. The Github free plan is perfectly adequate for the needs of this application.|
|[Netlify](https://www.netlify.com/)|This is our main site hosting service. "Publish" requests from the Admin pages trigger a site build request. The data repository submodule is automatically updated prior to site build, and the site builder ([Gridsome](https://gridsome.org/)) pulls the data from the submodule in order to pre-render the pages. The Netlify free hosting plan is sufficient to cover the needs of this site.|
|[Amazon Web Services (AWS)](https://aws.amazon.com/)|We use [S3](https://aws.amazon.com/s3/) to store <u>public</u> GIS files for download, as well as <u>private</u> files such as survey responses. [Lambda functions](https://aws.amazon.com/lambda/) (via [Netlify functions](https://www.netlify.com/products/functions/)) provide secure interfaces to maintain and access these files. [Batch jobs](https://aws.amazon.com/batch/) are used to generate map tiles used to visualize the GIS data sets on the application map. AWS services are not free, but with the modest level of usage anticipated for this application, the cost should be very affordable, perhaps under $1 a month. S3 storage services are quite inexpensive and computational resources are only allocated when used.|

### System components

|Component|Description|
|-----|-----|
|App sources|Contains the [Vue](https://vuejs.org/), Javascript, HTML, and CSS code of the application. In addition, to display interactive maps, the application uses the [Mapbox GL JS](https://github.com/mapbox/mapbox-gl-js) open source library.|
|Metadata and site content |Stores the public content files including GIS files metadata, news items and supporting images, about page, FAQ page, contact page, and the template used to generate the anonymous survey. Content files are in JSON and markdown format.|
|Site generator|The [Gridsome build](https://gridsome.org/docs/gridsome-cli/#build) command pulls the data from the content directory (which points to the geoportal data repo) and makes it available in [GraphQL](https://gridsome.org/docs/data-layer/), which is used to pre-render the content in the application pages. Upon a "publish" request performed by an authorized user, the site is built and deployed to the [Netlify CDN](https://www.netlify.com/products/edge/).|
|End-User Site|[Pre-rendered](https://gridsome.org/docs/fast-by-default/#pre-rendered-html) site (Javascript, HTML, CSS ,and images), generated by the site generator.|
|Admin|The Admin functionality is a sub-section of the site which is protected by an authentication wall. Admin users who have collaborator access to the data repository, can login onto the Admin page using their Github credentials and maintain the site content.|
|Lambda Functions|There are a number of lambda functions used to execute certain functionality that requires authorization (e.g., posting GIS data sets, retrieving survey results). In addition, there are two lambda functions needed to implement the [Github OAuth](https://docs.github.com/en/free-pro-team@latest/developers/apps/authorizing-oauth-apps) flow.|
|Batch Jobs|Map tile generation is implemented using AWS batch jobs. This ensures that computational resources are only allocated during job execution. In addition, we use [Spot instances](https://aws.amazon.com/ec2/spot) to run the batch jobs at the least cost possible.|
|GIS Data Sets|These are the GIS .zip and .tif files shared by the site. These files are stored in AWS S3 and they are configured to have public read access.|
|Generated Map Tiles|Vector and raster map tiles generated by the batch jobs, used to enable preview of the GIS data sets using the site's interactive map component. Shapefile files are used to generate vector tiles (in [pbf format](https://docs.mapbox.com/vector-tiles/specification/)) and geotiff files are used to generate raster tiles (in png format). These files are stored in AWS S3 with public read access.|
|Survey Results|These are the files used to capture the end-user survey responses. They are stored in AWS S3, and they can only be downloaded by authorized users using the Admin functionality.|


---

In a series of upcoming topic-specific posts, I will dig deeper in the implementation details of the site.

Until then, ciao!
