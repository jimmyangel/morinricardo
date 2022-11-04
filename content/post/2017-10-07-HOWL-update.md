---
title: New HOWL feature - ability to update content
date: 2017-10-07
tags: ["howl", "3d", "cesium", "oregonwild"]
---
### It is now possible to update [HOWL](https://oregonhowl.org) selected content directly from the application

---

Up until now, the data behind [HOWL](https://oregonhowl.org) 'spotlights' has been pre-processed off-line and loaded onto static files. This means that any data changes must be performed by a [HOWL](https://oregonhowl.org) developer and deployed to the web site like a new application release.

<!--more-->

This worked well for the 'spotlights' that were developed before the "[A Sample of Threats to Oregon's Wildlands, Wildlife and Waters](https://oregonhowl.org/?view=wthreats)," because the data did not change very often. For example, the "[History of Wildfire Severity](https://oregonhowl.org/?view=wildfires)" 'spotlight' has to be updated only once a year, and it does require a fair amount of data pre-processing. The other two 'spotlights' ("[Potential Forest Wilderness Areas](https://oregonhowl.org/?view=ecopwilderness)" and "The [Journey of Wolf OR-7](https://oregonhowl.org/?view=or7)") are essentially static.

However, the latest 'spotlight,' [Threats](https://oregonhowl.org/?view=wthreats), can be much more dynamic. Threats need to be added, updated or removed by authorized [Oregon Wild](http://oregonwild.org/) users any time, based on new information that may become available.

The latest [HOWL](https://oregonhowl.org) release now includes the the capability of editing, deleting and adding new items presented on the [Threats](https://oregonhowl.org/?view=wthreats) 'spotlight'.

To make changes to the list, a user must have a Github account and be on the list of collaborators to the [oregonhowl githubd repository](https://github.com/oregonhowl/githubd). We are using [Github](https://github.com/) credentials for authentication and the [Github](https://github.com/) Collaborators feature as our authorization framework. This ensures that only authorized [Oregon Wild](http://oregonwild.org/) users can edit the data that is being presented by the 'spotlight.'

**Activating update mode**

Activating update mode is easy.

Simply right-click (or ctrl-click) on the Info icon (top right circled i)...

![RikiTraki](/images/uploads/activate-update-mode.png)

... and enter your [Github](https://github.com/) credentials.

<p align="center">
	<img src="/images/uploads/activate-update-login.png"/>
</p>

If successful, the (i) icon will turn orange, indicating that you are in update mode.

<p align="center">
	<img src="/images/uploads/orange-icon.png"/>
</p>

Update mode will be only active for the current [HOWL](https://oregonhowl.org) session. If you navigate away from [HOWL](https://oregonhowl.org) in the same browser tab, activation mode will be turned off. Also, if you want to manually deactivate update mode, simply right-click (or control-click) on the orange (i) icon again.

<p align="center">
	<img src="/images/uploads/deactivate-update-mode.png"/>
</p>

Your [Github](https://github.com/) credentials are safe because they are not stored anywhere and are only used to communicate with [Github](https://github.com/).

**Updating an existing entry**

Right-click (or ctrl-click) on an item on the list shown on the info panel...

<p align="center">
	<img src="/images/uploads/right-click-update.png"/>
</p>

...and you will get an update dialog.

<p align="center">
	<img src="/images/uploads/update-dialog.png"/>
</p>

Make changes and click on the "Commit" button to post your changes onto the repository. Changes will be immediately available to users.

To discard changes, click on the close (X) icon on the top right corner of the dialog, or press the "esc" key, or simply click anywhere outside of the dialog box.

**Adding a new item**

Double click directly on a spot on the map where you want the item to appear. Don't worry if it is not exactly on the place you want, you can update the positioning (latitude/long) directly on the dialog box.

<p align="center">
	<img src="/images/uploads/add-dialog.png"/>
</p>

**Bonus functionality**

One nice thing about using [Github](https://github.com/) for this feature is the fact that you can trace every change, who made it, and when. And it is pretty easy to rollback changes to a specific commit point.

<p align="center">
	<img src="/images/uploads/commits.png"/>
</p>

**A note of caution for authorized users**

While it is possible for an authorized user to update the data directly in the [Github](https://github.com/) repository, it is strongly discouraged to avoid corrupting the data or simply entering invalid information without the help of the [HOWL](https://oregonhowl.org) dialogs. Just use [HOWL](https://oregonhowl.org) to update the data -- it is easy and safe.

---

Ok, that's it for now. Hasta la vista amigos!
