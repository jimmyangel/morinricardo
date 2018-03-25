---
title: Rikitraki Update
date: 2016-12-29
tags: ["rikitraki", "gpx", "gps", "3d", "cesium"]
---
### It has been a long time since my last post. Here is an update.

Wow, I can't believe that my last post was back in July. I have been pretty busy with some consulting work I am doing in the Internet of Things (IoT) field. Pretty exciting! But I am committed to continuing supporting and improving [RikiTraki](https://www.rikitraki.com)...

---

<!--more-->

## Latest [RikiTraki](https://www.rikitraki.com) Improvements

Back in July, I was looking around [RikiTraki](https://www.rikitraki.com) and noticed a couple of multi-segment tracks that had been uploaded to the site. Since [RikiTraki](https://www.rikitraki.com) did not have explicit support for this type of GPX file, the tracks were not being properly displayed on the site. So I went ahead and made some improvements to support this. [Here](https://www.rikitraki.com/?track=41lRnK_4Z) is one of the tracks that was not being displayed properly and now it is supported!

I also added a new type of activity to cover GPX tracks that are recorded with the explicit purpose of documenting the way of reaching the start of an activity such as a trailhead, for example, a bus ride or a road. The new activity type is called "Getting There," and it is available when uploading tracks as well as for filtering purposes. The associated icon looks like this ![](/images/uploads/gettingthere.png). Thanks to [tassia](https://github.com/tassia) who [requested](https://github.com/jimmyangel/rikitraki/issues/2) the feature on the [Rikitraki github repository](https://github.com/jimmyangel/rikitraki).

And finally, I updated the "about..." box to add credits for using road sign icons published in Richard C. Moeur's [site](http://www.trafficsign.us/).

## Planned Improvements

I am planning to spend time in the near future to incorporate some improvements to [RikiTraki](https://www.rikitraki.com). I would like to collect some feedback from folks who are using the site and prioritize those requests.

In the mean time, here is a list of things I would like to do:

* Upgrade to the latest version of [Cesium](https://cesiumjs.org/). The [Cesium team](https://cesiumjs.org/blog.html) has been making lots of improvements, including better performance and more efficient use of resources.

* Implement a more seamless transition between 2D and 3D visualizations. Right now, switching between these two modes require page reloads and using different libraries, [Leaflet](http://leafletjs.com/) for 2D and [Cesium](https://cesiumjs.org/) for 3D. [Cesium](https://cesiumjs.org/) supports 2D visualization, so an easy way to do it is to just use [Cesium](https://cesiumjs.org/) instead of using two separate libraries. But I have been hesitant to do that because of the hefty resource requirements of [Cesium](https://cesiumjs.org/), especially noticeable in mobile devices -- it is good to have a "lite" path that works well on mobile. After the upgrade above, I will go ahead and re-evaluate the decision.

* Add the option to pin tracks to the surface of the 3D terrain. Currently, tracks are being visualized in 3D using the altitude information recorded by the GPS. Naturally, the data does not exactly match the altitude of the terrain model, resulting in tracks that look like they are floating over the terrain and/or sinking under it. Adding the pin option would draw the track exactly on top of the terrain model, which should provide a more "natural" view of the track.

* Add the capability of filter tracks "near me" or near a selected click marker on the map. This has not really been a big issue thus far, but it will definitely be necessary as the number of tracks loaded in the database grows beyond what can be supported in the browser's memory.

* Add "swipe" support when displaying track pictures. This will make flipping through pictures more natural in touch devices like tablets and phones.

* Auto-assign region tags based on trailhead location. The application should be smart enough to do this, but it may not be as easy as it sounds :)

I would really appreciate if you let me know what you would like to see next in [RikiTraki](https://www.rikitraki.com) in the comments section below or directly in the [github repo](https://github.com/jimmyangel/rikitraki).

---
Ok, that's it for now. Happy New Year!
