---
title: Rikitraki renovations on the way!
tags:
  - rikitraki
  - gpx
  - gps
  - cesium
date: 2020-11-16T00:00:00.000Z
---

Wow! I can't believe it's been five years since I developed [Rikitraki](https://www.rikitraki.com/). A lot of things have changed in Web development since then, so I think it is time to implement some sorely needed renovations.

Some of the changes are simply to bring the code up to snuff with today's best practices. But some other changes will be functionality improvements that I have been thinking about for a long time.

In this post, I lay out what I am thinking for the next big release. Comments and suggestions are greatly appreciated.

<!--more-->

## Code base renovation

<p align="center">
  <img src="/images/uploads/renovation.jpg"/>
</p>

When I developed [Rikitraki](https://www.rikitraki.com/) five years ago, I decided to forgo any heavy duty frameworks in favor of plain Javascript and [JQuery](https://jquery.com/). At the time, [React](https://reactjs.org/) was really hot, but I thought it was overkill to implement the features that I had in mind for [Rikitraki](https://www.rikitraki.com/). Besides, it was not all that clear to me how easy it would be to incorporate big libraries such as [Leaflet](https://leafletjs.com/) and [Cesium](https://cesium.com/cesiumjs/) into a reactive framework such as [React](https://reactjs.org/). So I went ahead and implemented the project without a framework.

Another big decision I made back then was to incorporate two mapping libraries simultaneously: 1) [Leaflet](https://leafletjs.com/) for 2D mapping and 2) [Cesium](https://cesium.com/cesiumjs/) for 3D. This added complexity to the code base, but at the time I though it was worth it, because I needed a fall-back capability to support devices where [Cesium](https://cesium.com/cesiumjs/) would not be supported or simply it would be too heavy to run.

Fast forward five years:

* I had the opportunity to learn and use [Vue.js](https://vuejs.org/) and [Gridsome](https://gridsome.org/), both of which have been a real pleasure to use. I really like the component-based approach to coding: easier to read, easier to maintain, better reuse.
* [Cesium](https://cesium.com/cesiumjs/) has been around and continuously improving. I think it is pretty safe these days to use [Cesium](https://cesium.com/cesiumjs/)-based applications on a reasonably recent device and a modern browser.

So I think it is time to rewrite the front end in [Vue.js](https://vuejs.org/), [Gridsome](https://gridsome.org/) and [Cesium](https://cesium.com/cesiumjs/). No [JQuery](https://jquery.com/), no [Leaflet](https://leafletjs.com/), no [Bootstrap](https://getbootstrap.com/).

## Front end scalability improvements

Right now, [Rikitraki](https://www.rikitraki.com/) retrieves the entire list of tracks stored in the database and displays track markers on the map and the globe (the list is limited to MAX_TRACKS, defined in the back-end, and currently 5,000). In 2D, the markers are clustered, but in 3D, each marker is shown individually.

Even though marker clustering helps, it does not seem to be a great idea to put <u>everything</u> on the map, especially if the number of tracks grows above a few thousands. The current version of [Rikitraki](https://www.rikitraki.com/) has a filtering function to pare down the number of tracks to be loaded at once, but I don't think it is very intuitive to the casual user. I believe that I need to incorporate a very explicit and friendly way to limit the amount of data shown on the map.

I have learned that my original concept of "just show a map" as the main user interface, does not seem to scale well with a very large number of tracks.

I have been thinking about framing the map with interactive info panels that can be used to filter tracks as well as make it more explicit to the user about what they are actually seeing on the map.

<p align="center">
  <img src="/images/uploads/rikitraki-v2-sketch.svg"/>
</p>

I have done something similar already in a number of projects (e.g., [HOWL](https://oregonhowl.org/), [Libro Rojo](https://ecosistemasamenazados.org/fichas/), and [Geoportal](https://morinricardo.com/post/2020-10-25-geoportal/). Of course, I still need to define the details of the info panels, but that will happen as I develop the new application incrementally.

## Back end

The current [Rikitraki](https://www.rikitraki.com/) back end consists of a set of REST services running on [Node.js](https://nodejs.org/en/), accessing a [MongoDB](https://www.mongodb.com/) database, hosted on [Google Cloud Platform](https://cloud.google.com/). Even though I am not too keen on having to host and babysit a services layer and a database server, this seems to work just fine, and it is super cheap.

I may entertain going serverless using [Cloud Functions](https://cloud.google.com/functions) functions and a [cloud database](https://cloud.google.com/products/databases), but I am skeptical about finding something cheaper than what I currently have. So for the moment, no changes.

## Features

As a baseline, I will retain most, if not all, of the features currently available. I will also work on some new features I have had in mind for a long time ([documented](https://github.com/jimmyangel/rikitraki/issues) on Github):

* **Implement a more seamless transition between 2D and 3D visualizations** - By using [Cesium](https://cesium.com/cesiumjs/) as the single library for mapping, switching between 2D and 3D should be very straightforward.
* **Add the option to pin tracks to the surface of the 3D terrain** - This is still tricky. [Cesium](https://cesium.com/cesiumjs/) now supports ground clamping of polyline geometries. However, performance of dynamic lines (i.e., path graphics) clamped to ground is pretty slow. I will need to deep dive into this to see if I can figure out an reasonable approach or a workaround.
* **Add the capability of filter tracks “near me” or near a selected click marker on the map** - The [Rikitraki](https://www.rikitraki.com/) back end already has support for location-based queries. It is just a matter of integrating the front end.
* **Add “swipe” support when displaying track pictures** - Not hard to do, especially since I will be using [Vue.js](https://vuejs.org/).
* **Auto-assign region tags based on trailhead location** - I will need to integrate a reverse geolocation service. [Nominatim](https://nominatim.org/) looks really promising.

---

Please feel free to comment and/or suggest new features in the comments section below, or in the [issues](https://github.com/jimmyangel/rikitraki/issues) section of the Rikitraki Github repository. You can also send me a [Tweet](https://twitter.com/jimmieangel).

I will be posting here once I begin development, including a link to the early version, which will run in parallel since the back end is not changing.
