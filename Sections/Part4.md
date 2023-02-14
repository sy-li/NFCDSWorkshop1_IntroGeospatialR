[<<< Previous](Part3.md) | [Next >>>](Part5.md)  

## Data Model II - Raster

[Read raster data](#read-raster-data)

[Raster operations](#raster-operations)

### Read raster data

`terra::rast()` is the main function we use for loading raster data. GeoTIFF is a popular format of raster data. 

The raster data used in this tutorial is 1 km resolution land cover map of South America. Data are integer class values ranging from 1 through 41, with a fill value/missing data value of 0. Land cover class lookup values are be found in data folder.

```diff
sa_lu = rast('data/rasters/lulc.tif')
sa_lu
summary(sa_lu)

# quick plot
plot(sa_lu)
```

### Raster operations

- Subsetting

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

- Local operations

Local operations comprise all cell-by-cell operations. 

**Raster algebra** is a classical example, just like matrix algebra. (These commands are just for illustration as the results don't make sense...)

```
sa_lu + sa_lu
sa_lu^2
log(sa_lu)
sa_lu > 5
```

Another good use case of local operations is **classification**. Here is an example of reclassification based on a two-column matrix "is-becomes":

```diff
# define reclassification matrix
rcl = matrix(c(seq(1,41,1),rep(0,41)),ncol=2) 

# assign value of 1 to all classes of "forest"
rcl[c(1,2,10,11,13,15,16,21,23,29,32,36,40,41),2] = 1 

# reclassify
lu_rcl = classify(sa_lu, rcl = rcl)

plot(lu_rcl)
```

- Focal operations

Focal operations are also called as spatial filtering or convolution. Focal operations take into account a central cell and its neighbors (also named kernel, filter or moving window), which is typically of size 3-by-3 cells but can take on any other shape as defined by the user. A focal operation applies an aggregation function to all cells within the specified neighborhood, uses the corresponding output as the new value for the the central cell, and moves on to the next central cell.

Again, just for illustration purpose...

```
lu_focal = focal(sa_lu, w = matrix(1, nrow = 3, ncol = 3), fun = min)
```

- Zonal operations

Like focal operations, zonal operations apply an aggregation function to multiple raster cells, but the zonal filters are usually a second raster with categorical values. This returns the statistics for each category.

This command returns mean of "forest" and "non-forest" area (doesn't make a lot of sense though...)

```
lu_zonal = zonal(sa_lu, lu_rcl, fun = mean)
```


[<<< Previous](Part3.md) | [Next >>>](Part5.md)  
