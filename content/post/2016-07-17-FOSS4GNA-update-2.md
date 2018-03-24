---
title: FOSS4G NA Summary
date: 2016-07-17
tags: ["foss4gna"]
---
### It was my first time at a FOSS4G conference and I really enjoyed it. Here are my favorite talks.

---

## Favorite Talks

### Monday, May 2

#### <a name="zerowebmap"></a>[Zero to web map!](https://github.com/willbreitkreutz/web_mapping_workshop)
This was a well put together hands-on workshop that introduced the concepts of web mapping using Mapbox, adding data to the map using GeoJSON, handling pop-ups, map interactions, using the Geolocation API and mapping directions. In the end, you actually build a semi-useful mapping [web app](http://blog.rikitraki.com/web_mapping_workshop/) in just a few hours (note that if you are too far from Raleigh, NC, the walking directions API will not work due to maximum distance limitation &#x1F603;).

<!--more-->

I was already familiar with most of the concepts taught, but I did learn a bunch of useful bits including the OpenStreetMap Overpass API (including overpass-turbo), using the prose.io editor to directly edit Github content, and the Mapzen directions API. Plus, I really enjoyed Will's presentation style, quality, and pace.

### Tuesday, May 3

#### [Mapping the Planet from Outer Space](https://2016.foss4g-na.org/session/mapping-planet-outer-space.html)
Excellent introduction to NASA's [Global Imagery Browse Services (GIBS)](https://wiki.earthdata.nasa.gov/display/GIBS), the type of imagery available and how was the service built -- all code is open source and the imagery data is freely available to the public.

The speakers offered a nice overview of NASA's Earth Observation missions since the 1960's to present time, and how the quality of the imagery has drastically improved over the years.

The presentation also gave tons of great examples of how the imagery is being used today: air quality, tropical storms, ocean currents, flooding, sand storms and even visual arts!

RikiTraki project idea: near real-time visualization overlay of forest fires in the vicinity of a trail of interest.

#### [Valhalla - A Tiled Hierarchical Open-Source Routing Solution](https://2016.foss4g-na.org/session/valhalla-tiled-hierarchical-open-source-routing-solution.html)
This presentation introduced the Mapzen Valhalla Open Source multi-modal routing engine. The engine is used by the [Mapzen Turn-by-Turn](https://mapzen.com/products/turn-by-turn/?d=0&lat=40.7259&lng=-73.9805&z=12&c=multimodal&st_lat=37.737089&st_lng=-122.504467&st=SF%20Zoo&end_lat=37.80927&end_lng=-122.25981&end=699%20Bellevue%20Avenue%2C%20Oakland%2C%20CA%2C%20USA&use_bus=0.5&use_rail=0.6&use_transfers=0.4&dt=2016-07-18T08%3A00&dt_type=1) product, which is a hosted, free service with a very friendly API -- BTW, this service was the one used in the [Zero to Webmap!](#zerowebmap) workshop described above and it was really easy to use). [Here](http://valhalla.github.io/demos/routing/index.html#loc=14,45.996276,-121.809626&directionsoptions={"language":"en-US"}&costing="pedestrian"&directionsoptions={"language":"en-US"}) is a demo.

The API has some really interesting features such as route costing options (e.g., a bike route may want to offer less elevation gain), elevation profile of routes, and mobile optimizations like routing tiles.

I am seriously considering to add the routing capability to RikiTraki in two ways: a) getting to the trailhead, and b) creating a hiking or biking track on the fly since the Valhalla routing engine can route using OpenStreetMap trail data!

#### [Visualizing GPS Tracks with RikiTraki](https://2016.foss4g-na.org/session/visualizing-gps-tracks-rikitraki.html)

Hey, I really liked this presentation, especially because I was looking at the audience and not at the presenter! &#x1F60B;

### Wednesday, May 4

#### <a name="3dtiles"></a>[3D Tiles: Beyond 2D Tiling](https://2016.foss4g-na.org/session/3d-tiles-beyond-2d-tiling.html)

It was really great to attend one of the presentations given by the Cesium team! They described their proposal for 3D tiling as an open standard. 3D tiling is an data structure organization standard for storing and high performance streaming of 3D data. One example usage of 3D tiling is the creation of [visualization models for cities](https://cesiumjs.org/NewYork/index.html?view=-74.01931188565507%2C40.69274729396973%2C696.4681325179864%2C21.27912961153516%2C-21.34255089763543%2C0.07177796178673282) generated from building data (e.g., OpenStreetMap or Government entities). And the best part is that the Cesium team is implementing the standard as part of [Cesiumjs](https://cesiumjs.org) library! Go Cesium!

#### [OGC SensorThings API - Open IoT Made Easy](https://2016.foss4g-na.org/session/ogc-sensorthings-api-open-iot-made-easy.html)
The presenter gave an overview of the SensorThings API, an Open Geospatial Consortium (OGC) standard for interconnecting and accessing Internet of Things (IOT) devices.

The API definition slightly departs from pure REST by using a parenthesized representation of identities (with query functionality) instead of strictly hierarchical resources representations. It also includes a semantic model mostly based on the OGC Observations and Measurements standard. There is also an open source project called Whiskers that implements the standard in Javascript.

The project looks interesting but it is not clear how much traction it has, given that the entire IOT domain is evolving very rapidly and it is quite unstable at the present time. Besides, I did not like the not so RESTful API.

#### ["Hiroshima Archive" and disaster digital archives series](https://2016.foss4g-na.org/session/hiroshima-archive-and-disaster-digital-archives-series.html)
Hidenori Watanave introduced an interactive visualization of the Hiroshima nuclear bombing. Using the Cesium 3D library, the web application presents a compilation of historical documentation such as maps, photographs, survivor accounts and statistical information about the attack.

I think that [this web application](http://hiroshima.archiving.jp/index_en.html) is a very creative usage of 3D geographical visualization techniques and it very effectively conveys the terrible human impacts of nuclear warfare.

#### [Bringing OpenStreetMap to the third dimension with 3D Tiles and Cesium](https://2016.foss4g-na.org/session/bringing-openstreetmap-third-dimension-3d-tiles-and-cesium.html)
This was a great follow on presentation to the [3D Tiles session](#3dtiles). Matthew Amato went through the process he followed to grab OpenStreetMap data and turning it into [3D model of New York City](http://cesiumjs.org/NewYork). The result is amazing, especially considering that the OpenStreetMap data was not meant for this!

### Thursday, May 5

#### [Managing National Park Service Basemap Data with OpenStreetMap Tools](https://2016.foss4g-na.org/session/managing-national-park-service-basemap-data-openstreetmap-tools.html)
It was great to meet in person the team behind the National Park Service digital mapping capabilities!

The presenter, Jim McAndrew, told me that NPS trail data will be freely available soon -- I would definitely like to incorporate the data into RikiTraki in a visualization layer.

#### [Crowdsource logging roads using OSM](https://2016.foss4g-na.org/session/crowdsource-logging-roads-using-osm.html)
Construction of logging roads is a good indicator of tropical forest destruction. This group created a capability based on the OpenStreetMap (OSM) and the Humanitarian OSM team tool suite to identify and map logging roads that would otherwise be unknown. The data and visualizations enable researchers to understand ecosystem impacts and also identify illegal logging activities.

#### [Running Docker Containers in Development AND Production](https://2016.foss4g-na.org/session/running-docker-containers-development-and-production-sponsored-red-hat.html)
This presentation went over the next generation OpenShift platform, which is based on Kubernetes and Docker. The reason I found this presentation enlightening is the fact that RikiTraki is currently hosted in OpenShift online, so I was very interested on learning about the upcoming upgrade to the hosting platform.

---

## Other pretty good talks

### Monday, May 2

#### [Introduction to an Open Source GIS Desktop: QGIS](https://2016.foss4g-na.org/session/introduction-open-source-gis-desktop-qgis.html)
QGIS is a pretty powerful package, but frankly it is a bit complex and hard to use. This workshop was informational, but it could have been delivered more gently.

### Tuesday, May 3

#### [I see NoSQL document stores in geospatial applications](https://2016.foss4g-na.org/session/i-see-nosql-document-stores-geospatial-applications-sponsored-ibm.html)
An intro to IBM's Cloudant service.

#### [Open Source Photogrammetry with OpenDroneMap](https://2016.foss4g-na.org/session/open-source-photogrammetry-opendronemap.html)

How to take imagery captured with drones (and balloons and kites) and transform it into structured 3D data using [OpenDroneMap](https://github.com/OpenDroneMap/OpenDroneMap).

### Wednesday, May 4

#### [A Platform for Data Interoperability: Koop](https://2016.foss4g-na.org/session/open-source-photogrammetry-opendronemap.html)
Koop is an open source project for translating multiple geo formats.

#### [Cartography with Inkscape](https://2016.foss4g-na.org/session/cartography-inkscape.html)
Take the output from QGIS and import it into Inkscape for further manipulation and production of a final printable map.

#### [OpenSensorHub for SensorWebs and IoT](https://2016.foss4g-na.org/session/opensensorhub-sensorwebs-and-iot.html)
Showed some really cool IoT examples.

#### [Cheet'n Death - Tracking Predators in the 'open' Kalahari](https://2016.foss4g-na.org/session/cheetn-death-tracking-predators-open-kalahari.html)
Tracking and mapping Cheetahs in the Kalahari.

#### [Introduction to GTFS: The Standardized Transit Data Feed](https://2016.foss4g-na.org/session/introduction-gtfs-standardized-transit-data-feed.html)
Intro to the transit feed standard developed in Portland, OR.

### Thursday, May 5

#### [Point cloud web services with Greyhound, Entwine, and PDAL](https://2016.foss4g-na.org/session/point-cloud-web-services-greyhound-entwine-and-pdal.html)
Open source capabilities for processing point cloud data.

#### [The Contemporary Geospatial Webapp: Embracing React, Webpack, ES6, Node.js](https://2016.foss4g-na.org/session/contemporary-geospatial-webapp-embracing-react-webpack-es6-nodejs.html)
Described experiences with this software stack for developing geo web apps.

---
Ok, that's it for now. I learned a lot at this conference. I hope to have the chance to attend another FOSS4G conference in the future!
