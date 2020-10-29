---
title: "Developing a serverless Geoportal using the JAMStack"
tags:
  - geoportal
  - netlify
  - aws
  - vue
  - gridsome
  - serverless
  - provita
date: 2020-10-25T00:00:00.000Z
---

Over the past few months, I have been working on a cool new project for the [Provita](https://www.provita.org.ve/) folks.

[Provita](https://www.provita.org.ve/) wants to maintain and publish a collection of geospatial data sets to enable researchers, 3rd party organizations, and casual users to have access to up-to-date Geographical Information Systems (GIS) data for supporting ecology, biodiversity, and environmental protection applications, studies and analyses.

In this post, I introduce key project requirements and the overall design of the app.

<!--more-->

## Key requirements

* **Web Application** - Accessible from standard, modern Web browsers.

* **Publish data sets** - Provide the ability for authorized Provita users to upload GI) vector data sets in [shapefile](https://en.wikipedia.org/wiki/Shapefile) format (packaged in .zip files) and raster GIS data sets in geotiff format; data sets of arbitrary size shall be supported; data sets shall be available for download using a simple interface including an optional anonymous survey.

* **Publish metadata** - Provide the ability to associate metadata about the published data sets, including description, date, format, source, and keywords (or tags).

* **Interactive maps** - Provide the ability to pre-view the published data sets directly on the site using an interactive map.

* **Publish news updates** -- Provide the ability to publish brief news updates with an associated image and reference link.

* **Additional information sections** -- The site shall include additional pages such as FAQ, About, and Contact.

* **Multilingual** -- The site must be available in both Spanish and English languages.

* **Hands-off maintenance** - After deployment, the site should be as easy as possible to keep up, regardless of the number of users hitting the site.

* **Low cost deployment** - As close to free as possible!

## Design considerations

To develop this site, I looked around for some existing solutions, both commercial and open source, that I could adapt to meet our requirements. Unfortunately, I was not able to find anything truly affordable. I looked at [Open Geoportal](http://opengeoportal.io/), [Esri Geoportal Server](https://www.esri.com/en-us/arcgis/products/geoportal-server/overview), [GeoNode](http://geonode.org/), and the possibility of setting up a custom solution using [Geoserver](http://geoserver.org/).

First, each one of these solutions would require a hosting service to run the server components or signing up for a 3rd party proprietary GIS service. That means $$$. In addition, server components need to be monitored and maintained. More $$$.

Second, each solution comes with a pretty heavy technology stack that needs to be mastered in order to implement meaningful customizations. Django, Spring, Tomcat, JQuery (among others), Oh my! Who is going to support and maintain that?

And finally, the functionality generally provided by these solutions seems to be overkill for the capabilities required by this site.

<p align="center">
  <img width="500" src="/images/uploads/cannon.svg"/>
  <br><small><i>A cannon to kill a fly?</i></small>
</p>

Here is my rough estimate of expected usage patterns:

* There will be dozens of geospatial data sets (more than twenty but less than one thousand)
* The sizes of the shared data set sizes will vary widely from a few megabytes all the way to a few hundred megabytes each
* The content of the site will be updated a few times a year, but sometimes it may be updated once a day during busy periods
* There will be a handful of admin users with have update privileges
* There will be a few news releases per year, with several releases in a month during busy periods
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

And here is the obligatory diagram depicting the high level design of the system:

<p align="center">
  <img src="/images/uploads/geoportal-high-level.svg"/>
</p>

### Services used

|Service|Description|
|-----|-----|
|[Github](https://github.com/)|We store the [application code](https://github.com/jimmyangel/geoportal) and the <u>public</u> [data](https://github.com/jimmyangel/geoportal-data) in separate repositories. The data repo is defined as a Git submodule within the application which integrates with the site generator's data layer. We also take advantage of Github's identity service and OAuth to provide authorization capabilities to the site. The Github free plan is perfectly adequate for the needs of this application.|
|[Netlify](https://www.netlify.com/)|This is our main site hosting service. "Publish" requests from the Admin pages trigger a site build request. The data repository submodule is automatically updated prior to site build, and the site builder ([Gridsome](https://gridsome.org/)) pulls the data from the submodule in order to pre-render the pages. The Netlify free hosting plan is sufficient to cover the needs of this site.|
|[Amazon Web Services (AWS)](https://aws.amazon.com/)|We use [S3](https://aws.amazon.com/s3/) to store <u>public</u> GIS files for download as well as <u>private</u> files (e.g., surveys responses). [Lambda functions](https://aws.amazon.com/lambda/) (via [Netlify functions](https://www.netlify.com/products/functions/)) provide secure interfaces to maintain and access these files. [Batch jobs](https://aws.amazon.com/batch/) are used to generate map tiles used to visualize the GIS data sets on the application map. AWS services are not free, but with the modest level of usage anticipated for this application, the cost should be very affordable, perhaps under $1 a month. S3 storage services are quite inexpensive and computational resources are only allocated when used.|

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

Ciao!
