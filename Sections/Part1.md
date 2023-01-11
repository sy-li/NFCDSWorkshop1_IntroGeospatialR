[<<< Previous](../README.md) | [Next >>>](Part2.md)  

## Brief Overview of GIS (Geographic Information Systems)

[What is GIS](#what-is-gis)

[Basic components of GIS](#basic-components-of-gis)

[Two types of spatial data](#two-types-of-spatial-data)


### What is GIS

A Geographic Information System (GIS) is a computerized database management system for capture, storage, retrieval, manipulation, analysis and display of spatial (i.e. locationally defined) data.

In short: **computerized mapping software**

### Basic components of GIS

GIS is composed of layers of spatial information. They can be different types of data, but everything is referenced to a coordinate system (e.g. latitude/longitude).

GIS digitally models the real world using:
- Geometry (e.g. point, line, or polygon)
- Cells in an image
- Data tables

<p align="left">
  <img src="https://github.com/sy-li/NFCDSWorkshop1_IntroGeospatialR/blob/main/Sections/images/1.jpg">
</p>

### Two types of spatial data

1. **Vector data**

Vector data usually is used to represent spatial objects, such as **points**, **lines** and **polygons**. 
Such data consists of a description of the “geometry” or “shape” of the objects, and normally also includes additional variables. 

<p align="left">
  <img src="https://github.com/sy-li/NFCDSWorkshop1_IntroGeospatialR/blob/main/Sections/images/2.png" width="500"/>
</p>

In all cases, the geometry of these data structures consists of sets of coordinate pairs (x, y).

<p align="left">
  <img src="https://github.com/sy-li/NFCDSWorkshop1_IntroGeospatialR/blob/main/Sections/images/3.png" width="500"/>
</p>

And these additional variables are often referred to as “attributes”.

<p align="left">
  <img src="https://github.com/sy-li/NFCDSWorkshop1_IntroGeospatialR/blob/main/Sections/images/4.png" width="500"/>
</p>

Example: represent the same spatial object in different ways

<p align="left">
  <img src="https://github.com/sy-li/NFCDSWorkshop1_IntroGeospatialR/blob/main/Sections/images/5.png">
</p>


2. **Raster data**

Raster data is commonly used to represent spatially continuous phenomena, such as elevation. 
A raster divides the world into a grid of equally sized rectangles (referred to as **cells** or, in the context of satellite remote sensing, **pixels**) 
that all have one or more values (or missing values) for the variables of interest. 
A raster cell value should normally represent the average (or majority) value for the area it covers. 

In contrast to vector data, in raster data the geometry is not explicitly stored as coordinates. 
It is implicitly set by knowing the spatial extent and the number or rows and columns in which the area is divided. 

Example: different applications of raster data

<p align="left">
  <img src="https://github.com/sy-li/NFCDSWorkshop1_IntroGeospatialR/blob/main/Sections/images/6.png" width="800"/>
</p>


**\*Vector vs. Raster**

These two types of spatial data have their own advantages and disadvantages in representing spatial information.

<p align="left">
  <img src="https://github.com/sy-li/NFCDSWorkshop1_IntroGeospatialR/blob/main/Sections/images/7.png" width="700"/>
</p>
