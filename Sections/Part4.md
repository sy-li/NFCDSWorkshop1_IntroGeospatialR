[<<< Previous](Part3.md) | [Next >>>](Part5.md)  

## Data Model II - Raster

[Read raster data](#read-raster-data)

[Raster operations](#raster-operations)
- [Subsetting](#subsetting)
- [Local operations](#local-operations)
- [Focal operations](#focal-operations)
- [Zonal operations](#zonal-operations)

### Read raster data

`terra::rast()` is the main function we use for loading raster data. GeoTIFF is a popular format of raster data. 

The raster data used in this tutorial includes 1 km resolution land cover map and 30 arc-second DEM (Digital Elevation Model)of South America. Land cover data are integer class values ranging from 1 through 41, with a fill value/missing data value of 0. Land cover class lookup values are be found in data folder. A Digital Elevation Model (DEM) is a representation of the bare ground topographic surface of the Earth excluding trees, buildings, and any other surface objects. This elevation data is spaced at 30-arc seconds (approximately 1 kilometer).

```diff
sa_lu = rast('data/rasters/lulc.tif')
sa_lu
summary(sa_lu)

# quick plot
plot(sa_lu)

# same for elevation data
sa_dem = rast('data/rasters/samer_dem.tif')
sa_dem
plot(sa_dem)
```

### Raster operations

#### Subsetting

A common case of subsetting is using a raster with logical (or NA) values to mask another raster with the same extent and resolution. The code chunck below demonstrates how to subset the cells with value 1 - Tropical moist and semi-deciduous forest.

```diff
# create raster mask
rmask = sa_lu

# we only want 1-Tropical moist and semi-deciduous forest
rmask[rmask != 1] = NA

# spatial subsetting
sa_forest = mask(sa_lu, rmask)  

plot(sa_forest,col='forestgreen')
```

#### Local operations

Local operations comprise all cell-by-cell operations. 

1. Raster algebra

Raster algebra is a classical example, just like matrix algebra. (These commands are just for illustration purpose)

```
sa_lu + sa_lu
sa_lu^2
log(sa_lu)
sa_lu > 5
```

2. Classification

Another good use case of local operations is reclassification. 

Here is an example of reclassification based on a two-column matrix "**is-becomes**":

```diff
# define reclassification matrix
rcl = matrix(c(seq(1,41,1),rep(0,41)),ncol=2) 

# assign value of 1 to all classes of "forest"
rcl[c(1,2,10,11,13,15,16,21,23,29,32,36,40,41),2] = 1 

# reclassify
lu_rcl = classify(sa_lu, rcl = rcl)

plot(lu_rcl)
```
Another example of relassification based on a three-column matrix "**from-to-becomes**":

```diff
# define reclassification matrix
m = c(-200, 0, 0,
       0., 200, 1,
       200, 7000, 2)
       
# below sealevel, plain, and plateau
rclmat = matrix(m, ncol=3, byrow=TRUE)
dem_rcl = classify(sa_dem, rclmat, include.lowest=TRUE)

plot(dem_rcl)
```

#### Focal operations

Focal operations are also called as spatial filtering or convolution. Focal operations take into account a central cell and its neighbors (also named kernel, filter or moving window), which is typically of size 3-by-3 cells but can take on any other shape as defined by the user. A focal operation applies an aggregation function to all cells within the specified neighborhood, uses the corresponding output as the new value for the the central cell, and moves on to the next central cell.

```
dem_focal = focal(sa_dem, w = matrix(1, nrow = 3, ncol = 3), fun = mean)
plot(dem_focal) 
```

Note that focal output is a **raster**.

#### Zonal operations

Like focal operations, zonal operations apply an aggregation function to multiple raster cells, but the zonal filters are usually a second raster with categorical values. This returns the statistics for each category.

```diff
# Mean of below sealevel, plain, and plateau area 
dem_zonal = zonal(sa_dem, dem_rcl, fun = mean)
dem_zonal 
```
Note that zonal output is a **dataframe**.


[<<< Previous](Part3.md) | [Next >>>](Part5.md)  
