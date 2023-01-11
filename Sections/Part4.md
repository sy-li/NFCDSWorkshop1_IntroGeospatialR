[<<< Previous](Part3.md) | [Next >>>](Part5.md)  

## Geospatial R - Vectors

Now with basic understanding of how to use R, let's make it spatial!

First, we need to load in our key library.

```
library(terra)
```
A warning might pop up here saying that `extract are masked from 'package:magrittr'`, 
meaning when we use these commands R will default to using the terra version of extract, not the magrittr version. 
So if we want to use magrittr version we have to do a python like thing: `magrittr::extract`


For this exercise we will be using watershed outlines from two watersheds. First we need to read these shapefiles in and then we can play around with them. 

To read in the data we will use readOGR from the rgdal package, like this ` readOGR('path/to/the/folder', shapefile)`

```
mud.river = vect('data/shapefiles','mud')
stanley.fork = vect('data/shapefiles','stanley')
```
Print information on the shapefile to check
```
mud.river
stanley.fork
```
And plot them
```
plot(mud.river)
plot(stanley.fork,add=T)
```

Apparently we need to fix the fact that stanley fork, which is a watershed inside of the mud river watershed, is not plotting. 
Look at the coord output to see why.
```
crs(mud.river)
crs(stanley.fork)
```
Our shapefiles don't have matching projections. To fix:
```
stanley.match = terra::project(stanley.fork, crs(mud.river))

plot(mud.river)
plot(stanley.match,add=T,col='blue')

```
