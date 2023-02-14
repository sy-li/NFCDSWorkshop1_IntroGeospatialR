[<<< Previous](Part6.md) | [Homepage](../README.md)

## Part 7: Resources

To further explore or troubleshoot issues in using R for geospatial analysis, consider taking advantage of these resources:

- [Simple Features for R](https://r-spatial.github.io/sf/) - a tutorial for `sf` package.

- [Spatial Data Science with R (terra version)](https://rspatial.org/) - great thorough tutorial for spatial data analysis and modeling with R. 

   Note: There is an old [raster/sp version](https://rspatial.org/raster/index.html) available as well but the `terra` version is recommended, as `sp` package will be retired by the end of 2023 (and `raster` relies on `sp`).

- [ND Udemy](https://nd.udemy.com/) - a website offering thousands of professional development and training videos, freely accessible by all Notre Dame students.

- [Stack Exchange](https://gis.stackexchange.com/) - excellent source for GIS-related troubleshooting.



### Exercise Answer Key

```diff
# convert dataframe to simple feature
amz_agb_sp = amz_agb %>%
  st_as_sf(coords = c('lon','lat'),crs = st_crs(4326)) 

# rasterization
raster_template = rast(ext(amz_agb_sp),res= 0.5,crs = st_crs(amz_agb_sp)$wkt)
amz_agb_rast = rasterize(amz_agb_sp,raster_template,field=amz_agb_sp$agb)

# plot
plot(amz_agb_rast)
```

[<<< Previous](Part6.md) | [Homepage](../README.md)
