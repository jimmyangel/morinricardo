---
title: "Logging visualization: data pipeline documentation"
tags:
  - leaflet
  - howl
date: 2018-11-16T00:00:00.000Z
---
### The logging visualization site integrates data from multiple sources

- - -

The purpose of this post is to document the data processing steps applied to the original sources of the data, to create the layers used by [the logging visualization site](https://logging.oregonhowl.org).

<!--more-->

### Data pipeline in a nutshell

The diagram below depicts the flow of data from the original sources (left) mapped to the site navigation structure (right).
<p align="center">
	<a href="/images/uploads/logging-data-pipeline.svg"><img src="/images/uploads/logging-data-pipeline.svg"/></a>
</p>

### Primary data sources

|Name|Description|Format|
|-----------|-----------|------|
|[Global&nbsp;Forest&nbsp;Change](https://earthenginepartners.appspot.com/science-2013-global-forest/download_v1.5.html)|Results from time-series analysis of Landsat images in characterizing global forest extent and change.|GeoTIFF|
|Clearcuts&nbsp;in&nbsp;Federal&nbsp;Lands|Clearcuts in federal public lands documented by [Oregon Wild](https://oregonwild.org/) using aerial photography and logging records. Original dataset enhanced with the latest logging records from the US Forest Service and the BLM (below).|Shapefile|
|[USFS&nbsp;Timber&nbsp;Harvests](https://data.fs.usda.gov/geodata/edw/datasets.php?xmlKeyword=Timber+Harvests)|United States Forest Service (USFS) dataset which depicts areas treated as a part of the Timber Harvest program of work. Activities are self-reported by Forest Service Units.|Shapefile|
|[BLM&nbsp;Harvest&nbsp;Treatments](https://www.blm.gov/or/gis/data-details.php?id=421)|Bureau of Land Management (BLM) dataset which represents completed harvest land treatments on BLM managed lands in the states of Oregon and Washington. Harvest treatments are the cutting and removal or trees or biomass.|GDB|

### Processing

|Ref|Process|Input|Output|Tools|Commands|
|---------|----------|-----|------|-----|--------|
|<div style="text-align:center;">①</div>|Clip|<ul><li>Global Forest Change GeoTIFF (merged granules 50N-130W and 50N-120W)</li><li>Mask Vector Layer: Oregon boundary excluding Forest Service,  BLM, NPS, and BIA</li><li>Mask Vector Layer: Warm Springs boundary</li></ul>|<ul><li>Clipped GeoTIFF (minus Warm Springs)</li><li>Clipped GeoTIFF (Warm Springs)</li></ul>|QGIS|QGIS -> Raster -> Extraction -> Clip Raster By Mask Layer...|
|<div style="text-align:center;">②</div>|Merge|<ul><li>Clipped GeoTIFF (minus Warm Springs)</li><li>Vector Layer: Clearcuts in federal lands</li><li>Vector Layer: Clearcuts in Warm Springs</li></ul>|State-level clearcuts tiles: [{z}/{x}/{y}.png](https://github.com/jimmyangel/oregonhowl-tiles/tree/master/logging)|<ul><li>[Tilemill](https://github.com/jimmyangel/timber-harvest/tree/master/src/data-scripts/tilemill/top-level)</li><li>mb-util</li></ul>|Tilemill -> export<br><br>mb-util -> export|
|<div style="text-align:center;">③</div>|Gen|Clearcuts in Federal Lands|Federal lands clearcuts vector tiles: [{z}/{x}/{y}.pbf](https://github.com/jimmyangel/oregonhowl-vtiles/tree/master/timber-harvest/fedcuts-vtiles/all)|<ul><li>mapshaper</li><li>tippecanoe</li></ul>|mapshaper -i fedcuts/fedcuts.shp -proj wgs84 -verbose -o format=geojson precision=0.0001 fedcuts.json<br><br>tippecanoe --layer=fedcuts --name=fedcuts --no-tile-compression --minimum-zoom=9 --maximum-zoom=14 --simplification=20 --simplify-only-low-zooms --output-to-directory "fedcuts-vtiles/all" fedcuts.json<br><br><i>Note: Clipped versions of underreported areas (Fremont-Winema, Mount Hood, and Siuslaw) are also generated using the above recipe</i>|
|<div style="text-align:center;">④</div>|Split|<ul><li>Vector Layer: USFS Timber Harvests</li><li>Vector Layer: BLM Harvest Treatments</li></ul>|Area-level (National Forests and BLM District Offices) detailed vector tiles: [{z}/{x}/{y}.pbf](https://github.com/jimmyangel/oregonhowl-vtiles/tree/master/timber-harvest)|<ul><li>mapshaper</li><li>tippecanoe</li></ul>|[genvtiles.sh](https://github.com/jimmyangel/timber-harvest/blob/master/src/data-scripts/genvtiles.sh)|
|<div style="text-align:center;">⑤</div>|Poly|<ul><li>Clipped GeoTIFF (minus Warm Springs)</li><li>Clipped GeoTIFF (Warm Springs)</li></ul>|Private/state/tribal vector tiles: [{z}/{x}/{y}.pbf](https://github.com/jimmyangel/oregonhowl-vtiles/tree/master/timber-harvest)|<ul><li>QGIS</li><li>mapshaper</li><li>tippecanoe</li></ul>|QGIS -> Raster -> Conversion -> Polygonize<br><br>mapshaper -> simplify (Visvalingam, 30%), clean, filter remove-empty, filter 'this.area>12000', export precision 0.0001<br><br>mapshaper combine-files, export<br><br>mapshaper hansen-private-state-tribal.json -each 'assignedId=this.id' -o format=geojson hansen-private-state-tribal-i.json<br><br>tippecanoe --layer=timberharvest --name=timberharvest --no-tile-compression --minimum-zoom=6 --maximum-zoom=14 --include=assignedId --simplification=20 --simplify-only-low-zooms --coalesce --maximum-tile-bytes=100000 --output-to-directory "hansen" hansen-private-state-tribal-i.json<br><br>mapshaper hansen-private-state-tribal-i.json -each 'GIS_ACRES=Math.round(this.area*0.000247105)' -filter-fields assignedId,YEAR,GIS_ACRES -o format=json hansen/timber-or-s-info.json|

- - -
