---
title: "It is here! The Provita Geoportal"
tags:
  - geoportal
  - provita
date: 2021-06-02T00:00:00.000Z
---

I am pleased to announce the [beta release](https://www.provita.org.ve/noticias/provita-lanzo-el-geoportal-un-site-que-ofrece-informacion-geoespacial-y-ambiental-de-venezuela) of my latest volunteer project: the [Provita Geoportal](https://geoportal.provita.org.ve/en/). In previous posts I discussed some of the design and implementation details of the project, but I was waiting for the public release of the product to share the end-user and admin functionality of the site.

The purpose of the Geoportal is to enable professionals, researchers, academics and the general public to find, pre-view and download geospatial data sets about Venezuela. These data sets are published by the [Provita](https://www.provita.org.ve/) team and are associated with the organization's mission and projects.

<!--more-->

## What can I do with the Geoportal?

The key functionality of the site is available on the main page, through two informational panels presented side-by-side (or top-bottom in a narrow screen such as a mobile phone): a list of available data sets on the left (or top), and a map on the right (or bottom).

<p align="center">
  <a href="https://geoportal.provita.org.ve/en/"><img src="/images/uploads/geoportal-main.png"/></a>
</p>

### Left panel: find & download

The left panel presents a scrollable list of datasets available for preview and download. The list can be narrowed by using a "Search" field, a Tag "Filter" field, or a combination.

<p align="center">
  <img src="/images/uploads/geoportal-left.png"/>
</p>

Each item on the list can be expanded ![](/images/uploads/geoportal-expand.png) to expose the dataset details: the download link and approximate file size, a link to a descriptor of available map tiles, a switch to add (or remove) a preview of the data set to the map, the date of the data set, the data format (i.e., GeoTIFF, PDF, Shapefile), the data source (attribution), a list of tags associated with this data set, and a detailed description of the data set.

To narrow the list of items shown, there are two fields at the top of the panel: "search" and "filter.

<p align="center">
  <img src="/images/uploads/geoportal-search-filter.png"/>
</p>

The "search" field restricts the list of items to those with an exact match in either the title or the description. The "filter" field restricts the list of items to those that match the tags typed or selected; if multiple tags are used, the list of items is extended to include items that match **any** of the tags (i.e., logical OR).

### Rigth panel: preview on map

One or more data sets can be previewed on a slippy map panel by simply using the "add to map" switch ![](/images/uploads/geoportal-add-to-map.png). You can pan, zoom, and rotate the map using the pointing device (e.g., mouse) and the controls located on the top right corner of the map. Reset the view by clicking on ![](/images/uploads/geoportal-reset-view.png).

<p align="center">
  <img src="/images/uploads/geoportal-right.png"/>
</p>

With vector data sets, it is possible to inspect ![](/images/uploads/hand.png) individual shapes (e.g., polygons), to reveal their attributes.

<p align="center">
  <img src="/images/uploads/geoportal-attr-1.png"/>
  <br><br>
  <img src="/images/uploads/geoportal-attr-2.png"/>
</p>

To download an image of the preview map, click on the camera icon ![](/images/uploads/camera.svg). To select select a different  basemap layer, click on the layers icon ![](/images/uploads/layers.svg).

### Downloading data sets

To download a data set, click on the "Download" link on the information panel. You will be presented with an optional anonymous survey, prior to performing the download.

<p align="center">
  <img src="/images/uploads/geoportal-survey.png"/>
</p>

You only need to answer the survey once. After that, the survey will not show up again.

## Accessing the preview tiles

If you want to include the preview tiles in your own maps, click on the "tiles" ![](/images/uploads/geoportal-tiles-link.png) link on the data set information panel to get the details.

## More info

The main menu provides access to additional information about the site, a FAQ section, news section and contact information.

<p align="center">
  <img src="/images/uploads/geoportal-menu.png"/>
</p>

## Se habla español

Click on the little flag at the top right corner to choose your preferred language, english or español.

<p align="center">
  <img src="/images/uploads/geoportal-lang.png"/>
</p>

## How did I build the site?

If you want to know the technical details about how I built the Geoportal, head over to the following posts:

* [Developing a serverless Geoportal using the JAMStack](http://localhost:1313/post/2020-10-25-geoportal/)
* [Serverless Geoportal: where is the data?](https://morinricardo.com/post/2021-01-10-geoportal-data/)
* [Serverless Geoportal: Admin authorization](https://morinricardo.com/post/2021-01-25-geoportal-auth/)

---

Ok, that was a brief overview of the Provita Geoportal. In a future post, I will describe how the content of the site is managed using the "behind the scenes" Admin capability.

Goodbye!
