# (PART) R {-}

# Data Visualization

Data visualization Chapter in r4ds

- http://r4ds.had.co.nz/data-visualisation.html

Datacamp Courses:

- The `ggplot2` stack
    - https://www.datacamp.com/courses/data-visualization-with-ggplot2-1
    - https://www.datacamp.com/courses/data-visualization-with-ggplot2-2
    - https://www.datacamp.com/courses/data-visualization-with-ggplot2-part-3

## Loading ggplot2


```r
library(ggplot2)
```


```r
# mpg dataset from the ggplot2 library
mpg
```

```
## # A tibble: 234 x 11
##    manufacturer model    displ  year   cyl trans   drv     cty   hwy fl   
##    <chr>        <chr>    <dbl> <int> <int> <chr>   <chr> <int> <int> <chr>
##  1 audi         a4         1.8  1999     4 auto(l… f        18    29 p    
##  2 audi         a4         1.8  1999     4 manual… f        21    29 p    
##  3 audi         a4         2    2008     4 manual… f        20    31 p    
##  4 audi         a4         2    2008     4 auto(a… f        21    30 p    
##  5 audi         a4         2.8  1999     6 auto(l… f        16    26 p    
##  6 audi         a4         2.8  1999     6 manual… f        18    26 p    
##  7 audi         a4         3.1  2008     6 auto(a… f        18    27 p    
##  8 audi         a4 quat…   1.8  1999     4 manual… 4        18    26 p    
##  9 audi         a4 quat…   1.8  1999     4 auto(l… 4        16    25 p    
## 10 audi         a4 quat…   2    2008     4 manual… 4        20    28 p    
## # ... with 224 more rows, and 1 more variable: class <chr>
```


## Creating a ggplot


```r
ggplot(data = mpg) + geom_point(mapping = aes(x = displ, y = hwy))
```

<img src="010-data_vis_files/figure-html/unnamed-chunk-3-1.png" width="672" />

## Aesthetic Mapings


```r
# using color
ggplot(data = mpg) + geom_point(mapping = aes(x = displ, y = hwy, color = class))
```

<img src="010-data_vis_files/figure-html/unnamed-chunk-4-1.png" width="672" />


```r
# using size
ggplot(data = mpg) + geom_point(mapping = aes(x = displ, y = hwy, size = class))
```

```
## Warning: Using size for a discrete variable is not advised.
```

<img src="010-data_vis_files/figure-html/unnamed-chunk-5-1.png" width="672" />


```r
# using alpha
ggplot(data = mpg) + geom_point(mapping = aes(x = displ, y = hwy, alpha = class))
```

<img src="010-data_vis_files/figure-html/unnamed-chunk-6-1.png" width="672" />


```r
# using shape
ggplot(data = mpg) + geom_point(mapping = aes(x = displ, y = hwy, shape = class))
```

```
## Warning: The shape palette can deal with a maximum of 6 discrete values
## because more than 6 becomes difficult to discriminate; you have 7.
## Consider specifying shapes manually if you must have them.
```

```
## Warning: Removed 62 rows containing missing values (geom_point).
```

<img src="010-data_vis_files/figure-html/unnamed-chunk-7-1.png" width="672" />


```r
# manual set property
# note color is not in the aes
ggplot(data = mpg) + geom_point(mapping = aes(x = displ, y = hwy), color = "blue")
```

<img src="010-data_vis_files/figure-html/unnamed-chunk-8-1.png" width="672" />


```r
# if you put a variable outside you will get an error
ggplot(data = mpg) + geom_point(mapping = aes(x = displ, y = hwy), color = class)
```

## Facets


```r
ggplot(data = mpg) + geom_point(mapping = aes(x = displ, y = hwy)) + facet_wrap(~ class, nrow = 2)
```

<img src="010-data_vis_files/figure-html/unnamed-chunk-10-1.png" width="672" />


```r
ggplot(data = mpg) + geom_point(mapping = aes(x = displ, y = hwy)) + facet_grid(drv ~ cyl)
```

<img src="010-data_vis_files/figure-html/unnamed-chunk-11-1.png" width="672" />

## Geometic Objects


```r
ggplot(data = mpg) + geom_point(mapping = aes(x = displ, y = hwy))
```

<img src="010-data_vis_files/figure-html/unnamed-chunk-12-1.png" width="672" />


```r
ggplot(data = mpg) + geom_smooth(mapping = aes(x = displ, y = hwy))
```

```
## `geom_smooth()` using method = 'loess'
```

<img src="010-data_vis_files/figure-html/unnamed-chunk-13-1.png" width="672" />


```r
ggplot(data = mpg) + geom_smooth(mapping = aes(x = displ, y = hwy, linetime = drv))
```

```
## Warning: Ignoring unknown aesthetics: linetime
```

```
## `geom_smooth()` using method = 'loess'
```

<img src="010-data_vis_files/figure-html/unnamed-chunk-14-1.png" width="672" />


```r
# base plot before groupings
ggplot(data = mpg) + geom_smooth(mapping = aes(x = displ, y = hwy))
```

```
## `geom_smooth()` using method = 'loess'
```

<img src="010-data_vis_files/figure-html/unnamed-chunk-15-1.png" width="672" />


```r
# base plot before groupings
ggplot(data = mpg) + geom_smooth(mapping = aes(x = displ, y = hwy))
```

```
## `geom_smooth()` using method = 'loess'
```

<img src="010-data_vis_files/figure-html/unnamed-chunk-16-1.png" width="672" />

```r
# separate smoothing line by group
ggplot(data = mpg) + geom_smooth(mapping = aes(x = displ, y = hwy, group = drv))
```

```
## `geom_smooth()` using method = 'loess'
```

<img src="010-data_vis_files/figure-html/unnamed-chunk-16-2.png" width="672" />

```r
# different color foe each group
ggplot(data = mpg) + geom_smooth(mapping = aes(x = displ, y = hwy, color = drv), show.legend = FALSE)
```

```
## `geom_smooth()` using method = 'loess'
```

<img src="010-data_vis_files/figure-html/unnamed-chunk-16-3.png" width="672" />


Adding multiple geoms in the same plot

```r
ggplot(data = mpg) +
  geom_point(mapping = aes(x = displ, y = hwy)) +
  geom_smooth(mapping = aes(x = displ, y = hwy))
```

```
## `geom_smooth()` using method = 'loess'
```

<img src="010-data_vis_files/figure-html/unnamed-chunk-17-1.png" width="672" />

The layering system will carry over values from the previous layer.
the ggplot layer will specify the global values


```r
ggplot(data = mpg, mapping = aes(x = displ, y = hwy)) +
  geom_point() +
  geom_smooth()
```

```
## `geom_smooth()` using method = 'loess'
```

<img src="010-data_vis_files/figure-html/unnamed-chunk-18-1.png" width="672" />

Mappings in a a geom function, will overwrite the global settings (i.e., they are local settings)


```r
ggplot(data = mpg, mapping = aes(x = displ, y = hwy)) +
  geom_point(mapping = aes(color = class)) +
  geom_smooth()
```

```
## `geom_smooth()` using method = 'loess'
```

<img src="010-data_vis_files/figure-html/unnamed-chunk-19-1.png" width="672" />


```r
ggplot(data = mpg, mapping = aes(x = displ, y = hwy)) +
  geom_point(mapping = aes(color = class)) +
  geom_smooth(
    data = dplyr::filter(mpg, class == 'subcompact'),
    se = FALSE
  )
```

```
## `geom_smooth()` using method = 'loess'
```

<img src="010-data_vis_files/figure-html/unnamed-chunk-20-1.png" width="672" />

## Statistical Transformations


```r
dim(diamonds)
```

```
## [1] 53940    10
```


```r
head(diamonds)
```

```
## # A tibble: 6 x 10
##   carat cut       color clarity depth table price     x     y     z
##   <dbl> <ord>     <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
## 1 0.23  Ideal     E     SI2      61.5    55   326  3.95  3.98  2.43
## 2 0.21  Premium   E     SI1      59.8    61   326  3.89  3.84  2.31
## 3 0.23  Good      E     VS1      56.9    65   327  4.05  4.07  2.31
## 4 0.290 Premium   I     VS2      62.4    58   334  4.2   4.23  2.63
## 5 0.31  Good      J     SI2      63.3    58   335  4.34  4.35  2.75
## 6 0.24  Very Good J     VVS2     62.8    57   336  3.94  3.96  2.48
```



```r
ggplot(data = diamonds) + geom_bar(mapping = aes(x = cut))
```

<img src="010-data_vis_files/figure-html/unnamed-chunk-23-1.png" width="672" />

Use a *stat* to calculate a new value.
diamonds data gets "transformed" into a frequency table that get's plotted by the bar plot.

Look at the geom_bar documentation, you will see the `stat` will be `count` (i.e., `stat_count()`).


```r
ggplot(data = diamonds) + stat_count(mapping = aes(x = cut))
```

<img src="010-data_vis_files/figure-html/unnamed-chunk-24-1.png" width="672" />

You can set stat to 'identity' if you have already calculated a frequency table


```r
pre_counted <- tibble::as.tibble(table(diamonds$cut))
```


```r
ggplot(data = pre_counted) +
  geom_bar(
    mapping = aes(x = Var1, y = n), stat = 'identity'
  )
```

<img src="010-data_vis_files/figure-html/unnamed-chunk-26-1.png" width="672" />



```r
# overwrite default stat
# proportion instead of count
ggplot(data = diamonds) +
  geom_bar(
    mapping = aes(x = cut, y = ..prop.., group = 1)
  )
```

<img src="010-data_vis_files/figure-html/unnamed-chunk-27-1.png" width="672" />

grouping: http://ggplot2.tidyverse.org/reference/aes_group_order.html

By default, the group is set to the interaction of all discrete variables in the
plot. This often partitions the data correctly, but when it does not, or when
no discrete variable is used in the plot, you will need to explicitly define the
grouping structure, by mapping group to a variable that has a different value
for each group.

## Position Adjustments


```r
# using color
ggplot(data = diamonds) + geom_bar(mapping = aes(x = cut, color = cut))
```

<img src="010-data_vis_files/figure-html/unnamed-chunk-28-1.png" width="672" />


```r
# using fill
ggplot(data = diamonds) + geom_bar(mapping = aes(x = cut, fill = cut))
```

<img src="010-data_vis_files/figure-html/unnamed-chunk-29-1.png" width="672" />


```r
# another example of fill
# fill a different variable than x
# This creates a stacked bar chart
ggplot(data = diamonds) + geom_bar(mapping = aes(x = cut, fill = clarity))
```

<img src="010-data_vis_files/figure-html/unnamed-chunk-30-1.png" width="672" />

position: identity


```r
ggplot(data = diamonds, mapping = aes(x = cut, fill = clarity)) + geom_bar(alpha = 1/5, position = 'identity')
```

<img src="010-data_vis_files/figure-html/unnamed-chunk-31-1.png" width="672" />



```r
ggplot(data = diamonds, mapping = aes(x = cut, color = clarity)) + geom_bar(fill = NA, position = 'identity')
```

<img src="010-data_vis_files/figure-html/unnamed-chunk-32-1.png" width="672" />

potition: fill


```r
ggplot(data = diamonds) + geom_bar(mapping = aes(x = cut, fill = clarity), position = 'fill')
```

<img src="010-data_vis_files/figure-html/unnamed-chunk-33-1.png" width="672" />

position: dodge


```r
ggplot(data = diamonds) + geom_bar(mapping = aes(x = cut, fill = clarity), position = 'dodge')
```

<img src="010-data_vis_files/figure-html/unnamed-chunk-34-1.png" width="672" />


Jitter scatter plot


```r
ggplot(data = mpg) + geom_point(mapping = aes(x = displ, y = hwy))
```

<img src="010-data_vis_files/figure-html/unnamed-chunk-35-1.png" width="672" />



```r
ggplot(data = mpg) + geom_point(mapping = aes(x = displ, y = hwy), position = 'jitter')
```

<img src="010-data_vis_files/figure-html/unnamed-chunk-36-1.png" width="672" />

## Coordinate Systems

coord_flipswaps the x and y axis, useful when you have long labels


```r
ggplot(data = mpg, mapping = aes(x = class, y = hwy)) + geom_boxplot()
```

<img src="010-data_vis_files/figure-html/unnamed-chunk-37-1.png" width="672" />


```r
ggplot(data = mpg, mapping = aes(x = class, y = hwy)) + geom_boxplot() + coord_flip()
```

<img src="010-data_vis_files/figure-html/unnamed-chunk-38-1.png" width="672" />

coord_quickmap, sets aspect ratio for maps


```r
usa <- ggplot2::map_data('usa')
```



```r
ggplot(usa, aes(long, lat, group = group)) + geom_polygon(fill = 'white', color = 'black')
```

<img src="010-data_vis_files/figure-html/unnamed-chunk-40-1.png" width="672" />


```r
ggplot(usa, aes(long, lat, group = group)) + geom_polygon(fill = 'white', color = 'black') + coord_quickmap()
```

<img src="010-data_vis_files/figure-html/unnamed-chunk-41-1.png" width="672" />

corrd_polar, uses polar coordinates


```r
bar <- ggplot(data = diamonds) +
  geom_bar(mapping = aes(x = cut, fill = cut), show.legend = FALSE, width = 1) +
  theme(aspect.ratio = 1) +
  labs(x = NULL, y = NULL)
```


```r
bar + coord_flip()
```

<img src="010-data_vis_files/figure-html/unnamed-chunk-43-1.png" width="672" />


```r
bar + coord_polar()
```

<img src="010-data_vis_files/figure-html/unnamed-chunk-44-1.png" width="672" />


