---
title: "Logging visualization update: mo' data"
tags:
  - leaflet
  - howl
  - oregonwild
date: 2018-11-05T00:00:00.000Z
---
### Visualization now includes BLM, private and state lands data

- - -

The [logging visualization site](https://logging.oregonhowl.org) now includes data from the Bureau of Land Management (BLM) which expands the scope to cover nearly all federal public lands in Oregon. Where official records are incomplete, we include clearcuts on federal lands using data gathered by [Oregon Wild](https://oregonwild.org/) using a combination of public records and aerial imaging assessments.

In addition to public records from federal agencies, we have added clearcuts on private and state lands for the 2001-2017 time period using forest loss gleaned from Landsat satellite images thanks to the excellent [Global Forest Change](http://science.sciencemag.org/content/342/6160/850) project.

<!--more-->

The operation of the site remains pretty much the same, except that a new top level was added to select federal versus private/state data.

#### First time visit
<p align="center">
	<img src="/images/uploads/logging-new-top-level.png"/>
</p>

#### Subsequent visits

<p align="center">
	<img src="/images/uploads/logging-new-top-level-2.png"/>
</p>

### Data sources

* **Logging activities in National Forests** - Extracted from [USDA Forest Service timber harvests database](https://data.fs.usda.gov/geodata/edw/datasets.php?xmlKeyword=Timber+Harvests). Each individual National Forest independently contributes data to this database and it varies widely in terms of quality and time coverage. In cases where the data is severely underreported, we use clearcuts in federal lands data (see below).
* **Logging activities in BLM lands** - Extracted from the [BLM harvest land treatments dataset](https://www.blm.gov/or/gis/data-details.php?id=421) and organized by BLM District Office area. Covered time periods vary widely across District Offices.
* **Clearcuts in federal lands** - Obtained from logging records and aerial imaging assessments performed by the [Oregon Wild](https://oregonwild.org) team. These data cover the time period from 1960s to 2005.
* **Clearcuts in private and state lands** - Derived from Landsat imagery processed and published by the [Global Forest Change](https://earthenginepartners.appspot.com/science-2013-global-forest/download_v1.5.html) project. The raster data was then clipped to cover Oregon private and state lands, converted to vector format and filtered to remove areas under 3 acres in size. Note that the vast majority of the areas shown are clearcut logging with a very small component of other forest loss picked up by the satellites (e.g., thinning, agriculture, and fire).

- - -

Please contact me if you have any feedback or if you see any errors that need to be corrected.
