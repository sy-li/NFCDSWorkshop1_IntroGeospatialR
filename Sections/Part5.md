[<<< Previous](Part4.md) | [Next >>>](Part6.md)  

## Geospatial R - Rasters

[Crop, masking, and trimming](#crop-masking-and-trimming)

[Extracting data from rasters](#extracting-data-from-rasters)

[Assignment 2](#assignment-2)

Now we have some watershed outlines and maybe we want to figure out how much mining there was each year in these watersheds. 
In the raster folder we have annual mining extent for the mud river as rasters (as .tifs). 
So let's play with the raster library and see what this data looks like. 

First, see what mining looks like from 1999
```diff
mine.99 = rast('data/rasters/mining_1999_1014.tif')

# Plot the data
plot(mine.99)
plot(mud.river,add=T)
```

### Crop, masking, and trimming

Let's look just at stanley fork. 
```
stan.mine = crop(mine.99,stanley.match)
plot(stan.mine, main='Cropped to extent only')
plot(stanley.match,add=T)
```
But that only cropped the data to the extent of stanley fork watershed. 
```diff
mask(stan.mine,stanley.match) %>% 
  plot(., main='Cropped to watershed outline') 
# The dot just retrieves the previous object fed from the pipe!
plot(stanley.match,add=T)
```
We can trim this raster even more though with the trim command
```
mask(stan.mine,stanley.match) %>% 
  trim(.) %>%
  plot(., main='Cropped and trimmed to watershed outline')
plot(stanley.match,add=T)
```

### Summarising raster data

What is the area of this watershed?
```diff
stan.area = expanse(stanley.match) 
# this is in m2, km2 makes more sense
stan.area = expanse(stanley.match)/(1000*1000)
```
Mask stan.mine again
```
stan.shed = mask(stan.mine,stanley.match)
```
How many cells are in stan.shed? 
```
ncell(stan.shed)
```
But this includes a bunch of NAs so we can't use it to calculate area. 

Let's set NAs to 0
```
stan.shed[is.na(stan.shed)] = 0
```
This works because each cell is actually the area. 900m2 so we can just sum up the non-zero cells. 
```
tote99 = sum(as.matrix(stan.shed))/(1000*1000)

tote99/stan.area
```

### Extracting data from rasters 

We have seen that 83% of the watershed was undergoing mining in 1999. 
What about % active mining for every year of the watershed? 
Here we can use a loop to answer this question. 

```
library(stringr)
```
To make sure I get the years right, the best practice is to make a vector that lists the year from the .tif raster name. 
```
rast.name = list.files('data/rasters')
years = str_split_fixed(rast.name,'_',3) # Splits string into 3 column data frame
head(years)
```
All we want is the second column
```
years = as.numeric(years[,2])
```
Now let's setup a director to grab all rasters
```
rast.files = paste('data/rasters',rast.name,sep='/')
```
Setup an empty data frame to hold the results of our loop
```
stan.ann.mine = data.frame(year=numeric(),mining=numeric(),pmining=numeric())
```
Practice loop, good idea when building a loop that may take some time to run
```diff
i = 1
# Read in our raster data 
mining.yr = rast(rast.files[i])
# Trim it to stanely fork by first cropping it and then masking it
stan.mining.yr = crop(mining.yr, stanley.match) %>% mask(.,stanley.match)
# Set NAs to 0
stan.mining.yr[is.na(stan.mining.yr)] = 0
# Sum cells
tote.yr = sum(as.matrix(stan.mining.yr))/(1000*1000) #km2
# Get percent.
tote.p = tote.yr/stan.area
# Put data into data frame (i = the row where data will go)
stan.ann.mine[i,] = c(years[i],tote.yr,tote.p)
stan.ann.mine 
# looks good
```
Now all we have to do is put this into a loop
```diff
for(i in 1:length(rast.files)){
# Read in our raster data 
  mining.yr = rast(rast.files[i])
# Trim it to stanely fork by first cropping it and then masking it
  stan.mining.yr = crop(mining.yr, stanley.match) %>% mask(.,stanley.match)
# Let's set NAs to 0
  stan.mining.yr[is.na(stan.mining.yr)] = 0
# Sum cells and convert to km2
  tote.yr = sum(as.matrix(stan.mining.yr))/(1000*1000) 
# Get percent.
  tote.p = tote.yr/stan.area
# Put data into data frame (i = the row where data will go)
  stan.ann.mine[i,] = as.numeric(c(years[i],tote.yr,tote.p))
}
```

Plot our data.
```
ggplot(stan.ann.mine,aes(x=year,y=mining)) + geom_point() +
  ylab(expression(paste('Active Mining (',km^2,')',sep='')))
```


### Assignment 2

Make the same plot with mining as percent mining!
