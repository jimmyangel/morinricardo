---
title: 'Oregon Drinking Watersheds - compiling the data'
tags:
  - howl
  - qgis
  - oregonwild
date: 2022-11-03T00:00:00.000Z
---
In my [previous post](/post/2022-10-21-oregon-watersheds/), I introduced the Oregon Drinking Watersheds project and I described the [web application](https://watersheds.oregonhowl.org/) in detail.  In this post, I will discuss the process we used to compile the data.

<!--more-->

We carried out this process under the direction, and in partnership with Erik Fernandez of [Oregon Wild](https://oregonwild.org/).

## The data for the watersheds site comes from multiple sources

### Drinking watershed data: Oregon Department of Environmental Quality (DEQ)

The primary data source for the watersheds project is the [Surface water drinking water source areas in Oregon](https://www.oregon.gov/deq/wq/programs/pages/dwp-maps.aspx) dataset, published by [DEQ](https://www.oregon.gov/deq/Pages/index.aspx). This dataset delineates all community public water systems using surface water directly, or groundwater under the influence of surface water.

Each water system watershed is mapped upstream from its intake. When there are multiple water intakes for a single watershed, each water system is mapped upstream from intake to intake, resulting in a series of non-overlapping areas representing sections of a hydrological unit.

For example, the image below shows how [DEQ](https://www.oregon.gov/deq/Pages/index.aspx) maps the data for the Clackamas River watershed.

<p align="center">
	<img src="/images/uploads/oregon-watersheds-clackamas.jpg"/>
</p>
<p align="right" style="margin-top: -20px;"><small><small>Oregon DEQ, OpenStreetMap</small></small></p>

There are five non-overlapping sections of the Clackamas watershed. Moving upstream from intake to intake, they are: Lake Oswego, Oregon City, Milwaukie, Clackamas, and Estacada.

### Population data: Oregon Health Authority (OHA)

The [DEQ](https://www.oregon.gov/deq/Pages/index.aspx) data source does not include population information. To obtain population data, we consulted the [OHA Drinking Water Online](https://yourwater.oregon.gov/index.html) database. One of the data items available is the population being served by a given water system.

For example, for the [city of Estacada](https://yourwater.oregon.gov/inventory.php?pwsno=00279), the site shows a population of 3,725.

### Population data: Portland State University (PSU)

Another source of population data came from [PSU](https://www.pdx.edu/)'s [Population Estimate Reports](https://www.pdx.edu/population-research/population-estimate-reports), which is the official source of population estimates for the state of Oregon.

For example, for the city of Lake Oswego, [PSU](https://www.pdx.edu/) reports a population of 39,480 for the year 2020.

<a name="hydro"></a>
### Hydrography data: Oregon Spatial Data Library

We used [Oregon Rivers (Rivers Reach)](https://spatialdata.oregonexplorer.info/geoportal/details;id=161b8e74e7ef457180fba6429c9ee1ee) dataset and the [Oregon Watershed Boundary Dataset](https://spatialdata.oregonexplorer.info/geoportal/details;id=4b1b008d5a764a209b2df040689c0779) to assist in understanding water flows and watershed boundaries when analyzing areas with complicated topographies (e.g., Umpqua River), and areas where upstream watersheds were missing from the [DEQ](https://www.oregon.gov/deq/Pages/index.aspx) dataset (e.g., Rogue River, Chetco River).

## We followed a structured methodology

### Objective

The objective was to create a dataset in [Shapefile format](https://en.wikipedia.org/wiki/Shapefile) with the full extent of each drinking watershed and the population it serves. This is in contrast to the [DEQ](https://www.oregon.gov/deq/Pages/index.aspx) dataset which only maps watersheds from intake to intake, and does not include population data. Each watershed must also include the total downstream population (e.g, from current watershed and below).

Note that all water systems located in shared watersheds are represented as overlapping polygons. Given an arbitrary geographical location, we can make a list of all polygons intersecting at that location (several items, or just one in case there are no overlaps). This exposes all the water systems related or connected to the location. Moreover, by sorting the list in descending order of total population, the list can be presented in downstream order!

### Tools

To perform this analysis, we used the [QGIS Geographical Information System (GIS)](https://qgis.org/en/site/). [QGIS](https://en.wikipedia.org/wiki/QGIS) is an open source application for geospatial analysis and mapmaking. It is an extremely powerful GIS package and it is free!

We also used the [Mapshaper Command Line Interface (CLI)](https://github.com/mbloch/mapshaper/wiki) to generate the [Geojson](https://geojson.org/) dataset used by the [web application](https://watersheds.oregonhowl.org/). [Mapshaper](https://github.com/mbloch/mapshaper) is also open source and free.

### Process

#### Add population

We consulted the population data sources and updated the [DEQ](https://www.oregon.gov/deq/Pages/index.aspx) watersheds with the corresponding figures.

There are cases in which two or more watersheds are used to supply water to a single community.

* If the watersheds are independent, and belong to the same hydrological unit, we merged the watersheds into a single shape (e.g., City of Warrenton).

* If the watersheds fall under separate and independent hydrological units, we split the population evenly across watersheds (e.g., City of Powers). In some cases we had to do both (e.g., City of Corvallis).

#### Extend watersheds

To delineate the full extent of each watershed, we performed the following steps:

* Identified [DEQ](https://www.oregon.gov/deq/Pages/index.aspx) water systems that share a watershed (e.g., Clackamas); we used the [hydrography data](#hydro) to assist in this step

* Starting from the uppermost polygon in the set,
  * Made a copy of the polygon
  * Merged the copy with the next downstream polygon, taking the attributes of the latter
  * Repeated going downstream until reaching the last polygon in the set

The method is the same, but it becomes a bit tricky, when we find watersheds that have multiple "branches" (e.g., Umpqua River). We used the [hydrography data](#hydro) to assist in determining the direction of the water flow. The method also works for tributary watersheds located inside larger watersheds (e.g, city of Ashland).

The example below shows the watershed for the city of Clackamas. On the left is the [DEQ](https://www.oregon.gov/deq/Pages/index.aspx) shape. On the right is the extended watershed, after performing the steps above.

  <div style="display: flex; font-size: 0.75em;">
    <div align="center">
	   <img src="/images/uploads/oregon-watersheds-city-of-clackamas-deq.jpg"/>
     City of Clackamas - Before
    </div>
    <span>&nbsp;</span>
    <div align="center">
      <img src="/images/uploads/oregon-watersheds-city-of-clackamas-extended.jpg"/>
      City of Clackamas - After
    </div>
  </div>

#### Add missing watersheds

In some instances, there were entire upstream watershed sections missing from the [DEQ](https://www.oregon.gov/deq/Pages/index.aspx) dataset. Take, for instance, the city of Gold Beach. In the [DEQ](https://www.oregon.gov/deq/Pages/index.aspx) dataset, the watershed is limited to the Lower Rogue and the Lobster Creek [5th field watersheds](https://oregonexplorer.info/content/what-watershed?topic=56&ptopic=98#:~:text=Fifth%2Dfield%20watersheds%20have%20become,has%2033%20fifth%2Dfield%20watersheds.). So we added the missing parts, which include the entire Rogue River and Illinois River watersheds.

We extended the watersheds for the cities of Clatskanie, Rainier, and Herminston to include the Columbia River watershed. We also extended the watershed for the city of Ontario to include the Snake River upstream from Ontario, plus the Jordan and the Owyhee Rivers. In addition, we extended the watershed for the city of Monmouth to include missing parts up the Willamette River and tributaries.

We added the watersheds for the cities of Brookings and Harbor, from the intakes up the Chetco River. [Oregon Wild](https://oregonwild.org/) determined that these water sources should have been considered "surface water," but they were missing from the [DEQ](https://www.oregon.gov/deq/Pages/index.aspx) dataset.

#### Remove some watersheds

We removed inactive and zero population watersheds. We also removed the watershed for the city of St. Helens, as the source is an underground aquifer, so it was deemed to be a groundwater source as opposed to a surface water source.

#### Snap geometries

We ran a geometry snapping algorithm to make sure all overlapping and adjacent polygons lined up exactly against each other, thus eliminating undesirable visual artifacts when viewing the dataset. We used a tolerance of 200 feet.

#### Compute total population per watershed

After all polygons were extended following the process described above, we ran an algorithm to compute the total populations associated with all polygons. The total population is defined as the sum of the populations associated with a given watershed plus all downstream watersheds. The algorithm is laid out below.

```
for each polygon P
  total population of P = population of P
  find a point A inside of P
  for each candidate polygon C where its bounding box contains A
    if C != P AND C intersects P AND area of P <= area of C
      total population of P += population of C
    end if
  end for
end for

```

We implemented the algorithm using [QGIS' Python console](https://docs.qgis.org/3.28/en/docs/user_manual/plugins/python_console.html) capability.

#### Generate simplified dataset for the web application

We generated a separate [Geojson](https://geojson.org/) dataset using the WGS84 coordinate reference system (CRS) to be used in the [web application](https://watersheds.oregonhowl.org/). To reduce the size of the dataset, we ran a simplification algorithm and limited the precision of the coordinates (latitude and longitude) to four decimals.

It should be noted that, for the web dataset, we decided to drop the Columbia watershed extensions to the cities of Clatskanie and Rainier, since the populations involved are relatively small, and the watershed cluttered the web view by its sheer size.

---

The results of this project are available on [Github](https://github.com/jimmyangel/watersheds-data), and since all the data has been derived from U.S. and Oregon Government sources, they have been placed in the [Public Domain](https://creativecommons.org/publicdomain/zero/1.0/).

Please contact me if you have any feedback or if you see any errors that need to be corrected.

---
