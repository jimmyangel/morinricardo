---
title: 'New HOWL Project: Logging Visualization'
tags:
  - leaflet
  - howl
date: 2018-08-16T00:00:00.000Z
---
### Visualization and time-based animation of logging activities in Oregon

- - -

The latest HOWL project is about raising awareness of logging activities in Oregon.

We are starting the project with the visualization of logging activities in National Forests. Later on, we will add logging activities in Bureau of Land Management (BLM) lands. And finally, we will attempt to include logging activities in private lands as well.

<!--more-->

### Accessing the visualization

To view the latest version of the visualization, head over to [https://logging.oregonhowl.org](https://logging.oregonhowl.org/). The site is also embedded in the [HOWL](https://oregonhowl.org/) site as a new 'spotlight'. 

As of this blog post's date, we are showing logging activities in National Forests where data is available, i.e., all forests except for Mount Hood and Siuslaw.

<p align="center">
	<img src="/images/uploads/timber-harvest.png"/>
</p>

### Drilling down to a National Forest

To view details, click on one of the National Forest areas (or welcome signs).

<p align="center">
	<img src="/images/uploads/timber-harvest-willamette.png"/>
</p>

Use the controls located on the top-right part of the screen to adjust the time range and to view a time lapse animation of the selected range.

<p align="center">
	<img src="/images/uploads/timber-harvest-willamette-control.png"/>
</p>

### Getting more details

Zoom into an area of interest and you can see the logged areas in more detail. If you click on one of the shapes, the shape will be highlighted and the control box will display some details about the specific activities that occurred in that area.

<p align="center">
	<img src="/images/uploads/timber-harvest-willamette-control-pick.png"/>
</p>

### Available base map and overlay layers

We have included a number of layers to help customize the views.

<p align="center">
	<img src="/images/uploads/timber-harvest-layers.png"/>
</p>

Select among five base maps

* [World Topographic Map](http://www.arcgis.com/home/item.html?id=30e5fe3149c34df1ba922e6f5bbf808f) (default)
* [World imagery](http://www.arcgis.com/home/item.html?id=10df2279f9684e4a9f6a7f08febac2a9)
* [OpenStreetMap](https://www.openstreetmap.org/about)
* [Landsat Live](https://blog.mapbox.com/landsat-live-goes-live-21704dac3e0f)
* [Terrain Background](http://maps.stamen.com)

Pick one or more overlay layers

* [Township and Range Grid](https://en.wikipedia.org/wiki/Public_Land_Survey_System)
* [State Boundary](https://en.wikipedia.org/wiki/Oregon)
* [BLM Land](https://www.blm.gov/oregon-washington)
* Unharvested Forest Land (if available)

### Embedding in a web site

The logging application can be easily embedded in any web site by using the html `iframe` tag. Simply insert an `iframe` tag in your post or page with a link to site, like shown in the example below.

Notice how it is not merely an image, but the active timber harvests site which you can fully interact with.

<iframe width="100%" height="600" src="https://logging.oregonhowl.org/" frameborder="0"></iframe>

The code used to display the track above is here:

`<iframe width="100%" height="600" src="https://logging.oregonhowl.org/" frameborder="0"></iframe>`

- - -

Please contact me if you have any feedback or if you see any errors that need to be corrected.

- - -

_A note about data quality: logging spatial data are separately and independently reported by each National Forest office. Availability over date ranges varies considerably across offices and there can be significant omissions of logging activity, especially regarding older records.._
