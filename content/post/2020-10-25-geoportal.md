---
title: "Developing a serverless Geoportal for Provita"
tags:
  - geoportal
  - netlify
  - vue
  - gridsome
  - serverless
  - provita
date: 2020-10-25T00:00:00.000Z
---

Over the past few weeks, I have been working on a new project for the Provita organization.

The Provita team would like to maintain and publish a collection of geospatial data sets to enable researchers, 3rd party organizations, and casual users to have have access to up-to-date geographical information that can be used to support ecology, biodiversity, and environmental protection applications, studies and analyses.

In this post, I introduce the key project requirements and the overall design.

<!--more-->

## Key requirements

* **Web Application** - Accessible from standard, modern Web browsers

* **Publish data sets** - Provide the ability for authorized Provita users to upload vector data sets in shapefile format packaged in .zip format and raster data sets in geotiff format; data sets of arbitrary size shall be supported; data sets shall be available for download using a simple interface including an optional anonymous survey

* **Publish metadata** - Provide the ability to associate metadata about the published data sets, including description, date, format, source, and keywords (or tags)

* **Interactive maps** - Provide the ability to pre-view the data sets directly on the site using an interactive map

* **Publish news updates** -- Provide the ability to publish brief news updates with an associated image and reference link

* **Additional informational sections** -- The site shall include additional pages such as FAQ, About, and Contact

* **Multilingual** -- The site must be available in Spanish and in English

* **Hands-off maintenance** - After deployment, the site should be as easy as possible to keep up, regardless of the number of users hitting the site

* **Low cost deployment** - Preferably free!

## Design considerations

To develop this site, I looked around for some existing solutions, both commercial and open source, that I could adapt to meet our requirements. Unfortunately, I was not able to find anything truly affordable. I looked at Open Geoportal, Esri Geoportal Server, GeoNode, and the possibility of setting up a custom solution using Geoserver.

First, each one of these solutions would require a hosting service to run the server components or signing up for a 3rd party proprietary GIS service. That means $$$. In addition, server components need to be monitored and maintained. More $$$.

Second, each solution comes with a pretty heavy technology stack that needs to be mastered in order to implement meaningful customizations. Django, Spring, Tomcat, JQuery, oh my! Who is going to maintain that?

And finally, the functionality generally provided by these solutions seems to be overkill for the capabilities required by this site.

<p align="center">
  <img src="/images/uploads/cannon.svg"/>
</p>

Here is my rough estimate of usage patterns:

* Dozens of geospatial data sets with a maximum of probably around one thousand
* Data set sizes from a few megabytes with a few large files of hundreds of megabytes in size
* Site content updates a few times a year, may be once a day during busy periods
* A handful of admin users with site update privileges
* A few news items per year, with several items in a month during busy periods
* Maximum number of concurrent end-users unknown

Given the requirements and expected usage of the site, I am confident that adopting a serverless JAMStack approach for implementation is the best approach here.

* Pre-generated pages provide great performance and excellent SEO capability
* Scalability is not an issue given that most of the content is already pre-rendered and cached
* Pay only for what you use -- with the expected usage patterns, this could be as low as under $1 per month
* Minimal upkeep and maintenance needed with no servers or databases to monitor and tune

<p align="center">
  <img width="200" src="/images/uploads/swatter.svg"/>
</p>

## Design overview

And here is the obligatory diagram depicting the high level functionality of the system:

<p align="center">
  <img src="/images/uploads/geoportal-high-level.svg"/>
</p>

### Services used

|Service|Description|
|-----|-----|
|Github|We store the application code and the public data in separate repositories. The data repo is defined as a Git submodule within the application to integrate with the site generator. We also take advantage of Github's identity service and OAuth to provide authorization capabilities to the site.|
|Netlify |This is main site hosting service. Publish requests from the Admin pages trigger a site build request. The data repository submodule is updated prior to site build and the site builder (Gridsome) pulls the data from the submodule and it is used to pre-render the pages.|
|Amazon Web Services (AWS) | |

<style>
  table tr th:empty {
    display: none;
  }
</style>

### System components

|Component|Description|
|-----|-----|
|App sources  |   |
|Metadata and site content  |   |
|Site generator  |  |
|End-User Site| |
|Admin| |
|Lambda Functions| |
|Batch Jobs| |
|GIS Data Sets| |
|Generated Map Tiles| |
|Survey Results||


---

In my next post, I will dig deeper in the design and implementation of the site.

Chao!
