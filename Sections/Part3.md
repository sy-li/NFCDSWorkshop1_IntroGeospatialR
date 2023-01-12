[<<< Previous](Part2.md) | [Next >>>](Part4.md)  

## Tricks beyond Basic R

[Tidy data and pipe](#tidy-data-and-pipe)

[Plotting](#plotting)

[Simple regression](#simple-regression)

[Assignment 1](#assignment-1)

Like Python R is a object-oriented scripting language, which basically means that you can assign objects to have specific features. Unlike other programming languages, R scripts are small and the syntax is not complicate, which make R generally easier to learn and use. However, like any language, R takes hours to learn the basics and a lifetime to master. 

I will not start from R 101 (if you need basic introduction to R, I suggest you to take [R Programming - R Language for Absolute Beginners](https://nd.udemy.com/course/r-for-absolute-beginners/) from ND Udemy). Instead, I'll start with something not terribly complicated but surprisingly useful.


### Tidy data and pipe

Tidy datasets provide a standardized way to link the structure of a dataset (its physical layout) with its semantics (its meaning). A standard makes initial data cleaning easier because you don’t need to start from scratch and reinvent the wheel every time. In tidy data:

1. Every column is a variable.
2. Every row is an observation.
3. Every cell is a single value.

A typical journey from raw data to results might involve many steps, such as filtering cases, transforming values, summarising data, and then running a statistical test. But one operator can link all these steps together, while keeping our code efficient and readable, and that is "pipe". The pipe operator, written as `%>%`, takes the output of one function and passes it into another function as an argument. This allows us to link a sequence of analysis steps.

First, read in tabular data in the data folder. 
```diff
mining = read.csv('data/CumeMine.csv',stringsAsFactors=F)
# The second argument is used to prevent R to call strings factors
```
What does this data look like?
```
head(mining)
```
Headers look like state names with some empty columns that have notes in them.
```
names(mining)
```
Second column looks like total areas by state. 
Let's extract that column and put it in a different dataset.
Have a close look by grabbing the first row and columns 2-5
```
tote.areas = mining[1,2:5]
```
Check
```
str(tote.areas)
```
Problem found - those are characters not numbers.
```
as.numeric(tote.areas)
```
Still doesn't work, as the commas need to be removed.
```
tote.area.no.comma = gsub(pattern=',',replacement='',x=tote.areas)
```
Now it can be converted to a numeric. 
```
tote.area.num = as.numeric(tote.area.no.comma)
```
But the state names need to be binded back together.
```
total.areas = data.frame(state=names(mining)[2:5],area=tote.area.num,stringsAsFactors = F)
total.areas
```
Now let's fix the original data frame using package called `tidyr` `dplyr` and `magrittr`
```diff
# A great library of data filtering tools
library(dplyr) 
# Easiest way to make data tidy
library(tidyr) 
# Adds pipe functionality
library(magrittr) 
```
Reread in data and skip first row this time. And keep only first five columns
```
mine.dat = read.csv('data/CumeMine.csv',stringsAsFactors=F,skip=2,header=F)[1:5]
```
Rename columns
```
names(mine.dat) = c('year','kentucky','tennessee','virginia','west.virginia')
```
Convert to a tidy dataset by 'gathering' data
```
annual.mining = gather(mine.dat,key=state,value=mining,-year) %>% 
  select(state,year,mining) %>% arrange(year) 
  
head(annual.mining)
```
Now we have tidy data!


### Plotting

With our tidy dataset in hand, plotting is easy. We can look at data in so many different ways. 

How do mining rates differ over time and between states? Let's use ggplot2 and find out!
```
library(ggplot2)

glines = ggplot(annual.mining,aes(x=year,y=mining,color=state)) + geom_line()
glines
```
What about cumulative mining extent?
```
gstack = ggplot(annual.mining,aes(x=year,y=mining,fill=state)) + geom_area(position='stack')
gstack
```
In fact, we can reorder the stacking position by using factors.
```
annual.mining$States = factor(annual.mining$state,levels=c('tennessee','virginia','west.virginia','kentucky'))
```
And pipes too!
```
gstack1 = arrange(annual.mining,States) %>% ggplot(aes(x=year,y=mining,fill=States)) + geom_area(position='stack')
gstack1
```

### Simple regression

What about the correlation between mining in West Virginia and Kentucky?

This time our data is not in the right structure to immediately look at this correlation but that is easy to fix using the command "spread" from `tidyr` package

First remove factor version of states
```
spread.mining = select(annual.mining,-States) %>% 
  spread(key=state,value=mining)
```
Back to the original structure
```
head(spread.mining)

ky.wv = ggplot(spread.mining,aes(x=kentucky,y=west.virginia)) + geom_point()
ky.wv
```
We can even easily add a linear model to this data
```
ky.wv1 = ggplot(spread.mining,aes(x=kentucky,y=west.virginia,label=year)) + 
  geom_smooth(method='lm') +
  geom_point() 
ky.wv1
```
And print the model summary
```
mine.model = lm(west.virginia~kentucky,data=spread.mining)
summary(mine.model)
```
Looks like for every 1 square meter mined in KY there is 0.77 m2 mined in WV. 

We can easily add interactivity to these plots using `plotly` library ()
```
library(plotly)
ggplotly(ky.wv1)
```


###  Assignment 1
Now you have enough knowledge to create your own plots. Let's try to plot cumulative mining as a percentage of total area. 

```diff
# First we need to join the total area data with the annual mining extent data.
p.mining = left_join(annual.mining,total.areas,by='state') %>% 

# Then add a column dividing mining extent by total area
mutate(percent = mining/area)
head(p.mining)

# Now you make a plot here!


```
