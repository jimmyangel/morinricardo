---
title: 'Oregon Drinking Watersheds'
tags:
  - leaflet
  - howl
date: 2022-10-10T00:00:00.000Z
---
### Who's drinking water is influenced by any give spot on the map?

That is the question Oregon Wild wanted to be able to answer at a moment's notice.

My latest project as a volunteer for Oregon Wild was about drinking water in Oregon. The organization needed the data and a tool to easily understand the extent of Oregon's drinking watersheds and how populations are interconnected through common watersheds.

<!--more-->

### Data analysis and a web application

The project had two parts. First, generating a GIS dataset from multiple sources. Second, delivering a web application that could be easily used by anyone to view Oregon's drinking watersheds and discover how populations and communities are interconnected through them.

On this post, I introduce the web application: how to access and how to use it. On a future post, I will describe the process we used to compile and assemble the underlying data.

### Accessing the web application

To access the web application, head over to [https://logging.oregonhowl.org](https://watersheds.oregonhowl.org/). The site is also embedded in the [HOWL](https://oregonhowl.org/) site as a new 'spotlight'.

<a align="center" href="https://watersheds.oregonhowl.org/">
	<img src="/images/uploads/oregon-watersheds.jpg"/>
</a>

You will see a web map centered in Oregon, with a turquoise polygon layer, representing the geographical extent of Oregon's drinking watersheds.

* **Top-left** - Controls for zooming, view reset, base maps and overlays.

* **Top-right**, An information panel that displays data as you interact with the map. You can adjust the opacity of the watersheds layer by moving slider.

* **Bottom-right**, An "About" control used to display general information about the application.

### Viewing watersheds

Click on a spot within any of the turquoise polygons. In the example below, we show that the picked spot near Timothy Lake, connects 240,968 people through the Clackamas River watershed, which is highlighted in gray on the map.

A disturbance near that particular spot could potentially impact the drinking water of Estacada, Clackamas, Milwaukie, Oregon City and Lake Oswego communities!

<p align="center">
	<img src="/images/uploads/oregon-watersheds-spot.jpg"/>
</p>

By default, the list of communities is sorted by population in descending order. But it can also be shown in "downstream" order by clicking on the checkbox.

Clicking on the community name (or the down arrow), displays more details about the water system for that community and also highlights the sub-watershed using a purple stripes pattern.

<p align="center">
	<img src="/images/uploads/oregon-watersheds-detail.jpg"/>
</p>

### Available base map and overlay layers

We have included a number of layers to help customize the view.

<p align="center">
	<img src="/images/uploads/oregon-watersheds-layers.jpg"/>
</p>

Select among three base maps

* [World Topographic Map](http://www.arcgis.com/home/item.html?id=30e5fe3149c34df1ba922e6f5bbf808f) (default)
* [OpenStreetMap](https://www.openstreetmap.org/about)
* [World imagery](http://www.arcgis.com/home/item.html?id=10df2279f9684e4a9f6a7f08febac2a9)

Pick one or more overlay layers

* [Oregon DEQ Hydrography](https://arcgis.deq.state.or.us/arcgis/rest/services/WQ/DEQ_Streams/MapServer/0)
* [Township and Range Grid](https://en.wikipedia.org/wiki/Public_Land_Survey_System)
* [State Boundary](https://en.wikipedia.org/wiki/Oregon)

### Embedding in a web site

The web application can be easily embedded in any web site by using the html `iframe` tag. Simply insert an `iframe` tag in your post or page with a link to site, like shown in the example below.

Notice how it is not merely an image, but the active Oregon Drinking Watersheds site which you can fully interact with.

<iframe width="100%" height="600" src="https://watersheds.oregonhowl.org/" frameborder="0"></iframe>

The code used to display the site above is here:

`<iframe width="100%" height="600" src="https://watersheds.oregonhowl.org/" frameborder="0"></iframe>`

---

Please contact me if you have any feedback or if you see any errors that need to be corrected.
