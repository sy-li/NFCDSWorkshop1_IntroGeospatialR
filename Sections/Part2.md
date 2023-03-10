[<<< Previous](Part1.md) | [Next >>>](Part3.md)  

## Bridging R with GIS

Before we jump into geospatial analysis in R, let's review general properties of R itself. 


### What is R? 

R is a versatile statistical programming language, but it can also be used for geospatial analysis, making interactive data visualizations, and, with the help of RStudio, it can even be used to write papers. It is definitely a powerful and flexible tool.

### Why use R for geospatial analysis?

- Free, and open source
- Adding more functionalities through an entire ecosystem of packages
- Scripting makes work inherently reproducible
- RMarkdown is a wonderful way to share and comment code
- Github integration makes it easy to collaborate on projects
- Large and intuitive plotting library 

### Comparison between R and traditional GIS software

- Traditional GIS software (such as ArcGIS, QGIS,GRASS, etc.) allow to visually prepare a map in the same approach as one would prepare a poster or a document layout.
- R allows to compute statistical data and create graphs and maps in the same programmable language.
- Traditional GIS software implement a WYSIWIG approach (“What You See Is What You Get”)
- R implements a WYSIWYM approach (“What You See Is What You Mean”)

### Environment setting

Before working on real dataset, let's first install essential R packages. Please copy and paste the code block below into your RStudio console and run it.

```diff
packages.to.install = c(
# handle spatial vector data
'sf'        
# handle spatial raster data
'terra'    
# a standardized way to manage dataset
'tidyverse'   
)

install.packages(packages.to.install,repos = 'https://cloud.r-project.org')
```

### Download data

Please download the [data](https://drive.google.com/file/d/1D_zVgmR0dMP9Uc33B4BRlHdJA--OmcL1/view?usp=share_link) and unzip it into the same folder with your R script.

[<<< Previous](Part1.md) | [Next >>>](Part3.md)  
