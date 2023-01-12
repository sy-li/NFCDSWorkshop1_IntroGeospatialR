[<<< Previous](Part5.md) | [Homepage](../README.md)

## Part 6: Resources

To further explore or troubleshoot issues in using R for geospatial analysis, consider taking advantage of these resources:

- [Spatial Data Science with R (terra version)](https://rspatial.org/) - great thorough tutorial for spatial data analysis and modeling with R. 

   Note: There is an old [raster/sp version](https://rspatial.org/raster/index.html) available as well but the `terra` version is recommended, as `sp` package will be retired by the end of 2023 (and `raster` relies on `sp`).

- [ND Udemy](https://nd.udemy.com/) - a website offering thousands of professional development and training videos, freely accessible by all Notre Dame students.

- [Stack Exchange](https://gis.stackexchange.com/) - excellent source for GIS-related troubleshooting.



### Exercise 1&2 Answer Key

Exercise 1

```
ggplot(p.mining,aes(x=year,y=percent,fill=state)) + geom_area(position='stack')
```

Exercise 2

```
ggplot(stan.ann.mine,aes(x=year,y=pmining)) + geom_point() + ylab('Active Percent Mining')
```
