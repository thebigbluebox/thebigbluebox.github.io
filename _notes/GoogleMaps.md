---
title:  "Google Maps"
date:   2016-5-4 9:00:00
description: The most popular map api in the world

categories:
 - category: "web"

technologies:
 - name: "Google Maps JS"
---
# Google Maps API

# Quick Start
Import the Maps api
Create a new map object by map = new google.maps.Map(selector for where element going to be, json of where the map center should be})
On the bottom there should be as api key and call back for the map

## Map Options
* Within the map options, we have center -> which is the lat and lng of the centre at which you want to show at the beginning of the map.
* Zoom level which is the level at which the map is zoomed into, the level goes from
  1. 1: World
  2. 5: Landmass continent
  3. 10: City
  4. 15: Streets
  5. 20: Buildings

## Drawing on the Map
* Single locations on the map are displayed using markers. Markers may sometimes display custom icon images in whcich case they are usually referred to as "icons"
* An info window is a special kind of overlay for displaying content (usually text or images) within a popup ballon at a given location on a map
* Lines on the map are disaplyed using polylines representing an ordered sequence of locations
* Areas of arbitrary shape on the map are displayed using polygons. Like polylines, polygons are an ordered sequence of locations. 
* You can define circles or rectangles on the map

## Markers
Markers are designed to be interactive. For example, by default they receive 'click' events, so you can add an event listener to bring up an info window displaying custom information. You can allow users to move a marker on the map by setting the marker's draggable property to true.

* Draggable to true to allow the dragging of markers
* To create a new marker on the map by creating new google.maps.Marker({position:{lat,lng}, map: mapobject, title: 'hello world'});
* Setting markers after the initalizing of a marker can be done by not setting the map option in the marker creation, then maker.setMap(map)
* Removing a marker can be done by marker.setMap(null)
  * If you wish to manage a set of markers, you should create an array to hold the markers. Using this array you can then call setMap() on each marker in the array in turn when you need to remove the markers. You can delete the markers by removing them from the map and then setting the array's length to 0 -> which removes all references to the markers
  * You can create an array of markers = []
  * And each click event will be able to get the event.latlng to retrieve the location of the click event
* Customizing a marker image
  * Create a link to the image or custom icon
  * Then when the creation of the marker is happening, link the image to the marker using the icon: parameter
* Adding a info panel
  * A info panel is set by assigning each markers a template to the marker and adding a listener to listen for when the marker is pressed. 
  * The InfoWindow object has many properties
  * Content Main content of the info window, formated in HTML
  * PixelOffset
  * Position
  * MaxWidth how big the info window should be set by the Maxwidth parameter