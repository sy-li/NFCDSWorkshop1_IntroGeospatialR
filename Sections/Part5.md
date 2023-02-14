[<<< Previous](Part4.md) | [Next >>>](Part6.md)  

## Raster-Vector Interactions

[Raster cropping](#raster-cropping)

[Raster extraction](#raster-extraction)

[Rasterization](#rasterization)

[Spatial vectorization](#spatial-vectorization)


Raster-vector interaction includes four main techniques: raster cropping and masking using vector objects; extracting raster values using different types of vector data; and raster-vector conversion. 

### Raster cropping

`crop(x,y)` reduces the rectangular extent of x based on the extent of y.
```
br_lulc_crop = crop(sa_lu, sa_brazil)

plot(br_lulc_crop)
```

`mask(x,y)` replaces the cells of x outside of the bounds of y with NA.

```
br_lulc_mask = mask(sa_lu, sa_brazil)

plot(br_lulc_mask)
```

In reality, we want to use both `crop()` and `mask()` together in most cases. 
```
br_lulc_final = mask(br_lulc_crop,sa_brazil)

plot(br_lulc_final)
```

### Raster extraction

Raster extractions identify and returning the values associated with a ‘target’ raster at specific locations, based on a (typically vector) geographic ‘selector’. 

```diff
# points as selector
site_lulc = terra::extract(sa_lu, site_sp)
cbind(site_sp,site_lulc)

# polygon as selector
br_lulc = terra::extract(sa_lu,sa_brazil)
br_lulc %>% 
  group_by(ID, lulc) %>%
  count()
```

### Rasterization

We use `rasterize(x,y)` for rasterization. x is vector object to be rasterized, and y is a ‘template raster’ object defining the extent, resolution and CRS of the output. 

```diff
raster_template = rast(ext(amz_sp),res= 0.5,crs = st_crs(amz_sp)$wkt)

# rasterize MAR in Amazon
amz_mar = rasterize(amz_sp,raster_template,field=amz_sp$MAR)
plot(amz_mar)

# rasterize MAT in Amazon
amz_mat = rasterize(amz_sp,raster_template,field=amz_sp$MAT)
plot(amz_mat)
```

### Spatial vectorization
Vectorization converts spatially continuous raster data into spatially discrete vector data such as points, lines or polygons.

```diff
# to points
mar_point = as.points(amz_mar) %>%
  st_as_sf()
  
plot(mar_point)

# to contour lines
mar_contour = as.contour(amz_mar) %>%
  st_as_sf()
  
plot(amz_mar)
plot(mar_contour,add=T)
```

[<<< Previous](Part4.md) | [Next >>>](Part6.md)  
