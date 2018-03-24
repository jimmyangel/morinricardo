---
title: Open beta, 3D track animation and a new logo
date: 2016-03-11
tags: ["rikitraki"]
---
### A couple of changes to [RikiTraki](https://www.rikitraki.com)...

I would like to share some exciting changes I made to [RikiTraki](https://www.rikitraki.com) recently.

First, an invitation code is no longer required in order to create an account on [RikiTraki](https://www.rikitraki.com) so that you can upload your own GPS tracks and associated pictures right away. So essentially it means that we are on open beta now.

<!--more-->

Second, the 3D visualization function has been upgraded to enable track animation, which I like to call virtual drone (vDrone), because the follows the GPS track from beginning to end as if a drone device was filming it from above.

And third, I updated the [RikiTraki](https://www.rikitraki.com) logo to make give the product a little bit of a visual identity.

### Open beta

Now, [RikiTraki](https://www.rikitraki.com) does not require you to get an invitation code in order to register to the site. Instead, you will receive an email that enables you to activate your account before being used. That way, your email is verified, which will allow you to reset your password, in case you forget it. I have updated the [blog post]({{/post/2016-02-01-rikitraki-contrib-1) where I describe the new registration functionality.

### 3D track animation (vDrone)

This is a feature I always wanted to have in [RikiTraki](https://www.rikitraki.com) and finally, I took the time to implement it using the wonderful [CesiumJS'](https://cesiumjs.org/) library and [CZML](https://github.com/AnalyticalGraphicsInc/cesium/wiki/CZML-Guide).

<p align="center">
  <img src="/images/uploads/vDronePic.gif"/>
</p>

To use it, all you have to do is go to the 3D view of a specific track and click on the "Play" control ![RikiTraki](/images/uploads/vDronePlay.png) on the top left part of the visualization area. This action will trigger the animation, which can be paused ![RikiTraki](/images/uploads/vDronePause.png) at any time .

You can control the position of the view camera by just dragging the cursor. Zoom in/out with using the zoom control, or the mouse wheel, or the two-finger gesture on a trackpad.

To speed up the animation, click on the "Faster" ![RikiTraki](/images/uploads/vDroneFaster.png) control and to slow it down, use the "Slower" ![RikiTraki](/images/uploads/vDroneSlower.png) control. Use the "Reset view" ![RikiTraki](/images/uploads/vDroneReset.png) control to start over.

Notice the progress bar at the bottom right part of the screen, which indicates the relative advance of the animation.

![RikiTraki](/images/uploads/vDroneProgress.png)

### RikiTraki logo

Here you go, an Australian outback hat outline with the [RikiTraki](https://www.rikitraki.com) word at the bottom.

<p align="center">
  <img src="/images/uploads/logolarge.png"/>
</p>

The Australian outback hat is a great symbol of the outdoors, and I always wear one when I go out hiking.

<p align="center">
  <img src="/images/uploads/hiking_mthood.jpg"/>
</p>

---

Ok, that's it for now. Ah! and I have an exciting [announcement](/post/2016-03-31-at-foss4gna) to make soon... stay tuned.
