---
title: 'Oregon Drinking Watersheds - compiling the data'
tags:
  - howl
  - qgis
  - oregonwild
date: 2022-11-02T00:00:00.000Z
---
In my [previous post](/post/2022-10-21-oregon-watersheds/), I introduced the Oregon Drinking Watersheds project and I described the web application in some detail.  In this post, I will discuss the process we used to compile the data.

<!--more-->

## The data for the watersheds site comes from multiple sources

### Drinking watershed data: Oregon Department of Environmental Quality (DEQ)

The primary data source for the watersheds project is the [Surface water drinking water source areas in Oregon](https://www.oregon.gov/deq/wq/programs/pages/dwp-maps.aspx) dataset, published by [DEQ](https://www.oregon.gov/deq/Pages/index.aspx). This dataset delineates all community public water systems using surface water directly, or groundwater under the influence of surface water.

Each water system watershed is mapped upstream from its intake. When there are multiple water intakes for a single watershed, each water system is mapped upstream from intake to intake, resulting in a series of non-overlapping areas representing sections of a hydrological unit.

For example, the image below shows how DEQ maps the data for the Clackamas River watershed.

<p align="center">
	<img src="/images/uploads/oregon-watersheds-clackamas.jpg"/>
</p>
<p align="right" style="margin-top: -20px;"><small><small>Oregon DEQ, OpenStreetMap</small></small></p>

There are five non-overlapping sections of the Clackamas watershed. Moving upstream from intake to intake, they are: Lake Oswego, Oregon City, Milwaukie, Clackamas, and Estacada.

### Population data: Oregon Health Authority (OHA)

The DEQ data source does not include population information. To obtain population data, we consulted the [OHA Drinking Water Online](https://yourwater.oregon.gov/index.html) database. One of the data items available is the population being served by a given water system.

For example, for the [city of Estacada](https://yourwater.oregon.gov/inventory.php?pwsno=00279), the site shows a population of 3,725.

### Population data: Portland State University (PSU)

Another source of population data came from PSU's [Population Estimate Reports](https://www.pdx.edu/population-research/population-estimate-reports), which is the official source of population estimates for the state of Oregon.

For example, for the city of Lake Oswego, PSU reports a population of 39,480 for the year 2020.

### Hydrography data: Oregon Spatial Data Library

We used [Oregon Rivers (Rivers Reach)](https://spatialdata.oregonexplorer.info/geoportal/details;id=161b8e74e7ef457180fba6429c9ee1ee) dataset and the [Oregon Watershed Boundary Dataset](https://spatialdata.oregonexplorer.info/geoportal/details;id=4b1b008d5a764a209b2df040689c0779) to assist in understanding water flows and watershed boundaries when analyzing areas with complicated topographies (e.g., Umpqua River), and areas where upstream watersheds were missing from the DEQ dataset (e.g., Rogue River, Chetco River).

## Methodology
