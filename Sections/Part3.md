[<<< Previous](Part2.md) | [Next >>>](Part4.md)  

## Data Model I - Vector

[Read vector data](#read-vector-data)

[Vector operations](#vector-operations)
- [Spatial join](#spatial-join)
- [Spatial subsetting](#spatial-subsetting)
- [Spatial aggregation](#spatial-aggregation)

### Read vector data

`sf::read_sf()` is the main function we use for loading vector data. We can import shapefiles, a popular format of vector data, using this command. In fact, `read_rf` returns data as a tidyverse "tibble". (If you are not familiar with "tidy data", check out this [tutorial](https://cran.r-project.org/web/packages/tidyr/vignettes/tidy-data.html).)

We will first import a shapefile contains country boundaries in South America.

```diff
# read in shapefile 
sa_regions = read_sf("data/vectors/South_America.shp")

# check the basic metadata and the first a few lines
head(sa_regions)

# st_crs gives us detailed information about coordinate system
st_crs(sa_regions)

# basic maps are created in sf with plot()
plot(sa_regions)  
```

Spatial data in `sf` can be subset just like a dataframe. For example, we want to take a close look at Brazil.

```diff
# subsetting with dataframe syntax
sa_brazil = sa_regions[sa_regions$COUNTRY == "Brazil",]

# combines them into a single feature
sa_amazon = sa_regions[sa_regions$COUNTRY == "Brazil"|sa_regions$COUNTRY == "Bolivia"|
                      sa_regions$COUNTRY == "Colombia"|sa_regions$COUNTRY == "Ecuador"|
                      sa_regions$COUNTRY == "Guyana"|sa_regions$COUNTRY == "Peru"|
                      sa_regions$COUNTRY == "Suriname"|sa_regions$COUNTRY == "Venezuela"|
                      sa_regions$COUNTRY == "French Guiana (France)",]
amazon = st_union(sa_amazon)

# now plot Brazil over a map of South America, and see difference before and after union
plot(sa_regions,reset = FALSE)  
plot(amazon, add = TRUE, col = "red")
plot(sa_amazon, add = TRUE, col = "red")
```

Vector data can also be imported from tables. `st_as_sf()` function can transform lat/lon into geometry. (Learn more about the language of geomotry: [WKT](https://en.wikipedia.org/wiki/Well-known_text_representation_of_geometry))

```diff
# read data from inventory sites
site_df = read.csv('data/tables/AMZ_site.csv')

# transform into a vector by specifying coordinates and CRS
site_sp = site_df%>%
  st_as_sf(coords = c('lon','lat'),crs = st_crs(4326)) 
# 4326 stands for World Geodetic System 1984
  
# By default this creates a multi-panel plot, one sub-plot for each variable of the object
plot(site_sp)

# fancy map using ggplot
ggplot()+
  geom_sf(data = sa_regions)+
  geom_sf(data = site_sp,mapping = aes(size=agb))+
  coord_sf(xlim = c(-80, -45), ylim = c(-20, 10), expand = T)
```

Similarly, we import regional meteorological data.

```diff
amz_df = read.csv('data/tables/AMZ_region.csv')

amz_df = na.omit(amz_df)

amz_sp = amz_df%>%
  st_as_sf(coords = c('lon','lat'),crs = st_crs(4326))
  
plot(amz_sp)
```


### Vector operations

#### Spatial join

Joining two non-spatial datasets relies on a shared index. Spatial data joining applies the same concept, but instead relies on spatial relations. We use `st_join(x,y)` to add new columns to the target object (x) from a source object (y).

For example, this command gives each site a new attribute: country.

```diff
site_joined = st_join(site_sp, sa_regions) 
site_joined
```

#### Spatial subsetting

Spatial subsetting returns a new object containing only features that meet certain criteria. It can be implemented in several ways:

```diff
# like attribute subsetting
site_br = site_joined[site_joined$COUNTRY=='Brazil',]

# using st_intersects()
site_br = site_sp[sa_brazil,,op=st_intersects]

# using st_filter()
site_br = site_sp %>%
  st_filter(y = sa_brazil, .predicate = st_intersects)
  
# check each of them, we can find they are equivalent
ggplot()+
  geom_sf(data = sa_regions)+
  geom_sf(data = site_br,mapping = aes(size=agb))+
  coord_sf(xlim = c(-80, -45), ylim = c(-20, 10), expand = T)
```

#### Spatial aggregation

Spatial data aggregation condenses data. It "summarise" multiple values of a variable based on specified function and return a single value per grouping variable. 

```diff
# These two methods return the same results.
site_mean = aggregate(x = site_sp, by = sa_regions, FUN = mean) %>% 
  na.omit()
  
site_mean = st_join(x = site_sp, y = sa_regions) %>%
  group_by(COUNTRY) %>%
  summarize(agb = mean(agb, na.rm = TRUE))
```


[<<< Previous](Part2.md) | [Next >>>](Part4.md)  
