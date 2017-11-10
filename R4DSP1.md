---
title: "R4DS Learning Notes Part 1"
output:
  pdf_document: default
  html_notebook: default
  html_document: default
  word_document: default
---
<font size=4>

##Introduction

Data exploration is the art of looking at your data, rapidly generating hypotheses, quickly testing them, then repeating again and again and again.  

![](http://r4ds.had.co.nz/diagrams/data-science-explore.png)

##Data Visualization

> ¡°The simple graph has brought more information to the data analyst¡¯s mind than any other device.¡± ¡ª John Tukey

###Create ggplot


```r
library(tidyverse)
mpg
```

```
## # A tibble: 234 x 11
##    manufacturer      model displ  year   cyl      trans   drv   cty   hwy
##           <chr>      <chr> <dbl> <int> <int>      <chr> <chr> <int> <int>
##  1         audi         a4   1.8  1999     4   auto(l5)     f    18    29
##  2         audi         a4   1.8  1999     4 manual(m5)     f    21    29
##  3         audi         a4   2.0  2008     4 manual(m6)     f    20    31
##  4         audi         a4   2.0  2008     4   auto(av)     f    21    30
##  5         audi         a4   2.8  1999     6   auto(l5)     f    16    26
##  6         audi         a4   2.8  1999     6 manual(m5)     f    18    26
##  7         audi         a4   3.1  2008     6   auto(av)     f    18    27
##  8         audi a4 quattro   1.8  1999     4 manual(m5)     4    18    26
##  9         audi a4 quattro   1.8  1999     4   auto(l5)     4    16    25
## 10         audi a4 quattro   2.0  2008     4 manual(m6)     4    20    28
## # ... with 224 more rows, and 2 more variables: fl <chr>, class <chr>
```

`ggplot()` creats a coordinate system that we can add layers to. To complete the graph we add one or more layers to `ggplot()` . The function `geom_point()` adds a layer of points.  


```r
ggplot(data=mpg)+geom_point(mapping=aes(x=displ,y=hwy))
```

![plot of chunk unnamed-chunk-2](figure/unnamed-chunk-2-1.png)

When use `aes()`, fiil or color inside `aes()` will be grouped


```r
p  <- ggplot(data=diamonds, aes(x=carat, y=price))
p  + geom_boxplot(aes(x=cut, fill=cut)) +
  scale_fill_manual(values=rep("cyan", length(levels(diamonds$cut))))
```

![plot of chunk unnamed-chunk-3](figure/unnamed-chunk-3-1.png)

```r
p  + geom_boxplot(aes(x=cut), fill="cyan")
```

![plot of chunk unnamed-chunk-3](figure/unnamed-chunk-3-2.png)

### A Graphing Template  


```r
#ggplot(data = <DATA>) + 
#  <GEOM_FUNCTION>(mapping = aes(<MAPPINGS>))
```

> ¡°The greatest value of a picture is when it forces us to notice what we 
> never expected to see.¡± ¡ª John Tukey

To convey information about our data by mapping the aesthetics in our plot to variables in our dataset.


```r
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy, colour = class))
```

![plot of chunk unnamed-chunk-5](figure/unnamed-chunk-5-1.png)

To map an aesthetic to a variable, associate the name of the aesthetic to the name of the variable inside `aes()`. ggplot2 will automatically assign a unique level of the aesthetic (here a unique color) to each unique value of the variable, a process known as **scaling**. ggplot2 will also add a legend that explains which levels correspond to which values.

Now we try to map `class` to `size`,`alpha`(control the tranaparency of the points) and `shape`.


```r
#first size
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy, size = class))
```

```
## Warning: Using size for a discrete variable is not advised.
```

![plot of chunk unnamed-chunk-6](figure/unnamed-chunk-6-1.png)

```r
#second alpha
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy, alpha = class))
```

![plot of chunk unnamed-chunk-6](figure/unnamed-chunk-6-2.png)

```r
#third shape
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy, shape = class))
```

```
## Warning: The shape palette can deal with a maximum of 6 discrete values
## because more than 6 becomes difficult to discriminate; you have 7.
## Consider specifying shapes manually if you must have them.
```

```
## Warning: Removed 62 rows containing missing values (geom_point).
```

![plot of chunk unnamed-chunk-6](figure/unnamed-chunk-6-3.png)

We can also set the aesthetic properties of our geom manually. We set the aesthetic by name as an argument of your geom function; i.e. it goes outside of `aes()`. We¡¯ll need to pick a value that makes sense for that aesthetic:

* The name of a color as a character string.
* The size of a point in mm.
* The shape of a point as a number, as shown in the Figure.
![](http://r4ds.had.co.nz/visualize_files/figure-html/shapes-1.png)

One common problem when creating ggplot2 graphics is to put the `+`in the wrong place: it has to come at the end of the line, not the start.


```r
#wrong example
#ggplot(data = mpg) 
#+ geom_point(mapping = aes(x = displ, y = hwy))
#right example
ggplot(data = mpg) +
geom_point(mapping = aes(x = displ, y = hwy))
```

![plot of chunk unnamed-chunk-7](figure/unnamed-chunk-7-1.png)

###Facet 

One way to add additional variables is with aesthetics. Another way, particularly useful for categorical variables, is to split our plot into facets, subplots that each display one subset of the data.

To facet our plot by a single variable, use `facet_wrap()`. The first argument of `facet_wrap()` should be a formula, which we create with `~` followed by a variable name. The variable that we pass to `facet_wrap()` should be discrete.


```r
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy)) + 
  facet_wrap(~ class, nrow = 2)
```

![plot of chunk unnamed-chunk-8](figure/unnamed-chunk-8-1.png)

To facet our plot on the combination of two variables, add `facet_grid()` to our plot call. The first argument of `facet_grid()` is also a formula. This time the formula should contain two variable names separated by a `~`.


```r
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy)) + 
  facet_grid(drv ~ cyl)
```

![plot of chunk unnamed-chunk-9](figure/unnamed-chunk-9-1.png)

If we want to not facet in the rows or columns dimension, use a `.` instead of a variable name, e.g. `+ facet_grid(. ~ cyl)`.


```r
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy)) + 
  facet_grid(cyl~.)
```

![plot of chunk unnamed-chunk-10](figure/unnamed-chunk-10-1.png)

###Geometric Objects
A **geom** is the geometrical object that a plot uses to represent data. To change the geom in our plot, change the geom function that we add to `ggplot()`.


```r
# left
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy))
```

![plot of chunk unnamed-chunk-11](figure/unnamed-chunk-11-1.png)

```r
# right
ggplot(data = mpg) + 
  geom_smooth(mapping = aes(x = displ, y = hwy))
```

```
## `geom_smooth()` using method = 'loess'
```

![plot of chunk unnamed-chunk-11](figure/unnamed-chunk-11-2.png)

ggplot2 provides over 30 geoms, and extension packages provide even more (see https://www.ggplot2-exts.org for a sampling). The best way to get a comprehensive overview is the ggplot2 cheatsheet, which we can find at http://rstudio.com/cheatsheets. 

Many geoms, like `geom_smooth()`, use a single geometric object to display multiple rows of data. For these geoms, we can set the `group` aesthetic to a categorical variable to draw multiple objects. ggplot2 will draw a separate object for each unique value of the grouping variable. In practice, ggplot2 will automatically group the data for these geoms whenever you map an aesthetic to a discrete variable (as in the `linetype` example). 


```r
ggplot(data = mpg) +
  geom_smooth(mapping = aes(x = displ, y = hwy))
```

```
## `geom_smooth()` using method = 'loess'
```

![plot of chunk unnamed-chunk-12](figure/unnamed-chunk-12-1.png)

```r
ggplot(data = mpg) +
  geom_smooth(mapping = aes(x = displ, y = hwy, group = drv))
```

```
## `geom_smooth()` using method = 'loess'
```

![plot of chunk unnamed-chunk-12](figure/unnamed-chunk-12-2.png)

```r
ggplot(data = mpg) +
  geom_smooth(
    mapping = aes(x = displ, y = hwy, color = drv),
    show.legend = FALSE
  )
```

```
## `geom_smooth()` using method = 'loess'
```

![plot of chunk unnamed-chunk-12](figure/unnamed-chunk-12-3.png)

To display multiple geoms in the same plot, add multiple geom functions to `ggplot()`:


```r
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy)) +
  geom_smooth(mapping = aes(x = displ, y = hwy))
```

```
## `geom_smooth()` using method = 'loess'
```

![plot of chunk unnamed-chunk-13](figure/unnamed-chunk-13-1.png)

```r
# more convinient way to change x,y axis
ggplot(data = mpg, mapping = aes(x = displ, y = hwy)) + 
  geom_point() + 
  geom_smooth()
```

```
## `geom_smooth()` using method = 'loess'
```

![plot of chunk unnamed-chunk-13](figure/unnamed-chunk-13-2.png)

If we place mappings in a geom function, ggplot2 will treat them as local mappings for the layer. It will use these mappings to extend or overwrite the global mappings for that layer only. This makes it possible to display different aesthetics in different layers.


```r
ggplot(data = mpg, mapping = aes(x = displ, y = hwy)) + 
  geom_point(mapping = aes(color = class)) + 
  geom_smooth()
```

```
## `geom_smooth()` using method = 'loess'
```

![plot of chunk unnamed-chunk-14](figure/unnamed-chunk-14-1.png)

```r
#specify different data for each layer
ggplot(data = mpg, mapping = aes(x = displ, y = hwy)) + 
  geom_point(mapping = aes(color = class)) + 
  geom_smooth(data = filter(mpg, class == "subcompact"), se = FALSE)
```

```
## `geom_smooth()` using method = 'loess'
```

![plot of chunk unnamed-chunk-14](figure/unnamed-chunk-14-2.png)

###Statistical Transformations
Many graphs, like scatterplots, plot the raw values of our dataset. Other graphs, like **bar charts**, calculate new values to plot:

* bar charts, histograms, and frequency polygons bin our data and then plot bin counts, the number of points that fall in each bin.
* smoothers fit a model to our data and then plot predictions from the model.
* boxplots compute a robust summary of the distribution and then display a specially formatted box.

The algorithm used to calculate new values for a graph is called a stat, short for statistical transformation. The figure below describes how this process works with `geom_bar()`.
![](http://r4ds.had.co.nz/images/visualization-stat-bar.png)

**Three reasons that we might need to use a stat explicitly:**

1. We might want to override the default stat.In the code below, WE change the stat of `geom_bar()` from `count` (the default) to `identity`. This lets me map the height of the bars to the raw values of a `y` variable.


```r
#build a dataframe
demo <- tribble(
  ~cut,         ~freq,
  "Fair",       1610,
  "Good",       4906,
  "Very Good",  12082,
  "Premium",    13791,
  "Ideal",      21551
)

ggplot(data = demo) +
  geom_bar(mapping = aes(x = cut, y = freq), stat ="identity")
```

![plot of chunk unnamed-chunk-15](figure/unnamed-chunk-15-1.png)

2. We might want to override the default mapping from transformed variables to aesthetics. For example, we might want to display a bar chart of proportion, rather than count:


```r
ggplot(data = diamonds) + 
  geom_bar(mapping = aes(x = cut, y = ..prop.., group = 1))
```

![plot of chunk unnamed-chunk-16](figure/unnamed-chunk-16-1.png)

3. We might want to draw greater attention to the statistical transformation in our code. For example, we might use `stat_summary()`, which summarises the y values for each unique x value, to draw attention to the summary that we¡¯re computing:


```r
ggplot(data = diamonds) + 
  stat_summary(
    mapping = aes(x = cut, y = depth),
    fun.ymin = min,
    fun.ymax = max,
    fun.y = median
  )
```

![plot of chunk unnamed-chunk-17](figure/unnamed-chunk-17-1.png)

ggplot2 provides over 20 stats for us to use. Each stat is a function, so we can get help in the usual way, e.g. `?stat_bin`. To see a complete list of stats, try the ggplot2 [cheatsheet](http://rstudio.com/cheatsheets).

###Position Adjustments
We can colour a bar chart using either the `colour` aesthetic, or, more usefully, `fill`. If we map the fill aesthetic to another variable, like clarity: the bars are automatically stacked. Each colored rectangle represents a combination of cut and clarity.


```r
ggplot(data = diamonds) + 
  geom_bar(mapping = aes(x = cut, fill = clarity))
```

![plot of chunk unnamed-chunk-18](figure/unnamed-chunk-18-1.png)

The stacking is performed automatically by the **position adjustment** specified by the position argument. If we don¡¯t want a stacked bar chart, we can use one of three other options: `"identity"`, `"dodge"` or `"fill"`.
 
* `position = "identity"` will place each object exactly where it falls in the context of the graph. This is not very useful for bars, because it overlaps them. The identity position adjustment is more useful for 2d geoms, like points, where it is the default.


```r
ggplot(data = diamonds, mapping = aes(x = cut, fill = clarity)) + 
  geom_bar(alpha = 1/5, position = "identity")
```

![plot of chunk unnamed-chunk-19](figure/unnamed-chunk-19-1.png)

* `position = "fill"` works like stacking, but makes each set of stacked bars the same height. This makes it easier to compare proportions across groups.


```r
ggplot(data = diamonds) + 
  geom_bar(mapping = aes(x = cut, fill = clarity), position = "fill")
```

![plot of chunk unnamed-chunk-20](figure/unnamed-chunk-20-1.png)

* `position = "dodge"` places overlapping objects directly beside one another. This makes it easier to compare individual values.

There¡¯s one other type of adjustment that¡¯s not useful for bar charts, but it can be very useful for scatterplots. This problem is known as **overplotting**. This arrangement makes it hard to see where the mass of the data is.  
We can avoid this gridding by setting the position adjustment to ¡°jitter¡±. `position = "jitter"` adds a small amount of random noise to each point. This spreads the points out because no two points are likely to receive the same amount of random noise. Because this is such a useful operation, ggplot2 comes with a shorthand for `geom_point(position = "jitter")`: `geom_jitter()`.


```r
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy), position = "jitter")
```

![plot of chunk unnamed-chunk-21](figure/unnamed-chunk-21-1.png)

###Corordinate Systems

Coordinate systems are probably the most complicated part of ggplot2. The default coordinate system is the Cartesian coordinate system where the x and y positions act independently to determine the location of each point. There are a number of other coordinate systems that are occasionally helpful.

* `coord_flip()` switches the x and y axes. This is useful (for example), if we want horizontal boxplots. It¡¯s also useful for long labels: it¡¯s hard to get them to fit without overlapping on the x-axis.


```r
ggplot(data = mpg, mapping = aes(x = class, y = hwy)) + 
  geom_boxplot()
```

![plot of chunk unnamed-chunk-22](figure/unnamed-chunk-22-1.png)

```r
ggplot(data = mpg, mapping = aes(x = class, y = hwy)) + 
  geom_boxplot() +
  coord_flip()
```

![plot of chunk unnamed-chunk-22](figure/unnamed-chunk-22-2.png)

* `coord_quickmap()` sets the aspect ratio correctly for maps. This is very important if you¡¯re plotting spatial data with ggplot2.


```r
library("maps")
nz <- map_data("nz")

ggplot(nz, aes(long, lat, group = group)) +
  geom_polygon(fill = "white", colour = "black")
```

![plot of chunk unnamed-chunk-23](figure/unnamed-chunk-23-1.png)

```r
ggplot(nz, aes(long, lat, group = group)) +
  geom_polygon(fill = "white", colour = "black") +
  coord_quickmap()
```

![plot of chunk unnamed-chunk-23](figure/unnamed-chunk-23-2.png)

* `coord_polar()` uses polar coordinates. Polar coordinates reveal an interesting connection between a bar chart and a Coxcomb chart.


```r
bar <- ggplot(data = diamonds) + 
  geom_bar(
    mapping = aes(x = cut, fill = cut), 
    show.legend = FALSE,
    width = 1
  ) + 
  theme(aspect.ratio = 1) +
  labs(x = NULL, y = NULL)
bar + coord_flip()
```

![plot of chunk unnamed-chunk-24](figure/unnamed-chunk-24-1.png)

```r
bar + coord_polar()
```

![plot of chunk unnamed-chunk-24](figure/unnamed-chunk-24-2.png)

Here shows how to draw a pie chart with `coord_polar()`, more ways to use `coord_polar()` could be found in this [link](http://ggplot2.tidyverse.org/reference/coord_polar.html)


```r
ggplot(data = diamonds) +
  geom_bar(
    mapping = aes(x =factor(1), fill = cut),
    width = 1
  ) +
  coord_polar(theta="y")
```

![plot of chunk unnamed-chunk-25](figure/unnamed-chunk-25-1.png)

### The Layered Grammar of Graphics
Now we learned much more than how to make scatterplots, bar charts, and boxplots. We learned a foundation that we can use to make any type of plot with ggplot2. To see this, let¡¯s add position adjustments, stats, coordinate systems, and faceting to our code template:


```r
#ggplot(data = <DATA>) + 
#  <GEOM_FUNCTION>(
#     mapping = aes(<MAPPINGS>),
#     stat = <STAT>, 
#     position = <POSITION>
#  ) +
#  <COORDINATE_FUNCTION> +
#  <FACET_FUNCTION>
```

This is the first part of my learning notes for *R for Data Science*. Through this part of learning, I find many details that I didn't notice when I used ggplot2 before. Just as a chinese old saying said, "Consider the past, and you shall know the future.". It will be really helpful for me to review what I've learned before and try to find what I ignored before. And I will continue writing learning notes to record my study process. I also hope it could help someone who encounters problems about ggplot2 and want to find solutions through others' blog, just like what I usually do.








