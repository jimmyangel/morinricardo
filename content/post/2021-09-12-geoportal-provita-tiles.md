---
title: "Provita Geoportal: generating map preview tiles"
tags:
  - geoportal
  - provita
  - aws
  - docker
  - awsbatch
date: 2021-09-12T00:00:00.000Z
---

On this post, I describe the approach used by the [Provita Geoportal](https://geoportal.provita.org.ve/en/) to pre-generate map preview tiles using a serverless approach.

## Why pre-generate tiles?

Traditionally, map tiles are generated dynamically (and cached) using a GIS server. This requires installation, monitoring and maintenance of some server capability, whether it is a physical server somewhere, a hosted virtual machine, or perhaps a container image hosted in some cloud service provider. A solution like this would most likely require hosting database management system as well. And we don't want any of that!

<!--more-->

As described on an [earlier post](/post/2020-10-25-geoportal/), two of key requirements of the Provita Geoportal are: a) hands-off maintenance and b) low-cost deployment. And these two requirements drove the adoption of the [JAMStack](https://jamstack.org/) approach.

Map tiles are just files with unique urls which follow a predefined numbering scheme corresponding to zoom level, X coordinate and Y coordinate. Therefore, they could be easily mapped to pre-generated files and served as files following the appropriate directory structure, rather than using an intermediary layer like a GIS server.

In fact, pre-generating tiles when the source files or the metadata changes is analogous to building the site when content is updated. So, pre-generating tiles seems to be an approach quite compatible with the [JAMStack](https://jamstack.org/) concept. We just need to figure out a serverless way of doing it.

## Pre-generating tiles from source GIS files

Pre-generating tiles involves the execution of compute-intensive commands. The specific method used to pre-generate tile sets is different depending on whether we are dealing with vector (Shapefile format) or raster datasets (GeoTIFF format).

### Vector tile sets

To generate [vector tile sets](https://en.wikipedia.org/wiki/Vector_tiles), we use [tippecanoe](https://github.com/mapbox/tippecanoe), a command line utility created by [Mapbox](https://www.mapbox.com/). The resulting tiles follow the [Mapbox vector tile spec](https://docs.mapbox.com/vector-tiles/specification/), which work really well with our map preview library, [Maplibre](https://maplibre.org/). Vector tiles are very small and [Maplibre](https://maplibre.org/) can render them quickly, which was the key factor for choosing this library to display maps. Also, since vector tiles are styled at rendering time, they can be generated immediately after a GIS file is uploaded by the Administrator.

The [tippecanoe](https://github.com/mapbox/tippecanoe) utility has a ton of options. Here are the options that work well for us:

```
  tippecanoe
    -q                                    # Quiet, do not generate progress messages
    --force                               # Overwrite any existing output files
    --layer=$namelc                       # Include layer name (parameter) in the output
    --name=$namelc                        # Tile set name (parameter)
    -r1                                   # Do not drop point geometries at low zoom levels
    --minimum-zoom=4                      # Lowest zoom level of generated tiles
    --maximum-zoom=10                     # Highest zoom level of generated tiles
    --output-to-directory vtiles/$namelc  # Output directory (parameter)
    $namelc.geojson                       # Input file (parameter)
```

The [tippecanoe](https://github.com/mapbox/tippecanoe) utility takes a geojson file as input, so before running [tippecanoe](https://github.com/mapbox/tippecanoe) we generate a geojson file using the Shapefile source file. For this purpose, we use the [mapshaper](https://github.com/mbloch/mapshaper) command line utility.

### Raster tile sets

To generate raster tile sets, we use the [gdal2tiles.py](https://gdal.org/programs/gdal2tiles.html) command line utility which is part of the [GDAL](https://gdal.org/) library. The resulting tiles follow the [OSGeo](https://www.osgeo.org/) Tile Map Service Specification, which consist of image files in .png format.

Raster tiles need to be styled at creation time, so they are generated after the Administrator completes associating the metadata to the raster data sets. We use the styling metadata to generate a color table and then we invoke the [gdaldem](https://gdal.org/programs/gdaldem.html) command line utility to incorporate the color table into a GDAL .vrt file, which is then used by the [gdal2tiles.py](https://gdal.org/programs/gdal2tiles.html) command to generate the tiles with the appropriate styling.


```
  gdaldem
    color-relief  # Generate color relief map
    -alpha        # Include alpha channel to support opacity
    $EXACT        # Use gradient color blend vs nearest color entry match
    -of vrt       # Output file format is GDAL .vrt
    "$name.tif"   # Input file
    color.txt     # Color table
    temp.vrt      # Output file

  gdal2tiles.py
    --processes=2       # Use 2 parallel process
    --profile=mercator  # OSMTILE MapML tiling scheme
    -q                  # Quiet, do not generate progress messages
    -z 4-10             # Minimum and maximum zoom levels of generated tiles
    temp.vrt            # Input file (out from gdaldem)
    rtiles/$namelc      # Output directory
```

## Pre-generating tiles on-demand

In keeping within our serverless approach, we need to dynamically allocate (and deallocate) cloud computational resources to execute the tile generation commands described above. Can we use [Amazon Web Services (AWS)](https://aws.amazon.com/) to achieve this goal? Yes, of course we can!

AWS [Lambda](https://aws.amazon.com/lambda/) functions are not appropriate in this case because these computations can be intensive and command execution can take several minutes to complete. [Lambda](https://aws.amazon.com/lambda/) functions are meant to run and return a result to the client very quickly (milliseconds), and should not block the user interface. In addition, the tile generation commands have a number of dependencies (e.g, tippecanoe, sqllite, gdal, aws-cli) that cannot be easily installed using [Lambda](https://aws.amazon.com/lambda/) functions.

Enter [AWS Batch](https://aws.amazon.com/batch/). With this capability, we can run arbitrary jobs, on-demand, using customized [Docker](https://www.docker.com/) images. Thanks to [AWS Batch](https://aws.amazon.com/batch/), it is possible to request whatever computational resources we need (e.g, memory, CPUs) to execute batch jobs timely and efficiently. Resources are only allocated and used to satisfy job execution, thus minimizing the cost of running these commands. There is no need to have any computation resources (real, virtual, containerized) pre-instantiated  and available all the time.

In addition, [AWS Batch](https://aws.amazon.com/batch/) provides the ability of running on ["Spot instances"](https://aws.amazon.com/ec2/spot/), to further optimize batch job execution costs. Spot instances are allocated using AWS excess capacity and they are priced with deep discounts (up to 90%). The only down side is that job execution can take a little longer while AWS finds and allocate resources, but it is usually no more than a couple of minutes.

## Setting up the AWS Batch Job environment

[AWS](https://aws.amazon.com/) is a very powerful cloud environment, but it is also very complex. There are multiple ways of achieving the same thing, e.g., using the [AWS Console](https://aws.amazon.com/console/), using the [AWS API](https://docs.aws.amazon.com/general/latest/gr/aws-apis.html) or using the [AWS command line interface](https://aws.amazon.com/cli/). In addition, there are a number of [access control policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_controlling.html) that need to be set up to do just about anything in [AWS](https://aws.amazon.com/).

In order to keep things repeatable and self-documented, we have opted to set up the [AWS](https://aws.amazon.com/) environment using the [AWS command line interface](https://aws.amazon.com/cli/). We have a [Github repo](https://github.com/Provitaonline/geoportal-batch) that contains the scripts necessary to set up the [AWS](https://aws.amazon.com/) environment for this project.

Running the [```setupwas```](https://github.com/Provitaonline/geoportal-batch/blob/master/scripts/setupaws.sh) script offers the following options:

<style>
  pre .code, pre code, .code, code {
    padding: 0px 0px;
  }
</style>

```
1) Create API user, S3 bucket, create and attach policies
2) Create compute environment
3) Create container repos
4) Push container images
5) Create job queues and job definitions
6) Do all of the above in sequence
q) Quit (no changes)
```

### Create API user, S3 bucket, create and attach policies

This part of the script is used to support all of our [AWS](https://aws.amazon.com/) activities, not just the [AWS Batch](https://aws.amazon.com/batch/) part, for example, [S3](https://aws.amazon.com/s3/).

Here we create a user specifically to run the [AWS](https://aws.amazon.com/) APIs with restricted privileges, so that we do not use the [AWS](https://aws.amazon.com/) account administrator credentials anywhere in the application.

In this section we also create the [S3](https://aws.amazon.com/s3/) bucket where Geoportal keeps all of its files, including the public and private GIS files, pre-generated tiles, and private user survey responses.

Finally, we create the  policies and roles needed to enable API access and [AWS Batch](https://aws.amazon.com/batch/) job execution access requirements (i.e., [AWSBatchServiceRole](https://docs.aws.amazon.com/batch/latest/userguide/service_IAM_role.html), [ecsInstanceRole](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/instance_IAM_role.html), [ecsTaskExecutionRole](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task_execution_IAM_role.html), and [AmazonEC2SpotFleetRole](https://docs.aws.amazon.com/batch/latest/userguide/spot_fleet_IAM_role.html)).

This step only needs to be run once.

### Create compute environment

In this section of the script, we [establish the compute environment](https://docs.aws.amazon.com/cli/latest/reference/batch/create-compute-environment.html) requirements for running our batch jobs.

Here is the resulting definition:

```
{
  "computeEnvironmentName": "geoportalp-spot",
  "type": "MANAGED",
  "state": "ENABLED",
  "computeResources": {
    "type": "SPOT",
    "minvCpus": 0,
    "maxvCpus": 2,
    "desiredvCpus": 0,
    "instanceTypes": [
      "optimal"
    ],
    "tags": {
      "Name": "geoportal-spot"
    },
    "subnets": [
      "subnet-XXXXXXXX",
      "subnet-YYYYYYYY",
      "subnet-ZZZZZZZZ"
    ],
    "securityGroupIds": [
      "sg-SSSSSSSS"
    ],
    "spotIamFleetRole": "arn:aws:iam::ACCOUNT:role/AmazonEC2SpotFleetRole",
    "instanceRole": "arn:aws:iam::ACCOUNT:instance-profile/ecsInstanceRole"
  },
  "serviceRole": "arn:aws:iam::ACCOUNT:role/AWSBatchServiceRole"
}
```

This step only needs to be run once.

### Create container repos

Here we create [container repositories](https://aws.amazon.com/ecr/) to store the [Docker](https://www.docker.com/) images that are used by the [AWS Batch](https://aws.amazon.com/batch/) commands. We have two repos, one for the vector tile generation image (geoportalp-vtiles) and one for the raster tile generation image (geoportalp-rtiles).

This step only needs to be run once.

### Push container images

In this section of the script, we create the [container images](https://www.docker.com/resources/what-container) defined under the [```docker```](https://github.com/Provitaonline/geoportal-batch/tree/master/scripts/docker) subdirectory.

For generating vector tiles, we use as standard [amazonlinux docker image](https://hub.docker.com/_/amazonlinux), and we install the following dependencies:

* **deltarpm, which, unzip, aws-cli, git, sqlite-devel, curl, jq, development tools** - General purpose commands and utilities we need
* **mapshaper** - We use [mapshaper](https://github.com/mbloch/mapshaper) to convert shapefile files to geojson, which is tippecanoe's required input format
* **tippecanoe** - We use [tippecanoe](https://github.com/mapbox/tippecanoe) to generate vector tiles, as describe above.

For generating raster tiles, we use an [osgeo/gdal docker image](https://github.com/OSGeo/gdal/tree/master/gdal/docker) (osgeo/gdal:ubuntu-small-latest) which is a small image that already has [GDAL](https://gdal.org/) and all of it's dependencies pre-installed. We use [GDAL](https://gdal.org/) as described above to generate raster tiles.

This step needs to be run at least once and whenever there is a need to make changes to one or both of the container images.

### Create job queues and job definitions

Here we define two [job queues](https://docs.aws.amazon.com/batch/latest/userguide/job_queues.html) and two [job definitions](https://docs.aws.amazon.com/batch/latest/userguide/job_definitions.html) for the vector and raster jobs.

The [job queues](https://docs.aws.amazon.com/batch/latest/userguide/job_queues.html) are used to associate jobs to compute environments. In our case, the two job queues for vector and raster tiles are identical.

The [job definitions](https://docs.aws.amazon.com/batch/latest/userguide/job_definitions.html) are used to define command invocation, parameters, and additional job execution requirements (e.g., memory, CPUs) for each one of the batch job types (i.e., vtiles, rtiles).

This step needs to be run at least once and whenever there is a need to modify execution commands, parameters or computational requirements.

## Putting it all together

Ok, now that we have defined our [AWS Batch](https://aws.amazon.com/batch/) compute environment, we can easily submit batch jobs using the [AWS Batch](https://aws.amazon.com/batch/) API. We invoke the [AWS Batch](https://aws.amazon.com/batch/) API from within a [Lambda](https://aws.amazon.com/lambda/) function (submit-job), so that we can enforce our access control mechanism, i.e., the admin user must be a collaborator on the geoportal-data repository (see [here](/post/2021-01-25-geoportal-auth/) for details about admin user authorization).

The batch job invocation point will depend on what type of tiles are being generated. For vector tiles, the batch job can be invoked immediately after a GIS file is uploaded.

<p align="center">
  <img src="http://www.plantuml.com/plantuml/svg/XOynJmGX48LxViM4VYix2tDhSus52J4sDBFBU69W833ixvUiNivizjQIpFSztZjLmI8j1ZNEaWheqKNFq2eLhN8_-TbdP05zffHJ4GoMVvXNlD26US4uERHSsnIz35zfsW3Zku5dbFdJykMQbuT5VA2wWve7orhzxGJ3qxqgy0XlEIHqiD2wvQEdgdjmpxbzSLCF-PrvtJJh2_fBxHBTFDxTCyx4L53eMdqzg7-qxGI32_sQ9sBNOlW6">
  <br>
  <small>Vector tile generation</small>
</p>

For raster tiles, we must wait until the administrator defines the tile styling to be used as part of the file's metadata.

<p align="center">
  <img src="https://www.plantuml.com/plantuml/svg/bP0zJyGm38Rt_0ghUyN0nd2wW-4M1gGAiNHn6tCNb8_8SVZzD8NgKDGMCLLktiUFzHwJ84jsLk4eWQ4vQMSyv4JSg8WiPZGHlK1pfX032zhUVsTUy49rniWr3xslVZ-Z6pJsFkN16Ma_mb05JcyTFAACrzvV5arifu2bj1BjxelYzwdG7hOkX0TuZJQW1iOanF1kBAcjS7kuAMxeFrnFJ2XqbwEUFhCB70bE4-v8b-eMwGmN1fXYs0DNoYqhkdc2QLAhYKBkjLvsdCaJez1ItFoW_i3g0Zlyf1_oI5vdPxy0">
  <br>
  <small>Raster tile generation</small>
</p>

---

And that's it!

In this post, I described how we are using [AWS Batch](https://aws.amazon.com/batch/) to pre-generate tiles in the most economical and resource-conscious way possible.

Ah! On [September 29](https://callforpapers.2021.foss4g.org/foss4g2021/talk/GRQYCU/) I will be presenting the [Provita Geoportal](https://geoportal.provita.org.ve/en/) implementation at the [FOSS4G Buenos Aires conference](https://2021.foss4g.org/). See you there!
