---
title: 'Oregon Drinking Watersheds'
tags:
  - leaflet
  - howl
  - oregonwild
date: 2022-10-21T00:00:00.000Z
---
### Who's drinking water is influenced by any given spot on the map?

That is the question [Oregon Wild](https://oregonwild.org/) wants to be able to answer at a moment's notice, and that's the subject of my latest volunteer project.

[Oregon Wild](https://oregonwild.org/) needs the data, and a tool, to easily understand the full extent of Oregon's drinking watersheds and how are populations connected to any Oregon location through common watersheds.

Many thanks to Erik Fernandez of [Oregon Wild](https://oregonwild.org/) for giving me the opportunity to work on this project!

<!--more-->

### Data analysis and a web application

The project had two parts. First, generating a GIS dataset from multiple sources. Second, delivering a web application that could be easily used by anyone to view Oregon's drinking watersheds and discover how populations and communities are interconnected through them.

On this post, I introduce the web application: how to access, how to use it, and how I built it. On an upcoming post, I will describe the process that we went through to compile and assemble the underlying data.

### Accessing the web application

To access the web application, head over to [https://watersheds.oregonhowl.org](https://watersheds.oregonhowl.org/). The site is also embedded in the [HOWL](https://oregonhowl.org/) site as a new 'spotlight'.

<a align="center" href="https://watersheds.oregonhowl.org/">
	<img src="/images/uploads/oregon-watersheds.jpg"/>
</a>

You will see a web map centered in Oregon, with a turquoise polygon layer, representing the geographical extent of Oregon's drinking watersheds.

* **Top-left** - Controls for zooming, view reset, base maps and overlays.

* **Top-right**, An information panel that displays data as you interact with the map. You can adjust the opacity of the watersheds layer by moving the slider left or right.

* **Bottom-right**, An "About" control used to display general information about the project.

### Viewing watersheds

Click on a spot within any of the turquoise polygons. In the example below, we show that the picked spot near Timothy Lake, connects 240,968 people through the Clackamas River watershed, which is highlighted in gray on the map.

A disturbance near that particular spot could potentially impact the drinking water of Estacada, Clackamas, Milwaukie, Oregon City and Lake Oswego communities!

<p align="center">
	<img src="/images/uploads/oregon-watersheds-spot.jpg"/>
</p>

By default, the list of communities is sorted by population in descending order. But it can also be shown in "downstream" order by clicking on the checkbox.

Clicking on the community name (or the down arrow) displays more details about the water system for that community, and also highlights its entire watershed using a purple stripe pattern.

<p align="center">
	<img src="/images/uploads/oregon-watersheds-detail.jpg"/>
</p>

### Available base map and overlay layers

We have included a number of layers to help customize the view.

<p align="center">
	<img src="/images/uploads/oregon-watersheds-layers.jpg"/>
</p>

Select among three base maps: [World Topographic Map](http://www.arcgis.com/home/item.html?id=30e5fe3149c34df1ba922e6f5bbf808f) (default), [OpenStreetMap](https://www.openstreetmap.org/about), or [World Imagery](http://www.arcgis.com/home/item.html?id=10df2279f9684e4a9f6a7f08febac2a9). Pick one or more overlay layers: [Oregon DEQ Hydrography](https://arcgis.deq.state.or.us/arcgis/rest/services/WQ/DEQ_Streams/MapServer/0), [Township and Range Grid](https://en.wikipedia.org/wiki/Public_Land_Survey_System), and [State Boundary](https://en.wikipedia.org/wiki/Oregon).

### Embedding in a web site

The web application can be easily embedded in any web site by using the html `iframe` tag. Simply insert an `iframe` tag in your post or page with a link to site, like shown in the example below.

Notice how it is not merely an image, but the active Oregon Drinking Watersheds site which you can fully interact with.

<iframe width="100%" height="600" src="https://watersheds.oregonhowl.org/" frameborder="0"></iframe>

The code used to display the site above is:

<code style="font-size: 12px;">&lt;iframe width="100%" height="600" src=&quot;https://watersheds.oregonhowl.org/&quot; frameborder="0"&gt;&lt;/iframe&gt;</code>

### Application building blocks

The [Open Source application](https://github.com/jimmyangel/watersheds) takes advantage of the following building blocks:

#### Mapping with [Leaflet](https://leafletjs.com/)

For this application, I decided to use [Leaflet](https://leafletjs.com/), which in my experience, is the Open Source mapping library that delivers the quickest development time.

I have used [MapLibre](https://maplibre.org/) in the past because of its excellent performance, especially when accessing vector tiles, and I mulled over whether to use it for this project or not. I also considered [OpenLayers](https://openlayers.org/), which offers a vast array of geo-processing capabilities. In the end, for this relatively simple application, nothing beats [Leaflet's](https://leafletjs.com/) ease of use and healthy plug-in ecosystem.

I augmented [Leaflet's](https://leafletjs.com/) capabilities with the following plug-ins:

* [ESRI Leaflet](https://developers.arcgis.com/esri-leaflet/) - The Government-hosted "Oregon DEQ Hydrography" and "BLM Township and Range Grid" overlay layers are based on [ESRI's ArcGIS](https://www.esri.com/en-us/arcgis/about-arcgis/overview) mapping services. This plug-in simplifies access to these layers.

* [Leaflet PIP](https://github.com/mapbox/leaflet-pip) - This plug-in developed by [Mapbox](https://www.mapbox.com/), provides a convenient way of finding overlapping polygons under a given set of coordinates, which is **exactly** what I needed to do to expose connected watersheds.

#### CSS with [Bulma](https://bulma.io/)

Dealing with CSS can be a real pain. So many options and quirks! That's why I went with [Bulma](https://bulma.io/), an easy to use and customizable CSS framework that can be tweaked to load only the pieces that are actually being used. I have used [Bulma](https://bulma.io/) in other projects and I enjoy its simplicity and flexibility.

#### HTML templating with [Handlebars](https://handlebarsjs.com/)

I really like to keep all HTML out of the Javascript application code. [Handlebars](https://handlebarsjs.com/) allows me to use templates to easily render HTML while keeping presentation and logic nicely separated.

---

On this post I described the latest web application I developed for [Oregon Wild](https://oregonwild.org/) as a volunteer. I described its usage and capabilities, as well as the building blocks I used to create the site.

I will talk about the underlying data on an upcoming post.

Please contact me if you have any feedback or if you see any errors that need to be corrected.
