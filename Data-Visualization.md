Data Visualization
================
Declan Little
8/21/2020

# Data Visualization

### 3.1 Introduction

``` r
# we must load the tidyverse library every session
library(tidyverse)
```

    ## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.0 ──

    ## ✓ ggplot2 3.3.2     ✓ purrr   0.3.4
    ## ✓ tibble  3.0.3     ✓ dplyr   1.0.2
    ## ✓ tidyr   1.1.1     ✓ stringr 1.4.0
    ## ✓ readr   1.3.1     ✓ forcats 0.5.0

    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

### 3.2 First Steps

QUESTION: Do cars with big engines use more gas than cars with small
engines?

##### 3.2.1 The mpg data frame

A data frame is a rectangular collection of variables (in the columns)
and observations (in the rows). mpg contains observations collected by
the US Environmental Protection Agency on 38 models of car.

``` r
mpg
```

    ## # A tibble: 234 x 11
    ##    manufacturer model    displ  year   cyl trans   drv     cty   hwy fl    class
    ##    <chr>        <chr>    <dbl> <int> <int> <chr>   <chr> <int> <int> <chr> <chr>
    ##  1 audi         a4         1.8  1999     4 auto(l… f        18    29 p     comp…
    ##  2 audi         a4         1.8  1999     4 manual… f        21    29 p     comp…
    ##  3 audi         a4         2    2008     4 manual… f        20    31 p     comp…
    ##  4 audi         a4         2    2008     4 auto(a… f        21    30 p     comp…
    ##  5 audi         a4         2.8  1999     6 auto(l… f        16    26 p     comp…
    ##  6 audi         a4         2.8  1999     6 manual… f        18    26 p     comp…
    ##  7 audi         a4         3.1  2008     6 auto(a… f        18    27 p     comp…
    ##  8 audi         a4 quat…   1.8  1999     4 manual… 4        18    26 p     comp…
    ##  9 audi         a4 quat…   1.8  1999     4 auto(l… 4        16    25 p     comp…
    ## 10 audi         a4 quat…   2    2008     4 manual… 4        20    28 p     comp…
    ## # … with 224 more rows

##### 3.2.2 Creating a ggplot

With ggplot2, you begin a plot with the function ggplot(). ggplot()
creates a coordinate system that you can add layers to. The first
argument of ggplot() is the dataset to use in the graph.

You complete your graph by adding one or more layers to ggplot(). The
function geom\_point() adds a layer of points to your plot, which
creates a scatterplot.

Each geom function in ggplot2 takes a mapping argument. This defines how
variables in your dataset are mapped to visual properties. The mapping
argument is always paired with aes(), and the x and y arguments of aes()
specify which variables to map to the x and y axes. ggplot2 looks for
the mapped variables in the data argument, in this case, mpg.

``` r
ggplot(data = mpg) + geom_point(mapping = aes(x = displ, y = hwy))
```

![](Data-Visualization_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

The plot shows a negative relationship between engine size (displ) and
fuel efficiency (hwy). In other words, cars with big engines use more
fuel. Does this confirm or refute your hypothesis about fuel efficiency
and engine size?

##### Exercises

> 1.  Run ggplot(data = mpg). What do you see?

This code creates an empty plot. The ggplot() function creates the
background of the plot, but since no layers were specified with geom
function, nothing is drawn.

``` r
ggplot(data = mpg)
```

![](Data-Visualization_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

> 2.  How many rows are in mpg? How many columns?

There are 234 rows and 11 columns in the mpg data frame.

``` r
?mpg
```

> 3.  What does the drv variable describe? Read the help for ?mpg to
>     find out.

The drv variable is a categorical variable which categorizes cars into
front-wheels, rear-wheels, or four-wheel drive.

``` r
?mpg
```

> 4.  Make a scatterplot of hwy vs cyl.

``` r
ggplot(data = mpg) + geom_point(mapping = aes(x = cyl, y = hwy))
```

![](Data-Visualization_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

> 5.  What happens if you make a scatterplot of class vs drv? Why is the
>     plot not useful?

A scatter plot is not useful in this case because both variables are
CATEGORICAL, it doesn’t show us how many observations there are of each
variable. Scatter plots are better suited for plotting continuous
variables.

``` r
ggplot(data = mpg) + geom_point(mapping = aes(x = class, y = drv))
```

![](Data-Visualization_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->

### 3.3 Aesthetic mappings

You can convey information about your data by mapping the aesthetics in
your plot to the variables in your dataset. For example, you can map the
colors of your points to the class variable to reveal the class of each
car.

To map an aesthetic to a variable, associate the name of the aesthetic
to the name of the variable inside aes(). ggplot2 will automatically
assign a unique level of the aesthetic (here a unique color) to each
unique value of the variable, a process known as scaling. ggplot2 will
also add a legend that explains which levels correspond to which values.

``` r
ggplot(data = mpg) + geom_point(mapping = aes(x = displ, y = hwy, color = class))
```

![](Data-Visualization_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->

In the above example, we mapped class to the color aesthetic, but we
could have mapped class to the size aesthetic in the same way. In this
case, the exact size of each point would reveal its class affiliation.
We get a warning here, because mapping an unordered variable (class) to
an ordered aesthetic (size) is not a good idea.

``` r
ggplot(data = mpg) + geom_point(mapping = aes(x = displ, y = hwy, size = class))
```

    ## Warning: Using size for a discrete variable is not advised.

![](Data-Visualization_files/figure-gfm/unnamed-chunk-10-1.png)<!-- -->

Or we could have mapped class to the alpha aesthetic, which controls the
transparency of the points, or to the shape aesthetic, which controls
the shape of the points.

``` r
ggplot(data = mpg) + geom_point(mapping = aes(x = displ, y = hwy, alpha = class))
```

    ## Warning: Using alpha for a discrete variable is not advised.

![](Data-Visualization_files/figure-gfm/unnamed-chunk-11-1.png)<!-- -->

What happened to the SUVs? ggplot2 will only use six shapes at a time.
By default, additional groups will go unplotted when you use the shape
aesthetic.

``` r
ggplot(data = mpg) + geom_point(mapping = aes(x = displ, y = hwy, shape = class))
```

    ## Warning: The shape palette can deal with a maximum of 6 discrete values because
    ## more than 6 becomes difficult to discriminate; you have 7. Consider
    ## specifying shapes manually if you must have them.

    ## Warning: Removed 62 rows containing missing values (geom_point).

![](Data-Visualization_files/figure-gfm/unnamed-chunk-12-1.png)<!-- -->

For each aesthetic, you use aes() to associate the name of the aesthetic
with a variable to display. The aes() function gathers together each of
the aesthetic mappings used by a layer and passes them to the layer’s
mapping argument. The syntax highlights a useful insight about x and y:
the x and y locations of a point are themselves aesthetics, visual
properties that you can map to variables to display information about
the data.

Once you map an aesthetic, ggplot2 takes care of the rest. It selects a
reasonable scale to use with the aesthetic, and it constructs a legend
that explains the mapping between levels and values. For x and y
aesthetics, ggplot2 does not create a legend, but it creates an axis
line with tick marks and a label. The axis line acts as a legend; it
explains the mapping between locations and values.

You can also set the aesthetic properties of your geom manually. For
example, we can make all of the points in our plot blue:

``` r
ggplot(data = mpg) + geom_point(mapping = aes(x = displ, y = hwy), color = "blue")
```

![](Data-Visualization_files/figure-gfm/unnamed-chunk-13-1.png)<!-- -->

##### 3.3.1 Exercises

> 1.  What’s gone wrong with this code? Why are the points not blue?

``` r
ggplot(data = mpg) + geom_point(mapping = aes(x = displ, y = hwy, color = "blue"))
```

![](Data-Visualization_files/figure-gfm/unnamed-chunk-14-1.png)<!-- -->

The argument color = “blue” is included within the mapping argument, and
as such, it is treated as an aesthetic, which is a mapping between a
variable and a value.

The following code does produces the expected result.

``` r
ggplot(data = mpg) + geom_point(mapping = aes(x = displ, y = hwy), colour = "blue")
```

![](Data-Visualization_files/figure-gfm/unnamed-chunk-15-1.png)<!-- -->

> 2.  Which variables in mpg are categorical? Which variables are
>     continuous? (Hint: type ?mpg to read the documentation for the
>     dataset). How can you see this information when you run mpg?

``` r
?mpg
```

Categorical is type of drv train, trans, cyl, etc, whereas continous
variables are more like cty, hwy, mpg, etc \> 3. Map a continuous
variable to color, size, and shape. How do these aesthetics behave
differently for categorical vs. continuous variables?

``` r
ggplot(data = mpg) + geom_point(mapping = aes(x = displ, y = hwy, color = cty))
```

![](Data-Visualization_files/figure-gfm/unnamed-chunk-17-1.png)<!-- -->
You can group categorical variable on a scatter plot through aesthetics
like color, shape, or size

``` r
ggplot(data = mpg) + geom_point(mapping = aes(x = displ, y = hwy, size = cty))
```

![](Data-Visualization_files/figure-gfm/unnamed-chunk-18-1.png)<!-- -->

``` r
# THIS CODE WON'T WORK
# ggplot(data = mpg) + geom_point(mapping = aes(x = displ, y = hwy, shape = cty))
```

> 4.  What happens if you map the same variable to multiple aesthetics?

REDUNDANT

``` r
ggplot(data = mpg) + geom_point(mapping = aes(x = displ, y = hwy, color = hwy))
```

![](Data-Visualization_files/figure-gfm/unnamed-chunk-20-1.png)<!-- -->

> 6.  What happens if you map an aesthetic to something other than a
>     variable name, like aes(colour = displ \< 5)? Note, you’ll also
>     need to specify x and y.

Aesthetics can also be mapped to expressions like displ \< 5. The
ggplot() function behaves as if a temporary variable was added to the
data with values equal to the result of the expression. In this case,
the result of displ \< 5 is a logical variable which takes values of
TRUE or FALSE.

``` r
ggplot(data = mpg) + geom_point(mapping = aes(x = displ, y = hwy, color = displ < 5))
```

![](Data-Visualization_files/figure-gfm/unnamed-chunk-21-1.png)<!-- -->

### 3.5 Facets

``` r
ggplot(data = mpg) + geom_point(mapping = aes(x = displ, y = hwy)) + facet_wrap(~ class, nrow = 2)
```

![](Data-Visualization_files/figure-gfm/unnamed-chunk-22-1.png)<!-- -->

``` r
ggplot(data = mpg) + geom_point(mapping = aes(x = displ, y = hwy)) + facet_grid(drv ~ cyl)
```

![](Data-Visualization_files/figure-gfm/unnamed-chunk-23-1.png)<!-- -->

``` r
?mpg
```

##### 3.5.1 Exercises

> 1.  What happens if you facet on a continuous variable?

``` r
ggplot(data = mpg) + geom_point(mapping = aes(x = displ, y = hwy)) + facet_wrap(~ cty, nrow = 1)
```

![](Data-Visualization_files/figure-gfm/unnamed-chunk-25-1.png)<!-- -->
Facetting on a continous variable ends up separating the data so that
each data set is grouped by same cty mpg. \> 2. What do the empty cells
in plot with facet\_grid(drv \~ cyl) mean? How do they relate to this
plot?

``` r
ggplot(data = mpg) + geom_point(mapping = aes(x = displ, y = hwy)) + facet_grid(drv ~ cyl)
```

![](Data-Visualization_files/figure-gfm/unnamed-chunk-26-1.png)<!-- -->
The empty cells are sets for which no data exists, for example: There
are no 4 wd and 5 cyl cars in the data set.

> 3.  What plots does the following code make? What does . do?

``` r
ggplot(data = mpg) + geom_point(mapping = aes(x = displ, y = hwy)) + facet_grid(drv ~ .)
```

![](Data-Visualization_files/figure-gfm/unnamed-chunk-27-1.png)<!-- -->
Wherever a . is inputted, no data will show for that aesthetic. If it
comes first, no data will show on the side, if it comes later, no data
will show at the top.

``` r
ggplot(data = mpg) + geom_point(mapping = aes(x = displ, y = hwy)) + facet_grid(. ~ cyl)
```

![](Data-Visualization_files/figure-gfm/unnamed-chunk-28-1.png)<!-- -->

> 4.  Take the first faceted plot in this section: What are the
>     advantages to using faceting instead of the colour aesthetic? What
>     are the disadvantages? How might the balance change if you had a
>     larger dataset?

``` r
ggplot(data = mpg) + geom_point(mapping = aes(x = displ, y = hwy)) + facet_wrap(~ class, nrow = 2)
```

![](Data-Visualization_files/figure-gfm/unnamed-chunk-29-1.png)<!-- -->

Advantages of encoding class with facets instead of color include the
ability to encode more distinct categories. For me, it is difficult to
distinguish between the colors of “midsize” and “minivan”.

Given human visual perception, the max number of colors to use when
encoding unordered categorical (qualitative) data is nine, and in
practice, often much less than that. Displaying observations from
different categories on different scales makes it difficult to directly
compare values of observations across categories. However, it can make
it easier to compare the shape of the relationship between the x and y
variables across categories.

Disadvantages of encoding the class variable with facets instead of the
color aesthetic include the difficulty of comparing the values of
observations between categories since the observations for each category
are on different plots. Using the same x- and y-scales for all facets
makes it easier to compare values of observations across categories, but
it is still more difficult than if they had been displayed on the same
plot. Since encoding class within color also places all points on the
same plot, it visualizes the unconditional relationship between the x
and y variables; with facets, the unconditional relationship is no
longer visualized since the points are spread across multiple plots.

The benefit of encoding a variable with facetting over encoding it with
color increase in both the number of points and the number of
categories. With a large number of points, there is often overlap. It is
difficult to handle overlapping points with different colors color.
Jittering will still work with color. But jittering will only work well
if there are few points and the classes do not overlap much, otherwise,
the colors of areas will no longer be distinct, and it will be hard to
pick out the patterns of different categories visually. Transparency
(alpha) does not work well with colors since the mixing of overlapping
transparent colors will no longer represent the colors of the
categories. Binning methods already use color to encode the density of
points in the bin, so color cannot be used to encode categories.

As the number of categories increases, the difference between colors
decreases, to the point that the color of categories will no longer be
visually distinct.

> 5.  Read ?facet\_wrap. What does nrow do? What does ncol do? What
>     other options control the layout of the individual panels? Why
>     doesn’t facet\_grid() have nrow and ncol arguments?

``` r
?facet_wrap
```

nrow gives the number of rows you want, same with ncolumn

> 6.  When using facet\_grid() you should usually put the variable with
>     more unique levels in the columns. Why?

The y axis is the dependent variable, so it is usually easier to compare
trends and patterns more easily when they are adjacent.

``` r
ggplot(data = mpg) + geom_point(mapping = aes(x = displ, y = hwy)) + facet_grid(cyl ~ cty)
```

![](Data-Visualization_files/figure-gfm/unnamed-chunk-31-1.png)<!-- -->

### 3.6 Geometric Objects

``` r
ggplot(data = mpg) + geom_smooth(mapping=aes(x=displ, y=hwy))
```

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

![](Data-Visualization_files/figure-gfm/unnamed-chunk-32-1.png)<!-- -->

``` r
ggplot(data = mpg) + geom_smooth(mapping = aes(x=displ, y=hwy, linetype=drv))
```

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

![](Data-Visualization_files/figure-gfm/unnamed-chunk-33-1.png)<!-- -->

``` r
ggplot(data = mpg) + geom_smooth(mapping = aes(x=displ, y=hwy, color=drv))
```

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

![](Data-Visualization_files/figure-gfm/unnamed-chunk-34-1.png)<!-- -->

``` r
ggplot(data = mpg) + geom_point(mapping = aes(x=displ, y=hwy)) + geom_smooth(mapping = aes(x=displ, y=hwy))
```

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

![](Data-Visualization_files/figure-gfm/unnamed-chunk-35-1.png)<!-- -->

``` r
ggplot(data = mpg, mapping = aes(x=displ, y=hwy, color=drv)) + geom_point() + geom_smooth()
```

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

![](Data-Visualization_files/figure-gfm/unnamed-chunk-36-1.png)<!-- -->

``` r
ggplot(data = mpg, mapping = aes(x=displ, y=hwy)) + geom_smooth() + geom_point(mapping=aes(color=class)) 
```

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

![](Data-Visualization_files/figure-gfm/unnamed-chunk-37-1.png)<!-- -->

## Exercises 1-5

1.  What geom would you use to draw a line chart? A boxplot? A
    histogram? An area chart?

line chart is geom\_smooth, geom\_histogram, geom\_area

2.Run this code in your head and predict what the output will look like.
Then, run the code in R and check your predictions.

``` r
ggplot(data = mpg, mapping = aes(x = displ, y = hwy, color = drv)) + 
  geom_point() + geom_smooth(se = FALSE)
```

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

![](Data-Visualization_files/figure-gfm/unnamed-chunk-38-1.png)<!-- -->
Cod will produce something with drv variables, but im not sure how this
will combine with a smooth line of best fit

3.  What does show.legend = FALSE do? What happens if you remove it? Why
    do you think I used it earlier in the chapter?

show.legend=false makes the aesthetic legend dissapear

4.  What does the se argument to geom\_smooth() do?

standard error

5.  Will these two graphs look different? Why/why not?

no, one will have a legend and the other will not?

### 3.7 Statistical Information

``` r
diamonds
```

    ## # A tibble: 53,940 x 10
    ##    carat cut       color clarity depth table price     x     y     z
    ##    <dbl> <ord>     <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
    ##  1 0.23  Ideal     E     SI2      61.5    55   326  3.95  3.98  2.43
    ##  2 0.21  Premium   E     SI1      59.8    61   326  3.89  3.84  2.31
    ##  3 0.23  Good      E     VS1      56.9    65   327  4.05  4.07  2.31
    ##  4 0.290 Premium   I     VS2      62.4    58   334  4.2   4.23  2.63
    ##  5 0.31  Good      J     SI2      63.3    58   335  4.34  4.35  2.75
    ##  6 0.24  Very Good J     VVS2     62.8    57   336  3.94  3.96  2.48
    ##  7 0.24  Very Good I     VVS1     62.3    57   336  3.95  3.98  2.47
    ##  8 0.26  Very Good H     SI1      61.9    55   337  4.07  4.11  2.53
    ##  9 0.22  Fair      E     VS2      65.1    61   337  3.87  3.78  2.49
    ## 10 0.23  Very Good H     VS1      59.4    61   338  4     4.05  2.39
    ## # … with 53,930 more rows

ggplot(data = diamonds) + geom\_bar(mapping = aes(x=cut))

``` r
ggplot(data = diamonds) + geom_bar(mapping = aes(x=cut))
```

![](Data-Visualization_files/figure-gfm/unnamed-chunk-40-1.png)<!-- -->

### 3.8 Position Adjustments

``` r
ggplot(data = diamonds) + geom_bar(mapping = aes(x=cut, color=cut))
```

![](Data-Visualization_files/figure-gfm/unnamed-chunk-41-1.png)<!-- -->

``` r
ggplot(data = diamonds) + geom_bar(mapping = aes(x=cut, fill=cut))
```

![](Data-Visualization_files/figure-gfm/unnamed-chunk-42-1.png)<!-- -->

``` r
ggplot(data = diamonds) + geom_bar(mapping = aes(x= cut, fill=clarity))
```

![](Data-Visualization_files/figure-gfm/unnamed-chunk-43-1.png)<!-- -->

``` r
ggplot(data = diamonds, mapping = aes(x = cut, fill = clarity)) + geom_bar(position = "dodge")
```

![](Data-Visualization_files/figure-gfm/unnamed-chunk-44-1.png)<!-- -->

``` r
ggplot(data = mpg) + geom_point(mapping = aes(x=displ, y=hwy))
```

![](Data-Visualization_files/figure-gfm/unnamed-chunk-45-1.png)<!-- -->

``` r
ggplot(data = mpg) + geom_point(mapping = aes(x = displ, y = hwy), position = "jitter")
```

![](Data-Visualization_files/figure-gfm/unnamed-chunk-46-1.png)<!-- -->

``` r
ggplot(data = diamonds, mapping = aes(x = cut, fill  = clarity)) + geom_bar(position = "fill")
```

![](Data-Visualization_files/figure-gfm/unnamed-chunk-47-1.png)<!-- -->

``` r
ggplot(data = diamonds, mapping = aes(x = cut, color = clarity)) + geom_bar(fill = NA, position = "identity")
```

![](Data-Visualization_files/figure-gfm/unnamed-chunk-48-1.png)<!-- -->

``` r
ggplot(data = diamonds, mapping = aes(x = cut, fill = clarity)) + geom_bar(alpha = 0.2, position = "identity")
```

![](Data-Visualization_files/figure-gfm/unnamed-chunk-49-1.png)<!-- -->
\#\#\# 3.8.1 Exercises

1.  What is the problem with this plot? How could you improve it?

<!-- end list -->

``` r
ggplot(data = mpg, mapping = aes(x = cty, y = hwy)) + 
  geom_point()
```

![](Data-Visualization_files/figure-gfm/unnamed-chunk-50-1.png)<!-- -->
You could facet wrap this data, or you could make a box plot and fill
it, so the x variables dont stack, or we could add jitter so they are
less discrete

2.  What parameters to geom\_jitter() control the amount of jittering?
    you can control the height and width of the jitter

3.  Compare and contrast geom\_jitter() with geom\_count().

<!-- end list -->

``` r
ggplot(data = mpg, mapping = aes(x = cty, y = hwy)) + 
  geom_point() +
  geom_count()
```

![](Data-Visualization_files/figure-gfm/unnamed-chunk-51-1.png)<!-- -->

``` r
ggplot(data = mpg, mapping = aes(x = cty, y = hwy)) + 
  geom_point() +
  geom_jitter()
```

![](Data-Visualization_files/figure-gfm/unnamed-chunk-52-1.png)<!-- -->
Geom\_count() controls the size of the points based on what seems like
the frequency of that point

4.  What’s the default position adjustment for geom\_boxplot()? Create a
    visualisation of the mpg dataset that demonstrates it.

<!-- end list -->

``` r
ggplot(data = mpg, mapping = aes(x = class, y = displ)) + 
  geom_boxplot(aes(colour = drv))
```

![](Data-Visualization_files/figure-gfm/unnamed-chunk-53-1.png)<!-- -->

``` r
??baseball
```

# Data Visualization Project on

## By: Declan Little

From this data, we can see that in the mlb, OOBP (Opponent On Base
Percentage) affects the amount of wins one can expect in a season. When
you let your opponent get on base more often, it is obvious that you
will win less games. This is backed by the data. As you pitch better and
allow less runners on base, a correlation with a higher amount of wins
is seen. From this data, we can also see that in order to make the
playoffs (the blue points) a team should aim for about 85 wins. Still,
there is a large group around the middle where a team exhibits an
average to above average OOBP, yet their team still wins a higher than
average amount of games. This can be attributed to good hitting, or a
weak division/schedule.

From this data, we can visualize the the amount of times an AL/NL team
has held a ranking during the regular season. During the years in the
data set, we gather that the number 1 ranked team, historically, has a
higher chance of coming from the AL. However, the \#2 ranked team,
historically,has a higher likelihood of hailing from the NL. When
looking at this data set, we have to acknowledge that being ranked 5-8
is not recorded as often as being ranked 1-4. It is interesting that the
score between AL and NL are even when it comes to the amount of teams
that have been ranked 6th overall.
