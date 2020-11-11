Data Exploration
================
Declan Little
9/18/2020

This chapter will show you how to use visualisation and transformation
to explore your data in a systematic way, a task that statisticians call
exploratory data analysis, or EDA for short. EDA is an iterative cycle.
You:

1.  Generate questions about your data.

2.  Search for answers by visualising, transforming, and modelling your
    data.

3.  Use what you learn to refine your questions and/or generate new
    questions.

EDA is not a formal process with a strict set of rules. More than
anything, EDA is a state of mind. During the initial phases of EDA you
should feel free to investigate every idea that occurs to you. Some of
these ideas will pan out, and some will be dead ends. As your
exploration continues, you will hone in on a few particularly productive
areas that you’ll eventually write up and communicate to others.

EDA is an important part of any data analysis, even if the questions are
handed to you on a platter, because you always need to investigate the
quality of your data. Data cleaning is just one application of EDA: you
ask questions about whether your data meets your expectations or not. To
do data cleaning, you’ll need to deploy all the tools of EDA:
visualisation, transformation, and modelling.

### Prerequisites

In this chapter we’ll combine what you’ve learned about dplyr and
ggplot2 to interactively ask questions, answer them with data, and then
ask new questions.

``` r
library(tidyverse)
```

## Questions

> “There are no routine statistical questions, only questionable
> statistical routines.” — Sir David Cox

> “Far better an approximate answer to the right question, which is
> often vague, than an exact answer to the wrong question, which can
> always be made precise.” — John Tukey

Your goal during EDA is to develop an understanding of your data. The
easiest way to do this is to use questions as tools to guide your
investigation. When you ask a question, the question focuses your
attention on a specific part of your dataset and helps you decide which
graphs, models, or transformations to make.

EDA is fundamentally a creative process. And like most creative
processes, the key to asking *quality* questions is to generate a large
*quantity* of questions. It is difficult to ask revealing questions at
the start of your analysis because you do not know what insights are
contained in your dataset. On the other hand, each new question that you
ask will expose you to a new aspect of your data and increase your
chance of making a discovery. You can quickly drill down into the most
interesting parts of your data—and develop a set of thought-provoking
questions—if you follow up each question with a new question based on
what you find.

There is no rule about which questions you should ask to guide your
research. However, two types of questions will always be useful for
making discoveries within your data. You can loosely word these
questions as:

1.  What type of variation occurs within my variables?

2.  What type of covariation occurs between my variables?

The rest of this chapter will look at these two questions. I’ll explain
what variation and covariation are, and I’ll show you several ways to
answer each question. To make the discussion easier, let’s define some
terms:

  - A **variable** is a quantity, quality, or property that you can
    measure.

  - A **value** is the state of a variable when you measure it. The
    value of a variable may change from measurement to measurement.

  - An **observation** is a set of measurements made under similar
    conditions (you usually make all of the measurements in an
    observation at the same time and on the same object). An observation
    will contain several values, each associated with a different
    variable. I’ll sometimes refer to an observation as a data point.

  - **Tabular data** is a set of values, each associated with a variable
    and an observation. Tabular data is *tidy* if each value is placed
    in its own “cell”, each variable in its own column, and each
    observation in its own row.

So far, all of the data that you’ve seen has been tidy. In real-life,
most data isn’t tidy, so we’ll come back to these ideas again in \[tidy
data\].

## Variation

**Variation** is the tendency of the values of a variable to change from
measurement to measurement. You can see variation easily in real life;
if you measure any continuous variable twice, you will get two different
results. This is true even if you measure quantities that are constant,
like the speed of light. Each of your measurements will include a small
amount of error that varies from measurement to measurement. Categorical
variables can also vary if you measure across different subjects
(e.g. the eye colors of different people), or different times (e.g. the
energy levels of an electron at different moments). Every variable has
its own pattern of variation, which can reveal interesting information.
The best way to understand that pattern is to visualise the distribution
of the variable’s values.

### Visualising distributions

How you visualise the distribution of a variable will depend on whether
the variable is categorical or continuous. A variable is **categorical**
if it can only take one of a small set of values. In R, categorical
variables are usually saved as factors or character vectors. To examine
the distribution of a categorical variable, use a bar chart:

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

``` r
ggplot(data=diamonds)+
  geom_bar(mapping=aes(x=cut)) 
```

![](Data-Exploration.md_files/figure-gfm/unnamed-chunk-1-1.png)<!-- -->

The height of the bars displays how many observations occurred with each
x value. You can compute these values manually with `dplyr::count()`:

``` r
diamonds%>%
  count(cut)
```

    ## # A tibble: 5 x 2
    ##   cut           n
    ##   <ord>     <int>
    ## 1 Fair       1610
    ## 2 Good       4906
    ## 3 Very Good 12082
    ## 4 Premium   13791
    ## 5 Ideal     21551

A variable is **continuous** if it can take any of an infinite set of
ordered values. Numbers and date-times are two examples of continuous
variables. To examine the distribution of a continuous variable, use a
histogram:

``` r
ggplot(data=diamonds)+geom_histogram(mapping=aes(x=carat),binwidth=.5)
```

![](Data-Exploration.md_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

You can compute this by hand by combining `dplyr::count()` and
`ggplot2::cut_width()`:

``` r
diamonds %>%
  count(cut_width(carat, 0.5))
```

    ## # A tibble: 11 x 2
    ##    `cut_width(carat, 0.5)`     n
    ##    <fct>                   <int>
    ##  1 [-0.25,0.25]              785
    ##  2 (0.25,0.75]             29498
    ##  3 (0.75,1.25]             15977
    ##  4 (1.25,1.75]              5313
    ##  5 (1.75,2.25]              2002
    ##  6 (2.25,2.75]               322
    ##  7 (2.75,3.25]                32
    ##  8 (3.25,3.75]                 5
    ##  9 (3.75,4.25]                 4
    ## 10 (4.25,4.75]                 1
    ## 11 (4.75,5.25]                 1

A histogram divides the x-axis into equally spaced bins and then uses
the height of a bar to display the number of observations that fall in
each bin. In the graph above, the tallest bar shows that almost 30,000
observations have a `carat` value between 0.25 and 0.75, which are the
left and right edges of the bar.

You can set the width of the intervals in a histogram with the
`binwidth` argument, which is measured in the units of the `x` variable.
You should always explore a variety of binwidths when working with
histograms, as different binwidths can reveal different patterns. For
example, here is how the graph above looks when we zoom into just the
diamonds with a size of less than three carats and choose a smaller
binwidth.

``` r
smaller<-diamonds%>%
  filter(carat<3)


ggplot(data=smaller)+
  geom_histogram(mapping=aes(x=carat), binwidth=0.01)
```

![](Data-Exploration.md_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

If you wish to overlay multiple histograms in the same plot, I recommend
using `geom_freqpoly()` instead of `geom_histogram()`. `geom_freqpoly()`
performs the same calculation as `geom_histogram()`, but instead of
displaying the counts with bars, uses lines instead. It’s much easier to
understand overlapping lines than bars.

``` r
ggplot(data=smaller)+
  geom_freqpoly(mapping=aes(x=carat, color=cut), binwidth=0.1)
```

![](Data-Exploration.md_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

There are a few challenges with this type of plot, which we will come
back to in [visualising a categorical and a continuous
variable](#cat-cont).

Now that you can visualise variation, what should you look for in your
plots? And what type of follow-up questions should you ask? I’ve put
together a list below of the most useful types of information that you
will find in your graphs, along with some follow-up questions for each
type of information. The key to asking good follow-up questions will be
to rely on your curiosity (What do you want to learn more about?) as
well as your skepticism (How could this be misleading?).

### Typical values

In both bar charts and histograms, tall bars show the common values of a
variable, and shorter bars show less-common values. Places that do not
have bars reveal values that were not seen in your data. To turn this
information into useful questions, look for anything unexpected:

  - Which values are the most common? Why?

  - Which values are rare? Why? Does that match your expectations?

  - Can you see any unusual patterns? What might explain them?

As an example, the histogram below suggests several interesting
questions:

  - Why are there more diamonds at whole carats and common fractions of
    carats?

  - Why are there more diamonds slightly to the right of each peak than
    there are slightly to the left of each peak?

  - Why are there no diamonds bigger than 3 carats?

<!-- end list -->

``` r
ggplot(data=smaller, mapping=aes(x=carat))+
  geom_histogram(binwidth=.1)
```

![](Data-Exploration.md_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

Clusters of similar values suggest that subgroups exist in your data. To
understand the subgroups, ask:

  - How are the observations within each cluster similar to each other?

  - How are the observations in separate clusters different from each
    other?

  - How can you explain or describe the clusters?

  - Why might the appearance of clusters be misleading?

The histogram below shows the length (in minutes) of 272 eruptions of
the Old Faithful Geyser in Yellowstone National Park. Eruption times
appear to be clustered into two groups: there are short eruptions (of
around 2 minutes) and long eruptions (4-5 minutes), but little in
between.

``` r
faithful
```

    ##     eruptions waiting
    ## 1       3.600      79
    ## 2       1.800      54
    ## 3       3.333      74
    ## 4       2.283      62
    ## 5       4.533      85
    ## 6       2.883      55
    ## 7       4.700      88
    ## 8       3.600      85
    ## 9       1.950      51
    ## 10      4.350      85
    ## 11      1.833      54
    ## 12      3.917      84
    ## 13      4.200      78
    ## 14      1.750      47
    ## 15      4.700      83
    ## 16      2.167      52
    ## 17      1.750      62
    ## 18      4.800      84
    ## 19      1.600      52
    ## 20      4.250      79
    ## 21      1.800      51
    ## 22      1.750      47
    ## 23      3.450      78
    ## 24      3.067      69
    ## 25      4.533      74
    ## 26      3.600      83
    ## 27      1.967      55
    ## 28      4.083      76
    ## 29      3.850      78
    ## 30      4.433      79
    ## 31      4.300      73
    ## 32      4.467      77
    ## 33      3.367      66
    ## 34      4.033      80
    ## 35      3.833      74
    ## 36      2.017      52
    ## 37      1.867      48
    ## 38      4.833      80
    ## 39      1.833      59
    ## 40      4.783      90
    ## 41      4.350      80
    ## 42      1.883      58
    ## 43      4.567      84
    ## 44      1.750      58
    ## 45      4.533      73
    ## 46      3.317      83
    ## 47      3.833      64
    ## 48      2.100      53
    ## 49      4.633      82
    ## 50      2.000      59
    ## 51      4.800      75
    ## 52      4.716      90
    ## 53      1.833      54
    ## 54      4.833      80
    ## 55      1.733      54
    ## 56      4.883      83
    ## 57      3.717      71
    ## 58      1.667      64
    ## 59      4.567      77
    ## 60      4.317      81
    ## 61      2.233      59
    ## 62      4.500      84
    ## 63      1.750      48
    ## 64      4.800      82
    ## 65      1.817      60
    ## 66      4.400      92
    ## 67      4.167      78
    ## 68      4.700      78
    ## 69      2.067      65
    ## 70      4.700      73
    ## 71      4.033      82
    ## 72      1.967      56
    ## 73      4.500      79
    ## 74      4.000      71
    ## 75      1.983      62
    ## 76      5.067      76
    ## 77      2.017      60
    ## 78      4.567      78
    ## 79      3.883      76
    ## 80      3.600      83
    ## 81      4.133      75
    ## 82      4.333      82
    ## 83      4.100      70
    ## 84      2.633      65
    ## 85      4.067      73
    ## 86      4.933      88
    ## 87      3.950      76
    ## 88      4.517      80
    ## 89      2.167      48
    ## 90      4.000      86
    ## 91      2.200      60
    ## 92      4.333      90
    ## 93      1.867      50
    ## 94      4.817      78
    ## 95      1.833      63
    ## 96      4.300      72
    ## 97      4.667      84
    ## 98      3.750      75
    ## 99      1.867      51
    ## 100     4.900      82
    ## 101     2.483      62
    ## 102     4.367      88
    ## 103     2.100      49
    ## 104     4.500      83
    ## 105     4.050      81
    ## 106     1.867      47
    ## 107     4.700      84
    ## 108     1.783      52
    ## 109     4.850      86
    ## 110     3.683      81
    ## 111     4.733      75
    ## 112     2.300      59
    ## 113     4.900      89
    ## 114     4.417      79
    ## 115     1.700      59
    ## 116     4.633      81
    ## 117     2.317      50
    ## 118     4.600      85
    ## 119     1.817      59
    ## 120     4.417      87
    ## 121     2.617      53
    ## 122     4.067      69
    ## 123     4.250      77
    ## 124     1.967      56
    ## 125     4.600      88
    ## 126     3.767      81
    ## 127     1.917      45
    ## 128     4.500      82
    ## 129     2.267      55
    ## 130     4.650      90
    ## 131     1.867      45
    ## 132     4.167      83
    ## 133     2.800      56
    ## 134     4.333      89
    ## 135     1.833      46
    ## 136     4.383      82
    ## 137     1.883      51
    ## 138     4.933      86
    ## 139     2.033      53
    ## 140     3.733      79
    ## 141     4.233      81
    ## 142     2.233      60
    ## 143     4.533      82
    ## 144     4.817      77
    ## 145     4.333      76
    ## 146     1.983      59
    ## 147     4.633      80
    ## 148     2.017      49
    ## 149     5.100      96
    ## 150     1.800      53
    ## 151     5.033      77
    ## 152     4.000      77
    ## 153     2.400      65
    ## 154     4.600      81
    ## 155     3.567      71
    ## 156     4.000      70
    ## 157     4.500      81
    ## 158     4.083      93
    ## 159     1.800      53
    ## 160     3.967      89
    ## 161     2.200      45
    ## 162     4.150      86
    ## 163     2.000      58
    ## 164     3.833      78
    ## 165     3.500      66
    ## 166     4.583      76
    ## 167     2.367      63
    ## 168     5.000      88
    ## 169     1.933      52
    ## 170     4.617      93
    ## 171     1.917      49
    ## 172     2.083      57
    ## 173     4.583      77
    ## 174     3.333      68
    ## 175     4.167      81
    ## 176     4.333      81
    ## 177     4.500      73
    ## 178     2.417      50
    ## 179     4.000      85
    ## 180     4.167      74
    ## 181     1.883      55
    ## 182     4.583      77
    ## 183     4.250      83
    ## 184     3.767      83
    ## 185     2.033      51
    ## 186     4.433      78
    ## 187     4.083      84
    ## 188     1.833      46
    ## 189     4.417      83
    ## 190     2.183      55
    ## 191     4.800      81
    ## 192     1.833      57
    ## 193     4.800      76
    ## 194     4.100      84
    ## 195     3.966      77
    ## 196     4.233      81
    ## 197     3.500      87
    ## 198     4.366      77
    ## 199     2.250      51
    ## 200     4.667      78
    ## 201     2.100      60
    ## 202     4.350      82
    ## 203     4.133      91
    ## 204     1.867      53
    ## 205     4.600      78
    ## 206     1.783      46
    ## 207     4.367      77
    ## 208     3.850      84
    ## 209     1.933      49
    ## 210     4.500      83
    ## 211     2.383      71
    ## 212     4.700      80
    ## 213     1.867      49
    ## 214     3.833      75
    ## 215     3.417      64
    ## 216     4.233      76
    ## 217     2.400      53
    ## 218     4.800      94
    ## 219     2.000      55
    ## 220     4.150      76
    ## 221     1.867      50
    ## 222     4.267      82
    ## 223     1.750      54
    ## 224     4.483      75
    ## 225     4.000      78
    ## 226     4.117      79
    ## 227     4.083      78
    ## 228     4.267      78
    ## 229     3.917      70
    ## 230     4.550      79
    ## 231     4.083      70
    ## 232     2.417      54
    ## 233     4.183      86
    ## 234     2.217      50
    ## 235     4.450      90
    ## 236     1.883      54
    ## 237     1.850      54
    ## 238     4.283      77
    ## 239     3.950      79
    ## 240     2.333      64
    ## 241     4.150      75
    ## 242     2.350      47
    ## 243     4.933      86
    ## 244     2.900      63
    ## 245     4.583      85
    ## 246     3.833      82
    ## 247     2.083      57
    ## 248     4.367      82
    ## 249     2.133      67
    ## 250     4.350      74
    ## 251     2.200      54
    ## 252     4.450      83
    ## 253     3.567      73
    ## 254     4.500      73
    ## 255     4.150      88
    ## 256     3.817      80
    ## 257     3.917      71
    ## 258     4.450      83
    ## 259     2.000      56
    ## 260     4.283      79
    ## 261     4.767      78
    ## 262     4.533      84
    ## 263     1.850      58
    ## 264     4.250      83
    ## 265     1.983      43
    ## 266     2.250      60
    ## 267     4.750      75
    ## 268     4.117      81
    ## 269     2.150      46
    ## 270     4.417      90
    ## 271     1.817      46
    ## 272     4.467      74

``` r
ggplot(data=faithful)+
  geom_histogram(mapping=aes(x=eruptions), binwidth = 0.25)
```

![](Data-Exploration.md_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->

Many of the questions above will prompt you to explore a relationship
*between* variables, for example, to see if the values of one variable
can explain the behavior of another variable. We’ll get to that shortly.

### Unusual values

Outliers are observations that are unusual; data points that don’t seem
to fit the pattern. Sometimes outliers are data entry errors; other
times outliers suggest important new science. When you have a lot of
data, outliers are sometimes difficult to see in a histogram. For
example, take the distribution of the `y` variable from the diamonds
dataset. The only evidence of outliers is the unusually wide limits on
the x-axis.

``` r
ggplot(data=diamonds)+
  geom_histogram(mapping=aes(x=y), binwidth=0.01             )
```

![](Data-Exploration.md_files/figure-gfm/unnamed-chunk-10-1.png)<!-- -->

There are so many observations in the common bins that the rare bins are
so short that you can’t see them (although maybe if you stare intently
at 0 you’ll spot something). To make it easy to see the unusual values,
we need to zoom to small values of the y-axis with `coord_cartesian()`:

``` r
ggplot(data=diamonds)+
  geom_histogram(mapping=aes(x=y),binwidth=0.5)+
  coord_cartesian(ylim=c(0,50))
```

![](Data-Exploration.md_files/figure-gfm/unnamed-chunk-11-1.png)<!-- -->

(`coord_cartesian()` also has an `xlim()` argument for when you need to
zoom into the x-axis. ggplot2 also has `xlim()` and `ylim()` functions
that work slightly differently: they throw away the data outside the
limits.)

This allows us to see that there are three unusual values: 0, \~30, and
\~60. We pluck them out with dplyr:

``` r
unusual <- diamonds %>% 
  filter(y < 3 | y > 20) %>% 
  select(price, x, y, z) %>%
  arrange(y)
unusual
```

    ## # A tibble: 9 x 4
    ##   price     x     y     z
    ##   <int> <dbl> <dbl> <dbl>
    ## 1  5139  0      0    0   
    ## 2  6381  0      0    0   
    ## 3 12800  0      0    0   
    ## 4 15686  0      0    0   
    ## 5 18034  0      0    0   
    ## 6  2130  0      0    0   
    ## 7  2130  0      0    0   
    ## 8  2075  5.15  31.8  5.12
    ## 9 12210  8.09  58.9  8.06

The `y` variable measures one of the three dimensions of these diamonds,
in mm. We know that diamonds can’t have a width of 0mm, so these values
must be incorrect. We might also suspect that measurements of 32mm and
59mm are implausible: those diamonds are over an inch long, but don’t
cost hundreds of thousands of dollars\!

It’s good practice to repeat your analysis with and without the
outliers. If they have minimal effect on the results, and you can’t
figure out why they’re there, it’s reasonable to replace them with
missing values, and move on. However, if they have a substantial effect
on your results, you shouldn’t drop them without justification. You’ll
need to figure out what caused them (e.g. a data entry error) and
disclose that you removed them in your write-up.

### Exercises

1.  Explore the distribution of each of the `x`, `y`, and `z` variables
    in `diamonds`. What do you learn? Think about a diamond and how you
    might decide which dimension is the length, width, and depth.

2.  Explore the distribution of `price`. Do you discover anything
    unusual or surprising? (Hint: Carefully think about the `binwidth`
    and make sure you try a wide range of values.)

3.  How many diamonds are 0.99 carat? How many are 1 carat? What do you
    think is the cause of the difference?

4.  Compare and contrast `coord_cartesian()` vs `xlim()` or `ylim()`
    when zooming in on a histogram. What happens if you leave `binwidth`
    unset? What happens if you try and zoom so only half a bar shows?

## Missing values

If you’ve encountered unusual values in your dataset, and simply want to
move on to the rest of your analysis, you have two options.

1.  Drop the entire row with the strange values:

<!-- end list -->

``` r
diamonds2<-diamonds %>%
  filter(between(y,3,20))
```

    I don't recommend this option because just because one measurement
    is invalid, doesn't mean all the measurements are. Additionally, if you
    have low quality data, by time that you've applied this approach to every
    variable you might find that you don't have any data left!

1.  Instead, I recommend replacing the unusual values with missing
    values. The easiest way to do this is to use `mutate()` to replace
    the variable with a modified copy. You can use the `ifelse()`
    function to replace unusual values with `NA`:

<!-- end list -->

``` r
dimaonds2<-diamonds %>%
  mutate(y=ifelse(y<3|y>20, NA, y))
```

`ifelse()` has three arguments. The first argument `test` should be a
logical vector. The result will contain the value of the second
argument, `yes`, when `test` is `TRUE`, and the value of the third
argument, `no`, when it is false. Alternatively to ifelse, use
`dplyr::case_when()`. `case_when()` is particularly useful inside mutate
when you want to create a new variable that relies on a complex
combination of existing variables.

Like R, ggplot2 subscribes to the philosophy that missing values should
never silently go missing. It’s not obvious where you should plot
missing values, so ggplot2 doesn’t include them in the plot, but it does
warn that they’ve been removed:

``` r
ggplot(data=dimaonds2)+
  geom_point(mapping=aes(x=x, y=y)) 
```

    ## Warning: Removed 9 rows containing missing values (geom_point).

![](Data-Exploration.md_files/figure-gfm/unnamed-chunk-17-1.png)<!-- -->

To suppress that warning, set `na.rm = TRUE`:

``` r
ggplot(data=diamonds2, mapping=aes(x=x, y=y))+
  geom_point(na.rm=TRUE)
```

Other times you want to understand what makes observations with missing
values different to observations with recorded values. For example, in
`nycflights13::flights`, missing values in the `dep_time` variable
indicate that the flight was cancelled. So you might want to compare the
scheduled departure times for cancelled and non-cancelled times. You can
do this by making a new variable with `is.na()`.

However this plot isn’t great because there are many more non-cancelled
flights than cancelled flights. In the next section we’ll explore some
techniques for improving this comparison.

### Exercises

1.  What happens to missing values in a histogram? What happens to
    missing values in a bar chart? Why is there a difference?

2.  What does `na.rm = TRUE` do in `mean()` and `sum()`?

## Covariation

If variation describes the behavior *within* a variable, covariation
describes the behavior *between* variables. **Covariation** is the
tendency for the values of two or more variables to vary together in a
related way. The best way to spot covariation is to visualise the
relationship between two or more variables. How you do that should again
depend on the type of variables involved.

### A categorical and continuous variable

It’s common to want to explore the distribution of a continuous variable
broken down by a categorical variable, as in the previous frequency
polygon. The default appearance of `geom_freqpoly()` is not that useful
for that sort of comparison because the height is given by the count.
That means if one of the groups is much smaller than the others, it’s
hard to see the differences in shape. For example, let’s explore how the
price of a diamond varies with its quality:

``` r
ggplot(data=diamonds)+
  geom_freqpoly(mapping=aes(x=price, color=cut), binwidth=500)
```

![](Data-Exploration.md_files/figure-gfm/unnamed-chunk-20-1.png)<!-- -->

It’s hard to see the difference in distribution because the overall
counts differ so much:

``` r
ggplot(diamonds)+
  geom_bar(mapping=aes(x=cut))
```

![](Data-Exploration.md_files/figure-gfm/unnamed-chunk-21-1.png)<!-- -->

To make the comparison easier we need to swap what is displayed on the
y-axis. Instead of displaying count, we’ll display **density**, which is
the count standardised so that the area under each frequency polygon is
one.

``` r
ggplot(data = diamonds, mapping = aes(x = price, y = ..density..)) + 
  geom_freqpoly(mapping = aes(colour = cut), binwidth = 500)
```

![](Data-Exploration.md_files/figure-gfm/unnamed-chunk-22-1.png)<!-- -->

There’s something rather surprising about this plot - it appears that
fair diamonds (the lowest quality) have the highest average price\! But
maybe that’s because frequency polygons are a little hard to interpret -
there’s a lot going on in this plot.

Another alternative to display the distribution of a continuous variable
broken down by a categorical variable is the boxplot. A **boxplot** is a
type of visual shorthand for a distribution of values that is popular
among statisticians. Each boxplot consists of:

  - A box that stretches from the 25th percentile of the distribution to
    the 75th percentile, a distance known as the interquartile range
    (IQR). In the middle of the box is a line that displays the median,
    i.e. 50th percentile, of the distribution. These three lines give
    you a sense of the spread of the distribution and whether or not the
    distribution is symmetric about the median or skewed to one side.

  - Visual points that display observations that fall more than 1.5
    times the IQR from either edge of the box. These outlying points are
    unusual so are plotted individually.

  - A line (or whisker) that extends from each end of the box and goes
    to the  
    farthest non-outlier point in the distribution.

Let’s take a look at the distribution of price by cut using
`geom_boxplot()`:

``` r
ggplot(data=diamonds, mapping=aes(x=cut, y=price))+
  geom_boxplot()
```

![](Data-Exploration.md_files/figure-gfm/unnamed-chunk-23-1.png)<!-- -->

We see much less information about the distribution, but the boxplots
are much more compact so we can more easily compare them (and fit more
on one plot). It supports the counterintuitive finding that better
quality diamonds are cheaper on average\! In the exercises, you’ll be
challenged to figure out why.

`cut` is an ordered factor: fair is worse than good, which is worse than
very good and so on. Many categorical variables don’t have such an
intrinsic order, so you might want to reorder them to make a more
informative display. One way to do that is with the `reorder()`
function.

For example, take the `class` variable in the `mpg` dataset. You might
be interested to know how highway mileage varies across classes:

``` r
ggplot(data=mpg, mapping=aes(x=class, y=hwy))+
  geom_boxplot()
```

![](Data-Exploration.md_files/figure-gfm/unnamed-chunk-24-1.png)<!-- -->

To make the trend easier to see, we can reorder `class` based on the
median value of `hwy`:

``` r
ggplot(data=mpg)+geom_boxplot(mapping=aes(x=reorder(class, hwy, FUN=median), y=hwy))
```

![](Data-Exploration.md_files/figure-gfm/unnamed-chunk-25-1.png)<!-- -->

If you have long variable names, `geom_boxplot()` will work better if
you flip it 90°. You can do that with `coord_flip()`.

``` r
ggplot(data = mpg) +
  geom_boxplot(mapping = aes(x = reorder(class, hwy, FUN = median), y = hwy)) +
  coord_flip()
```

![](Data-Exploration.md_files/figure-gfm/unnamed-chunk-26-1.png)<!-- -->

#### Exercises

1.  Use what you’ve learned to improve the visualisation of the
    departure times of cancelled vs. non-cancelled flights.

2.  What variable in the diamonds dataset is most important for
    predicting the price of a diamond? How is that variable correlated
    with cut? Why does the combination of those two relationships lead
    to lower quality diamonds being more expensive?

3.  Install the ggstance package, and create a horizontal boxplot. How
    does this compare to using `coord_flip()`?

4.  One problem with boxplots is that they were developed in an era of
    much smaller datasets and tend to display a prohibitively large
    number of “outlying values”. One approach to remedy this problem is
    the letter value plot. Install the lvplot package, and try using
    `geom_lv()` to display the distribution of price vs cut. What do you
    learn? How do you interpret the plots?

5.  Compare and contrast `geom_violin()` with a facetted
    `geom_histogram()`, or a coloured `geom_freqpoly()`. What are the
    pros and cons of each method?

6.  If you have a small dataset, it’s sometimes useful to use
    `geom_jitter()` to see the relationship between a continuous and
    categorical variable. The ggbeeswarm package provides a number of
    methods similar to `geom_jitter()`. List them and briefly describe
    what each one does.

### Two categorical variables

To visualise the covariation between categorical variables, you’ll need
to count the number of observations for each combination. One way to do
that is to rely on the built-in `geom_count()`:

``` r
ggplot(diamonds, aes(x=cut, y=color))+
  geom_count()
```

![](Data-Exploration.md_files/figure-gfm/unnamed-chunk-27-1.png)<!-- -->

The size of each circle in the plot displays how many observations
occurred at each combination of values. Covariation will appear as a
strong correlation between specific x values and specific y values.

Another approach is to compute the count with dplyr:

``` r
diamonds %>%
  count(color, cut)
```

    ## # A tibble: 35 x 3
    ##    color cut           n
    ##    <ord> <ord>     <int>
    ##  1 D     Fair        163
    ##  2 D     Good        662
    ##  3 D     Very Good  1513
    ##  4 D     Premium    1603
    ##  5 D     Ideal      2834
    ##  6 E     Fair        224
    ##  7 E     Good        933
    ##  8 E     Very Good  2400
    ##  9 E     Premium    2337
    ## 10 E     Ideal      3903
    ## # … with 25 more rows

Then visualise with `geom_tile()` and the fill aesthetic:

``` r
diamonds %>% 
  count(color, cut) %>%
  ggplot(aes(x=color, y=cut)) + 
  geom_tile(mapping=aes(fill=n))
```

![](Data-Exploration.md_files/figure-gfm/unnamed-chunk-29-1.png)<!-- -->

If the categorical variables are unordered, you might want to use the
seriation package to simultaneously reorder the rows and columns in
order to more clearly reveal interesting patterns. For larger plots, you
might want to try the d3heatmap or heatmaply packages, which create
interactive plots.

#### Exercises

1.  How could you rescale the count dataset above to more clearly show
    the distribution of cut within colour, or colour within cut?

2.  Use `geom_tile()` together with dplyr to explore how average flight
    delays vary by destination and month of year. What makes the plot
    difficult to read? How could you improve it?

3.  Why is it slightly better to use `aes(x = color, y = cut)` rather
    than `aes(x = cut, y = color)` in the example above?

### Two continuous variables

You’ve already seen one great way to visualise the covariation between
two continuous variables: draw a scatterplot with `geom_point()`. You
can see covariation as a pattern in the points. For example, you can see
an exponential relationship between the carat size and price of a
diamond.

``` r
ggplot(diamonds, aes(x=carat, y=price))+
  geom_point() 
```

![](Data-Exploration.md_files/figure-gfm/unnamed-chunk-30-1.png)<!-- -->

Scatterplots become less useful as the size of your dataset grows,
because points begin to overplot, and pile up into areas of uniform
black (as above). You’ve already seen one way to fix the problem: using
the `alpha` aesthetic to add transparency.

``` r
ggplot(diamonds, aes(c=carat, y=price))+
  geom_point(mapping=aes(x=carat, y=price), alpha=.3)
```

![](Data-Exploration.md_files/figure-gfm/unnamed-chunk-31-1.png)<!-- -->

But using transparency can be challenging for very large datasets.
Another solution is to use bin. Previously you used `geom_histogram()`
and `geom_freqpoly()` to bin in one dimension. Now you’ll learn how to
use `geom_bin2d()` and `geom_hex()` to bin in two dimensions.

`geom_bin2d()` and `geom_hex()` divide the coordinate plane into 2d bins
and then use a fill color to display how many points fall into each bin.
`geom_bin2d()` creates rectangular bins. `geom_hex()` creates hexagonal
bins. You will need to install the hexbin package to use `geom_hex()`.

``` r
ggplot(smaller, aes(x=carat, y=price))+
  geom_bin2d()
```

<img src="Data-Exploration.md_files/figure-gfm/unnamed-chunk-32-1.png" width="50%" />

``` r
ggplot(smaller, aes(x=carat, y=price))+
  geom_hex()
```

![](Data-Exploration.md_files/figure-gfm/unnamed-chunk-33-1.png)<!-- -->
Another option is to bin one continuous variable so it acts like a
categorical variable. Then you can use one of the techniques for
visualising the combination of a categorical and a continuous variable
that you learned about. For example, you could bin `carat` and then for
each group, display a boxplot:

``` r
ggplot(smaller, aes(x=carat, y=price))+
  geom_boxplot(aes(group=cut_width(carat, 0.1)))
```

![](Data-Exploration.md_files/figure-gfm/unnamed-chunk-34-1.png)<!-- -->

`cut_width(x, width)`, as used above, divides `x` into bins of width
`width`. By default, boxplots look roughly the same (apart from number
of outliers) regardless of how many observations there are, so it’s
difficult to tell that each boxplot summarises a different number of
points. One way to show that is to make the width of the boxplot
proportional to the number of points with `varwidth = TRUE`.

Another approach is to display approximately the same number of points
in each bin. That’s the job of `cut_number()`:

#### Exercises

1.  Instead of summarising the conditional distribution with a boxplot,
    you could use a frequency polygon. What do you need to consider when
    using `cut_width()` vs `cut_number()`? How does that impact a
    visualisation of the 2d distribution of `carat` and `price`?

2.  Visualise the distribution of carat, partitioned by price.

3.  How does the price distribution of very large diamonds compare to
    small diamonds? Is it as you expect, or does it surprise you?

4.  Combine two of the techniques you’ve learned to visualise the
    combined distribution of cut, carat, and price.

5.  Two dimensional plots reveal outliers that are not visible in one
    dimensional plots. For example, some points in the plot below have
    an unusual combination of `x` and `y` values, which makes the points
    outliers even though their `x` and `y` values appear normal when
    examined separately.
    
    Why is a scatterplot a better display than a binned plot for this
    case?

## Patterns and models

Patterns in your data provide clues about relationships. If a systematic
relationship exists between two variables it will appear as a pattern in
the data. If you spot a pattern, ask yourself:

  - Could this pattern be due to coincidence (i.e. random chance)?

  - How can you describe the relationship implied by the pattern?

  - How strong is the relationship implied by the pattern?

  - What other variables might affect the relationship?

  - Does the relationship change if you look at individual subgroups of
    the data?

A scatterplot of Old Faithful eruption lengths versus the wait time
between eruptions shows a pattern: longer wait times are associated with
longer eruptions. The scatterplot also displays the two clusters that we
noticed above.

Patterns provide one of the most useful tools for data scientists
because they reveal covariation. If you think of variation as a
phenomenon that creates uncertainty, covariation is a phenomenon that
reduces it. If two variables covary, you can use the values of one
variable to make better predictions about the values of the second. If
the covariation is due to a causal relationship (a special case), then
you can use the value of one variable to control the value of the
second.

Models are a tool for extracting patterns out of data. For example,
consider the diamonds data. It’s hard to understand the relationship
between cut and price, because cut and carat, and carat and price are
tightly related. It’s possible to use a model to remove the very strong
relationship between price and carat so we can explore the subtleties
that remain. The following code fits a model that predicts `price` from
`carat` and then computes the residuals (the difference between the
predicted value and the actual value). The residuals give us a view of
the price of the diamond, once the effect of carat has been removed.

Once you’ve removed the strong relationship between carat and price, you
can see what you expect in the relationship between cut and price:
relative to their size, better quality diamonds are more expensive.

You’ll learn how models, and the modelr package, work in the final part
of the book, [model](#model-intro). We’re saving modelling for later
because understanding what models are and how they work is easiest once
you have tools of data wrangling and programming in hand.

## ggplot2 calls

As we move on from these introductory chapters, we’ll transition to a
more concise expression of ggplot2 code. So far we’ve been very
explicit, which is helpful when you are learning:

Typically, the first one or two arguments to a function are so important
that you should know them by heart. The first two arguments to
`ggplot()` are `data` and `mapping`, and the first two arguments to
`aes()` are `x` and `y`. In the remainder of the book, we won’t supply
those names. That saves typing, and, by reducing the amount of
boilerplate, makes it easier to see what’s different between plots.
That’s a really important programming concern that we’ll come back in
\[functions\].

Rewriting the previous plot more concisely yields:

Sometimes we’ll turn the end of a pipeline of data transformation into a
plot. Watch for the transition from `%>%` to `+`. I wish this transition
wasn’t necessary but unfortunately ggplot2 was created before the pipe
was discovered.

## Learning more

If you want to learn more about the mechanics of ggplot2, I’d highly
recommend grabbing a copy of the ggplot2 book:
<https://amzn.com/331924275X>. It’s been recently updated, so it
includes dplyr and tidyr code, and has much more space to explore all
the facets of visualisation. Unfortunately the book isn’t generally
available for free, but if you have a connection to a university you can
probably get an electronic version for free through SpringerLink.

Another useful resource is the [*R Graphics
Cookbook*](https://amzn.com/1449316956) by Winston Chang. Much of the
contents are available online at <http://www.cookbook-r.com/Graphs/>.

I also recommend [*Graphical Data Analysis with
R*](https://amzn.com/1498715230), by Antony Unwin. This is a book-length
treatment similar to the material covered in this chapter, but has the
space to go into much greater depth.
