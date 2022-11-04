---
title: Pick location (lat/long) when updating content
date: 2017-10-22
tags: ["howl", "3d", "cesium", "oregonwild"]
---
### It is now possible to pick a location when updating content

---

Adding a new item to the [Threats](https://oregonhowl.org/?view=wthreats) 'spotlight' grabs the latitude and longitude of the double click position on the map, so the values do not need to be explicitly known. However, sometimes the location of an item needs to be updated, but entering latitude and longitude directly on the form is not very friendly when the coordinates are not known in advance.

Now, latitude and longitude can be picked from the map when updating an item. Simply click on the marker icon displayed on the form.

<!--more-->

<p align="center">
	<img src="/images/uploads/update-form-pick-icon.png"/>
</p>

The form will be temporarily closed to allow the user to find the location directly on the map.

<p align="center">
	<img src="/images/uploads/pick-location.png"/>
</p>

Just double-click and the latitude and longitude pointed by the bottom of the marker will be brought back into the update form. After making any other changes, click on the 'Commit' button to update the repository.

And voilà, easy location picking!
