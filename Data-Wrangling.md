Data Transformation
================
Declan Little
9/18/2020

# 5.1 Introduction

## 5.1.1 Prerequisites

``` r
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

``` r
library(nycflights13)
```

### 5.1.2

``` r
flights
```

    ## # A tibble: 336,776 x 19
    ##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
    ##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
    ##  1  2013     1     1      517            515         2      830            819
    ##  2  2013     1     1      533            529         4      850            830
    ##  3  2013     1     1      542            540         2      923            850
    ##  4  2013     1     1      544            545        -1     1004           1022
    ##  5  2013     1     1      554            600        -6      812            837
    ##  6  2013     1     1      554            558        -4      740            728
    ##  7  2013     1     1      555            600        -5      913            854
    ##  8  2013     1     1      557            600        -3      709            723
    ##  9  2013     1     1      557            600        -3      838            846
    ## 10  2013     1     1      558            600        -2      753            745
    ## # … with 336,766 more rows, and 11 more variables: arr_delay <dbl>,
    ## #   carrier <chr>, flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
    ## #   air_time <dbl>, distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>

### 5.1.3 dplyr

\-verbs

filter arrange select mutate summarize

verb(database,whatToDo?)

# 5.2 Filter Rows with Filter()

Flights that occured in Jan 1st

filter(flights, month==1, day==1)

``` r
filter(flights, month==1, day==1)
```

    ## # A tibble: 842 x 19
    ##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
    ##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
    ##  1  2013     1     1      517            515         2      830            819
    ##  2  2013     1     1      533            529         4      850            830
    ##  3  2013     1     1      542            540         2      923            850
    ##  4  2013     1     1      544            545        -1     1004           1022
    ##  5  2013     1     1      554            600        -6      812            837
    ##  6  2013     1     1      554            558        -4      740            728
    ##  7  2013     1     1      555            600        -5      913            854
    ##  8  2013     1     1      557            600        -3      709            723
    ##  9  2013     1     1      557            600        -3      838            846
    ## 10  2013     1     1      558            600        -2      753            745
    ## # … with 832 more rows, and 11 more variables: arr_delay <dbl>, carrier <chr>,
    ## #   flight <int>, tailnum <chr>, origin <chr>, dest <chr>, air_time <dbl>,
    ## #   distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>

``` r
(jan1 <- filter(flights, month==1, day==1))
```

    ## # A tibble: 842 x 19
    ##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
    ##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
    ##  1  2013     1     1      517            515         2      830            819
    ##  2  2013     1     1      533            529         4      850            830
    ##  3  2013     1     1      542            540         2      923            850
    ##  4  2013     1     1      544            545        -1     1004           1022
    ##  5  2013     1     1      554            600        -6      812            837
    ##  6  2013     1     1      554            558        -4      740            728
    ##  7  2013     1     1      555            600        -5      913            854
    ##  8  2013     1     1      557            600        -3      709            723
    ##  9  2013     1     1      557            600        -3      838            846
    ## 10  2013     1     1      558            600        -2      753            745
    ## # … with 832 more rows, and 11 more variables: arr_delay <dbl>, carrier <chr>,
    ## #   flight <int>, tailnum <chr>, origin <chr>, dest <chr>, air_time <dbl>,
    ## #   distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>

``` r
filter(flights, month==12, day==25)
```

    ## # A tibble: 719 x 19
    ##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
    ##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
    ##  1  2013    12    25      456            500        -4      649            651
    ##  2  2013    12    25      524            515         9      805            814
    ##  3  2013    12    25      542            540         2      832            850
    ##  4  2013    12    25      546            550        -4     1022           1027
    ##  5  2013    12    25      556            600        -4      730            745
    ##  6  2013    12    25      557            600        -3      743            752
    ##  7  2013    12    25      557            600        -3      818            831
    ##  8  2013    12    25      559            600        -1      855            856
    ##  9  2013    12    25      559            600        -1      849            855
    ## 10  2013    12    25      600            600         0      850            846
    ## # … with 709 more rows, and 11 more variables: arr_delay <dbl>, carrier <chr>,
    ## #   flight <int>, tailnum <chr>, origin <chr>, dest <chr>, air_time <dbl>,
    ## #   distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>

``` r
(santa<-filter(flights, month==12, day==25))
```

    ## # A tibble: 719 x 19
    ##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
    ##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
    ##  1  2013    12    25      456            500        -4      649            651
    ##  2  2013    12    25      524            515         9      805            814
    ##  3  2013    12    25      542            540         2      832            850
    ##  4  2013    12    25      546            550        -4     1022           1027
    ##  5  2013    12    25      556            600        -4      730            745
    ##  6  2013    12    25      557            600        -3      743            752
    ##  7  2013    12    25      557            600        -3      818            831
    ##  8  2013    12    25      559            600        -1      855            856
    ##  9  2013    12    25      559            600        -1      849            855
    ## 10  2013    12    25      600            600         0      850            846
    ## # … with 709 more rows, and 11 more variables: arr_delay <dbl>, carrier <chr>,
    ## #   flight <int>, tailnum <chr>, origin <chr>, dest <chr>, air_time <dbl>,
    ## #   distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>

##### 5.2.1 Comparisons

\==, \>, \<, \<=, \!

``` r
filter(flights, month==1)
```

    ## # A tibble: 27,004 x 19
    ##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
    ##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
    ##  1  2013     1     1      517            515         2      830            819
    ##  2  2013     1     1      533            529         4      850            830
    ##  3  2013     1     1      542            540         2      923            850
    ##  4  2013     1     1      544            545        -1     1004           1022
    ##  5  2013     1     1      554            600        -6      812            837
    ##  6  2013     1     1      554            558        -4      740            728
    ##  7  2013     1     1      555            600        -5      913            854
    ##  8  2013     1     1      557            600        -3      709            723
    ##  9  2013     1     1      557            600        -3      838            846
    ## 10  2013     1     1      558            600        -2      753            745
    ## # … with 26,994 more rows, and 11 more variables: arr_delay <dbl>,
    ## #   carrier <chr>, flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
    ## #   air_time <dbl>, distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>

USE == instead of =

``` r
sin(pi/2)
```

    ## [1] 1

``` r
cos(pi/2)
```

    ## [1] 6.123234e-17

Computer estimates, it doesnt know tht cos(pi/2)=o

``` r
near(cos(pi/2), 0)
```

    ## [1] TRUE

##### 5.2.2 Logical Operators

or —–\> | and —–\> & not —–\> \!

Flights that departed in november **or** december

**word** is bolded

``` r
filter(flights, month==11|month==12)
```

    ## # A tibble: 55,403 x 19
    ##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
    ##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
    ##  1  2013    11     1        5           2359         6      352            345
    ##  2  2013    11     1       35           2250       105      123           2356
    ##  3  2013    11     1      455            500        -5      641            651
    ##  4  2013    11     1      539            545        -6      856            827
    ##  5  2013    11     1      542            545        -3      831            855
    ##  6  2013    11     1      549            600       -11      912            923
    ##  7  2013    11     1      550            600       -10      705            659
    ##  8  2013    11     1      554            600        -6      659            701
    ##  9  2013    11     1      554            600        -6      826            827
    ## 10  2013    11     1      554            600        -6      749            751
    ## # … with 55,393 more rows, and 11 more variables: arr_delay <dbl>,
    ## #   carrier <chr>, flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
    ## #   air_time <dbl>, distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>

may, june, july, august,

summer\<-filter(flights,month==5|month=6|month=7|month=8|)

``` r
(summer<-filter(flights,month==5|month==6|month==7|month==8))
```

    ## # A tibble: 115,791 x 19
    ##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
    ##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
    ##  1  2013     5     1        9           1655       434      308           2020
    ##  2  2013     5     1      451            500        -9      641            640
    ##  3  2013     5     1      537            540        -3      836            840
    ##  4  2013     5     1      544            545        -1      818            827
    ##  5  2013     5     1      548            600       -12      831            854
    ##  6  2013     5     1      549            600       -11      804            810
    ##  7  2013     5     1      553            600        -7      700            712
    ##  8  2013     5     1      553            600        -7      655            701
    ##  9  2013     5     1      554            600        -6      731            756
    ## 10  2013     5     1      554            600        -6      707            725
    ## # … with 115,781 more rows, and 11 more variables: arr_delay <dbl>,
    ## #   carrier <chr>, flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
    ## #   air_time <dbl>, distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>

``` r
filter(flights, month %in% c(5,6,7,8))
```

    ## # A tibble: 115,791 x 19
    ##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
    ##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
    ##  1  2013     5     1        9           1655       434      308           2020
    ##  2  2013     5     1      451            500        -9      641            640
    ##  3  2013     5     1      537            540        -3      836            840
    ##  4  2013     5     1      544            545        -1      818            827
    ##  5  2013     5     1      548            600       -12      831            854
    ##  6  2013     5     1      549            600       -11      804            810
    ##  7  2013     5     1      553            600        -7      700            712
    ##  8  2013     5     1      553            600        -7      655            701
    ##  9  2013     5     1      554            600        -6      731            756
    ## 10  2013     5     1      554            600        -6      707            725
    ## # … with 115,781 more rows, and 11 more variables: arr_delay <dbl>,
    ## #   carrier <chr>, flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
    ## #   air_time <dbl>, distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>

flights that weren’t delayed (on departure or arrival) by more than two
hours

filter(flights, dep\_delay\<120|arr\_delay\<120)

``` r
filter(flights, dep_delay<120|arr_delay<120)
```

    ## # A tibble: 319,911 x 19
    ##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
    ##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
    ##  1  2013     1     1      517            515         2      830            819
    ##  2  2013     1     1      533            529         4      850            830
    ##  3  2013     1     1      542            540         2      923            850
    ##  4  2013     1     1      544            545        -1     1004           1022
    ##  5  2013     1     1      554            600        -6      812            837
    ##  6  2013     1     1      554            558        -4      740            728
    ##  7  2013     1     1      555            600        -5      913            854
    ##  8  2013     1     1      557            600        -3      709            723
    ##  9  2013     1     1      557            600        -3      838            846
    ## 10  2013     1     1      558            600        -2      753            745
    ## # … with 319,901 more rows, and 11 more variables: arr_delay <dbl>,
    ## #   carrier <chr>, flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
    ## #   air_time <dbl>, distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>

``` r
filter(flights, !dep_delay>120|arr_delay>120)
```

    ## # A tibble: 327,133 x 19
    ##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
    ##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
    ##  1  2013     1     1      517            515         2      830            819
    ##  2  2013     1     1      533            529         4      850            830
    ##  3  2013     1     1      542            540         2      923            850
    ##  4  2013     1     1      544            545        -1     1004           1022
    ##  5  2013     1     1      554            600        -6      812            837
    ##  6  2013     1     1      554            558        -4      740            728
    ##  7  2013     1     1      555            600        -5      913            854
    ##  8  2013     1     1      557            600        -3      709            723
    ##  9  2013     1     1      557            600        -3      838            846
    ## 10  2013     1     1      558            600        -2      753            745
    ## # … with 327,123 more rows, and 11 more variables: arr_delay <dbl>,
    ## #   carrier <chr>, flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
    ## #   air_time <dbl>, distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>

Negating it changes it from or to and, and from \< to \>

##### 5.2.3 Missing Values

One important feature of R that can make comparisons tricky are missing
values, NAs (“not availables”) NA represents an unkown value or missing
values are “contagious”: almost any operation invloving an unknown value
will also be unknown.

``` r
100==NA
```

    ## [1] NA

``` r
2>NA
```

    ## [1] NA

``` r
x<-NA
is.na(x)
```

    ## [1] TRUE

``` r
is.na(10)
```

    ## [1] FALSE

How do I get rid of NA’s in my dataset

``` r
filter(flights, is.na(dep_time))
```

    ## # A tibble: 8,255 x 19
    ##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
    ##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
    ##  1  2013     1     1       NA           1630        NA       NA           1815
    ##  2  2013     1     1       NA           1935        NA       NA           2240
    ##  3  2013     1     1       NA           1500        NA       NA           1825
    ##  4  2013     1     1       NA            600        NA       NA            901
    ##  5  2013     1     2       NA           1540        NA       NA           1747
    ##  6  2013     1     2       NA           1620        NA       NA           1746
    ##  7  2013     1     2       NA           1355        NA       NA           1459
    ##  8  2013     1     2       NA           1420        NA       NA           1644
    ##  9  2013     1     2       NA           1321        NA       NA           1536
    ## 10  2013     1     2       NA           1545        NA       NA           1910
    ## # … with 8,245 more rows, and 11 more variables: arr_delay <dbl>,
    ## #   carrier <chr>, flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
    ## #   air_time <dbl>, distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>

``` r
filter(flights, !is.na(dep_time))
```

    ## # A tibble: 328,521 x 19
    ##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
    ##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
    ##  1  2013     1     1      517            515         2      830            819
    ##  2  2013     1     1      533            529         4      850            830
    ##  3  2013     1     1      542            540         2      923            850
    ##  4  2013     1     1      544            545        -1     1004           1022
    ##  5  2013     1     1      554            600        -6      812            837
    ##  6  2013     1     1      554            558        -4      740            728
    ##  7  2013     1     1      555            600        -5      913            854
    ##  8  2013     1     1      557            600        -3      709            723
    ##  9  2013     1     1      557            600        -3      838            846
    ## 10  2013     1     1      558            600        -2      753            745
    ## # … with 328,511 more rows, and 11 more variables: arr_delay <dbl>,
    ## #   carrier <chr>, flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
    ## #   air_time <dbl>, distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>

``` r
(withoutNA<-filter(flights, !is.na(dep_time)))
```

    ## # A tibble: 328,521 x 19
    ##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
    ##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
    ##  1  2013     1     1      517            515         2      830            819
    ##  2  2013     1     1      533            529         4      850            830
    ##  3  2013     1     1      542            540         2      923            850
    ##  4  2013     1     1      544            545        -1     1004           1022
    ##  5  2013     1     1      554            600        -6      812            837
    ##  6  2013     1     1      554            558        -4      740            728
    ##  7  2013     1     1      555            600        -5      913            854
    ##  8  2013     1     1      557            600        -3      709            723
    ##  9  2013     1     1      557            600        -3      838            846
    ## 10  2013     1     1      558            600        -2      753            745
    ## # … with 328,511 more rows, and 11 more variables: arr_delay <dbl>,
    ## #   carrier <chr>, flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
    ## #   air_time <dbl>, distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>

``` r
withoutNA
```

    ## # A tibble: 328,521 x 19
    ##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
    ##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
    ##  1  2013     1     1      517            515         2      830            819
    ##  2  2013     1     1      533            529         4      850            830
    ##  3  2013     1     1      542            540         2      923            850
    ##  4  2013     1     1      544            545        -1     1004           1022
    ##  5  2013     1     1      554            600        -6      812            837
    ##  6  2013     1     1      554            558        -4      740            728
    ##  7  2013     1     1      555            600        -5      913            854
    ##  8  2013     1     1      557            600        -3      709            723
    ##  9  2013     1     1      557            600        -3      838            846
    ## 10  2013     1     1      558            600        -2      753            745
    ## # … with 328,511 more rows, and 11 more variables: arr_delay <dbl>,
    ## #   carrier <chr>, flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
    ## #   air_time <dbl>, distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>

``` r
filter(withoutNA, is.na(dep_time))
```

    ## # A tibble: 0 x 19
    ## # … with 19 variables: year <int>, month <int>, day <int>, dep_time <int>,
    ## #   sched_dep_time <int>, dep_delay <dbl>, arr_time <int>,
    ## #   sched_arr_time <int>, arr_delay <dbl>, carrier <chr>, flight <int>,
    ## #   tailnum <chr>, origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>,
    ## #   hour <dbl>, minute <dbl>, time_hour <dttm>

Is empty when you check

##### 5.2.4 Exercises

> 1.  Find all flights that

1.1Had an arrival delay of two or more hours

``` r
filter(flights, arr_delay>=120)
```

    ## # A tibble: 10,200 x 19
    ##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
    ##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
    ##  1  2013     1     1      811            630       101     1047            830
    ##  2  2013     1     1      848           1835       853     1001           1950
    ##  3  2013     1     1      957            733       144     1056            853
    ##  4  2013     1     1     1114            900       134     1447           1222
    ##  5  2013     1     1     1505           1310       115     1638           1431
    ##  6  2013     1     1     1525           1340       105     1831           1626
    ##  7  2013     1     1     1549           1445        64     1912           1656
    ##  8  2013     1     1     1558           1359       119     1718           1515
    ##  9  2013     1     1     1732           1630        62     2028           1825
    ## 10  2013     1     1     1803           1620       103     2008           1750
    ## # … with 10,190 more rows, and 11 more variables: arr_delay <dbl>,
    ## #   carrier <chr>, flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
    ## #   air_time <dbl>, distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>

1.2Flew to Houston (IAH or HOU)

``` r
filter(flights, dest=="IAH"|dest=="HOU")
```

    ## # A tibble: 9,313 x 19
    ##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
    ##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
    ##  1  2013     1     1      517            515         2      830            819
    ##  2  2013     1     1      533            529         4      850            830
    ##  3  2013     1     1      623            627        -4      933            932
    ##  4  2013     1     1      728            732        -4     1041           1038
    ##  5  2013     1     1      739            739         0     1104           1038
    ##  6  2013     1     1      908            908         0     1228           1219
    ##  7  2013     1     1     1028           1026         2     1350           1339
    ##  8  2013     1     1     1044           1045        -1     1352           1351
    ##  9  2013     1     1     1114            900       134     1447           1222
    ## 10  2013     1     1     1205           1200         5     1503           1505
    ## # … with 9,303 more rows, and 11 more variables: arr_delay <dbl>,
    ## #   carrier <chr>, flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
    ## #   air_time <dbl>, distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>

``` r
filter(flights,dest%in% c("IAH","HOU"))
```

    ## # A tibble: 9,313 x 19
    ##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
    ##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
    ##  1  2013     1     1      517            515         2      830            819
    ##  2  2013     1     1      533            529         4      850            830
    ##  3  2013     1     1      623            627        -4      933            932
    ##  4  2013     1     1      728            732        -4     1041           1038
    ##  5  2013     1     1      739            739         0     1104           1038
    ##  6  2013     1     1      908            908         0     1228           1219
    ##  7  2013     1     1     1028           1026         2     1350           1339
    ##  8  2013     1     1     1044           1045        -1     1352           1351
    ##  9  2013     1     1     1114            900       134     1447           1222
    ## 10  2013     1     1     1205           1200         5     1503           1505
    ## # … with 9,303 more rows, and 11 more variables: arr_delay <dbl>,
    ## #   carrier <chr>, flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
    ## #   air_time <dbl>, distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>

1.3Were operated by United, American, or Delta

``` r
filter(flights, carrier%in% c("UA","AA","DL"))
```

    ## # A tibble: 139,504 x 19
    ##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
    ##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
    ##  1  2013     1     1      517            515         2      830            819
    ##  2  2013     1     1      533            529         4      850            830
    ##  3  2013     1     1      542            540         2      923            850
    ##  4  2013     1     1      554            600        -6      812            837
    ##  5  2013     1     1      554            558        -4      740            728
    ##  6  2013     1     1      558            600        -2      753            745
    ##  7  2013     1     1      558            600        -2      924            917
    ##  8  2013     1     1      558            600        -2      923            937
    ##  9  2013     1     1      559            600        -1      941            910
    ## 10  2013     1     1      559            600        -1      854            902
    ## # … with 139,494 more rows, and 11 more variables: arr_delay <dbl>,
    ## #   carrier <chr>, flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
    ## #   air_time <dbl>, distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>

1.4Departed in summer (July, August, and September)

``` r
filter(flights, month%in% c(7,8,9))
```

    ## # A tibble: 86,326 x 19
    ##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
    ##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
    ##  1  2013     7     1        1           2029       212      236           2359
    ##  2  2013     7     1        2           2359         3      344            344
    ##  3  2013     7     1       29           2245       104      151              1
    ##  4  2013     7     1       43           2130       193      322             14
    ##  5  2013     7     1       44           2150       174      300            100
    ##  6  2013     7     1       46           2051       235      304           2358
    ##  7  2013     7     1       48           2001       287      308           2305
    ##  8  2013     7     1       58           2155       183      335             43
    ##  9  2013     7     1      100           2146       194      327             30
    ## 10  2013     7     1      100           2245       135      337            135
    ## # … with 86,316 more rows, and 11 more variables: arr_delay <dbl>,
    ## #   carrier <chr>, flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
    ## #   air_time <dbl>, distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>

1.5Arrived more than two hours late, but didn’t leave late

``` r
filter(flights, arr_delay>120&dep_delay<=0)
```

    ## # A tibble: 29 x 19
    ##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
    ##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
    ##  1  2013     1    27     1419           1420        -1     1754           1550
    ##  2  2013    10     7     1350           1350         0     1736           1526
    ##  3  2013    10     7     1357           1359        -2     1858           1654
    ##  4  2013    10    16      657            700        -3     1258           1056
    ##  5  2013    11     1      658            700        -2     1329           1015
    ##  6  2013     3    18     1844           1847        -3       39           2219
    ##  7  2013     4    17     1635           1640        -5     2049           1845
    ##  8  2013     4    18      558            600        -2     1149            850
    ##  9  2013     4    18      655            700        -5     1213            950
    ## 10  2013     5    22     1827           1830        -3     2217           2010
    ## # … with 19 more rows, and 11 more variables: arr_delay <dbl>, carrier <chr>,
    ## #   flight <int>, tailnum <chr>, origin <chr>, dest <chr>, air_time <dbl>,
    ## #   distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>

1.6Were delayed by at least an hour, but made up over 30 minutes in
flight Departed between midnight and 6am (inclusive)

ˆ

``` r
filter(flights, dep_delay>=60, dep_delay - arr_delay>60)
```

    ## # A tibble: 30 x 19
    ##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
    ##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
    ##  1  2013     1     3     1950           1845        65     2228           2227
    ##  2  2013    11    12     2216           2015       121       51           2354
    ##  3  2013    12    27     1936           1830        66     2204           2159
    ##  4  2013     2    23     1226            900       206     1746           1540
    ##  5  2013     2    26     1000            900        60     1513           1540
    ##  6  2013     2    27     2006           1830        96     2238           2205
    ##  7  2013     3     1     1528            920       368     1738           1240
    ##  8  2013     3     1     1738           1545       113     1958           1910
    ##  9  2013     3     1     1827           1620       127     2106           2003
    ## 10  2013     3     1     1929           1645       164     2141           2005
    ## # … with 20 more rows, and 11 more variables: arr_delay <dbl>, carrier <chr>,
    ## #   flight <int>, tailnum <chr>, origin <chr>, dest <chr>, air_time <dbl>,
    ## #   distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>

Another useful dplyr filtering helper is between(). What does it do? Can
you use it to simplify the code needed to answer the previous
challenges?

``` r
filter(flights, between(dep_time, 00, 600))
```

    ## # A tibble: 9,344 x 19
    ##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
    ##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
    ##  1  2013     1     1      517            515         2      830            819
    ##  2  2013     1     1      533            529         4      850            830
    ##  3  2013     1     1      542            540         2      923            850
    ##  4  2013     1     1      544            545        -1     1004           1022
    ##  5  2013     1     1      554            600        -6      812            837
    ##  6  2013     1     1      554            558        -4      740            728
    ##  7  2013     1     1      555            600        -5      913            854
    ##  8  2013     1     1      557            600        -3      709            723
    ##  9  2013     1     1      557            600        -3      838            846
    ## 10  2013     1     1      558            600        -2      753            745
    ## # … with 9,334 more rows, and 11 more variables: arr_delay <dbl>,
    ## #   carrier <chr>, flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
    ## #   air_time <dbl>, distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>

How many flights have a missing dep\_time? What other variables are
missing? What might these rows represent?

``` r
filter(flights, is.na(dep_time))
```

    ## # A tibble: 8,255 x 19
    ##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
    ##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
    ##  1  2013     1     1       NA           1630        NA       NA           1815
    ##  2  2013     1     1       NA           1935        NA       NA           2240
    ##  3  2013     1     1       NA           1500        NA       NA           1825
    ##  4  2013     1     1       NA            600        NA       NA            901
    ##  5  2013     1     2       NA           1540        NA       NA           1747
    ##  6  2013     1     2       NA           1620        NA       NA           1746
    ##  7  2013     1     2       NA           1355        NA       NA           1459
    ##  8  2013     1     2       NA           1420        NA       NA           1644
    ##  9  2013     1     2       NA           1321        NA       NA           1536
    ## 10  2013     1     2       NA           1545        NA       NA           1910
    ## # … with 8,245 more rows, and 11 more variables: arr_delay <dbl>,
    ## #   carrier <chr>, flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
    ## #   air_time <dbl>, distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>

Why is NA ^ 0 not missing? Why is NA | TRUE not missing? Why is FALSE &
NA not missing? Can you figure out the general rule? (NA \* 0 is a
tricky counterexample\!)

``` r
flights
```

    ## # A tibble: 336,776 x 19
    ##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
    ##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
    ##  1  2013     1     1      517            515         2      830            819
    ##  2  2013     1     1      533            529         4      850            830
    ##  3  2013     1     1      542            540         2      923            850
    ##  4  2013     1     1      544            545        -1     1004           1022
    ##  5  2013     1     1      554            600        -6      812            837
    ##  6  2013     1     1      554            558        -4      740            728
    ##  7  2013     1     1      555            600        -5      913            854
    ##  8  2013     1     1      557            600        -3      709            723
    ##  9  2013     1     1      557            600        -3      838            846
    ## 10  2013     1     1      558            600        -2      753            745
    ## # … with 336,766 more rows, and 11 more variables: arr_delay <dbl>,
    ## #   carrier <chr>, flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
    ## #   air_time <dbl>, distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>

``` r
filter(flights, dest=="IAH")
```

    ## # A tibble: 7,198 x 19
    ##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
    ##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
    ##  1  2013     1     1      517            515         2      830            819
    ##  2  2013     1     1      533            529         4      850            830
    ##  3  2013     1     1      623            627        -4      933            932
    ##  4  2013     1     1      728            732        -4     1041           1038
    ##  5  2013     1     1      739            739         0     1104           1038
    ##  6  2013     1     1      908            908         0     1228           1219
    ##  7  2013     1     1     1028           1026         2     1350           1339
    ##  8  2013     1     1     1044           1045        -1     1352           1351
    ##  9  2013     1     1     1114            900       134     1447           1222
    ## 10  2013     1     1     1205           1200         5     1503           1505
    ## # … with 7,188 more rows, and 11 more variables: arr_delay <dbl>,
    ## #   carrier <chr>, flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
    ## #   air_time <dbl>, distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>

##### 5.3 Arrange rows with arrange()

``` r
flights
```

    ## # A tibble: 336,776 x 19
    ##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
    ##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
    ##  1  2013     1     1      517            515         2      830            819
    ##  2  2013     1     1      533            529         4      850            830
    ##  3  2013     1     1      542            540         2      923            850
    ##  4  2013     1     1      544            545        -1     1004           1022
    ##  5  2013     1     1      554            600        -6      812            837
    ##  6  2013     1     1      554            558        -4      740            728
    ##  7  2013     1     1      555            600        -5      913            854
    ##  8  2013     1     1      557            600        -3      709            723
    ##  9  2013     1     1      557            600        -3      838            846
    ## 10  2013     1     1      558            600        -2      753            745
    ## # … with 336,766 more rows, and 11 more variables: arr_delay <dbl>,
    ## #   carrier <chr>, flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
    ## #   air_time <dbl>, distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>

``` r
arrange(flights, dep_delay, month, day)
```

    ## # A tibble: 336,776 x 19
    ##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
    ##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
    ##  1  2013    12     7     2040           2123       -43       40           2352
    ##  2  2013     2     3     2022           2055       -33     2240           2338
    ##  3  2013    11    10     1408           1440       -32     1549           1559
    ##  4  2013     1    11     1900           1930       -30     2233           2243
    ##  5  2013     1    29     1703           1730       -27     1947           1957
    ##  6  2013     8     9      729            755       -26     1002            955
    ##  7  2013     3    30     2030           2055       -25     2213           2250
    ##  8  2013    10    23     1907           1932       -25     2143           2143
    ##  9  2013     3     2     1431           1455       -24     1601           1631
    ## 10  2013     5     5      934            958       -24     1225           1309
    ## # … with 336,766 more rows, and 11 more variables: arr_delay <dbl>,
    ## #   carrier <chr>, flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
    ## #   air_time <dbl>, distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>

Use desc() to re-order by a column in descending order

``` r
arrange(flights, desc(dep_delay, month, day))
```

    ## # A tibble: 336,776 x 19
    ##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
    ##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
    ##  1  2013     1     9      641            900      1301     1242           1530
    ##  2  2013     6    15     1432           1935      1137     1607           2120
    ##  3  2013     1    10     1121           1635      1126     1239           1810
    ##  4  2013     9    20     1139           1845      1014     1457           2210
    ##  5  2013     7    22      845           1600      1005     1044           1815
    ##  6  2013     4    10     1100           1900       960     1342           2211
    ##  7  2013     3    17     2321            810       911      135           1020
    ##  8  2013     6    27      959           1900       899     1236           2226
    ##  9  2013     7    22     2257            759       898      121           1026
    ## 10  2013    12     5      756           1700       896     1058           2020
    ## # … with 336,766 more rows, and 11 more variables: arr_delay <dbl>,
    ## #   carrier <chr>, flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
    ## #   air_time <dbl>, distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>

**Missing values are always sorted at the end**

Template for cleaning database————\>

newDataBase\<-filter(database, \!is.no(desired\_variable))

##### 5.3.1 Ezercises

1.How could you use arrange() to sort all missing values to the start?
(Hint: use is.na()).

2.Sort flights to find the most delayed flights. Find the flights that
left earliest.

3.Sort flights to find the fastest (highest speed) flights.

speed=distance/time (mph)

``` r
arrange(flights, air_time) #incorrect
```

    ## # A tibble: 336,776 x 19
    ##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
    ##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
    ##  1  2013     1    16     1355           1315        40     1442           1411
    ##  2  2013     4    13      537            527        10      622            628
    ##  3  2013    12     6      922            851        31     1021            954
    ##  4  2013     2     3     2153           2129        24     2247           2224
    ##  5  2013     2     5     1303           1315       -12     1342           1411
    ##  6  2013     2    12     2123           2130        -7     2211           2225
    ##  7  2013     3     2     1450           1500       -10     1547           1608
    ##  8  2013     3     8     2026           1935        51     2131           2056
    ##  9  2013     3    18     1456           1329        87     1533           1426
    ## 10  2013     3    19     2226           2145        41     2305           2246
    ## # … with 336,766 more rows, and 11 more variables: arr_delay <dbl>,
    ## #   carrier <chr>, flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
    ## #   air_time <dbl>, distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>

``` r
arrange(flights, desc(distance/air_time))
```

    ## # A tibble: 336,776 x 19
    ##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
    ##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
    ##  1  2013     5    25     1709           1700         9     1923           1937
    ##  2  2013     7     2     1558           1513        45     1745           1719
    ##  3  2013     5    13     2040           2025        15     2225           2226
    ##  4  2013     3    23     1914           1910         4     2045           2043
    ##  5  2013     1    12     1559           1600        -1     1849           1917
    ##  6  2013    11    17      650            655        -5     1059           1150
    ##  7  2013     2    21     2355           2358        -3      412            438
    ##  8  2013    11    17      759            800        -1     1212           1255
    ##  9  2013    11    16     2003           1925        38       17             36
    ## 10  2013    11    16     2349           2359       -10      402            440
    ## # … with 336,766 more rows, and 11 more variables: arr_delay <dbl>,
    ## #   carrier <chr>, flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
    ## #   air_time <dbl>, distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>

4.Which flights travelled the farthest? Which travelled the shortest?

``` r
arrange(flights, desc(distance))
```

    ## # A tibble: 336,776 x 19
    ##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
    ##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
    ##  1  2013     1     1      857            900        -3     1516           1530
    ##  2  2013     1     2      909            900         9     1525           1530
    ##  3  2013     1     3      914            900        14     1504           1530
    ##  4  2013     1     4      900            900         0     1516           1530
    ##  5  2013     1     5      858            900        -2     1519           1530
    ##  6  2013     1     6     1019            900        79     1558           1530
    ##  7  2013     1     7     1042            900       102     1620           1530
    ##  8  2013     1     8      901            900         1     1504           1530
    ##  9  2013     1     9      641            900      1301     1242           1530
    ## 10  2013     1    10      859            900        -1     1449           1530
    ## # … with 336,766 more rows, and 11 more variables: arr_delay <dbl>,
    ## #   carrier <chr>, flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
    ## #   air_time <dbl>, distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>

``` r
arrange(flights, distance)
```

    ## # A tibble: 336,776 x 19
    ##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
    ##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
    ##  1  2013     7    27       NA            106        NA       NA            245
    ##  2  2013     1     3     2127           2129        -2     2222           2224
    ##  3  2013     1     4     1240           1200        40     1333           1306
    ##  4  2013     1     4     1829           1615       134     1937           1721
    ##  5  2013     1     4     2128           2129        -1     2218           2224
    ##  6  2013     1     5     1155           1200        -5     1241           1306
    ##  7  2013     1     6     2125           2129        -4     2224           2224
    ##  8  2013     1     7     2124           2129        -5     2212           2224
    ##  9  2013     1     8     2127           2130        -3     2304           2225
    ## 10  2013     1     9     2126           2129        -3     2217           2224
    ## # … with 336,766 more rows, and 11 more variables: arr_delay <dbl>,
    ## #   carrier <chr>, flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
    ## #   air_time <dbl>, distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>

### 3.4 select columns with select()

``` r
select(flights, year, month, day)
```

    ## # A tibble: 336,776 x 3
    ##     year month   day
    ##    <int> <int> <int>
    ##  1  2013     1     1
    ##  2  2013     1     1
    ##  3  2013     1     1
    ##  4  2013     1     1
    ##  5  2013     1     1
    ##  6  2013     1     1
    ##  7  2013     1     1
    ##  8  2013     1     1
    ##  9  2013     1     1
    ## 10  2013     1     1
    ## # … with 336,766 more rows

``` r
select(flights, year:day)
```

    ## # A tibble: 336,776 x 3
    ##     year month   day
    ##    <int> <int> <int>
    ##  1  2013     1     1
    ##  2  2013     1     1
    ##  3  2013     1     1
    ##  4  2013     1     1
    ##  5  2013     1     1
    ##  6  2013     1     1
    ##  7  2013     1     1
    ##  8  2013     1     1
    ##  9  2013     1     1
    ## 10  2013     1     1
    ## # … with 336,766 more rows

``` r
select(flights, -(year:day))
```

    ## # A tibble: 336,776 x 16
    ##    dep_time sched_dep_time dep_delay arr_time sched_arr_time arr_delay carrier
    ##       <int>          <int>     <dbl>    <int>          <int>     <dbl> <chr>  
    ##  1      517            515         2      830            819        11 UA     
    ##  2      533            529         4      850            830        20 UA     
    ##  3      542            540         2      923            850        33 AA     
    ##  4      544            545        -1     1004           1022       -18 B6     
    ##  5      554            600        -6      812            837       -25 DL     
    ##  6      554            558        -4      740            728        12 UA     
    ##  7      555            600        -5      913            854        19 B6     
    ##  8      557            600        -3      709            723       -14 EV     
    ##  9      557            600        -3      838            846        -8 B6     
    ## 10      558            600        -2      753            745         8 AA     
    ## # … with 336,766 more rows, and 9 more variables: flight <int>, tailnum <chr>,
    ## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
    ## #   minute <dbl>, time_hour <dttm>

*-* gives the complement or everything but that variable or column.

There are a numbe of functions you can use within select ():

starts\_with() ends\_with() contains()

``` r
select(flights, starts_with("dep"))
```

    ## # A tibble: 336,776 x 2
    ##    dep_time dep_delay
    ##       <int>     <dbl>
    ##  1      517         2
    ##  2      533         4
    ##  3      542         2
    ##  4      544        -1
    ##  5      554        -6
    ##  6      554        -4
    ##  7      555        -5
    ##  8      557        -3
    ##  9      557        -3
    ## 10      558        -2
    ## # … with 336,766 more rows

``` r
select(flights, ends_with("delay"))
```

    ## # A tibble: 336,776 x 2
    ##    dep_delay arr_delay
    ##        <dbl>     <dbl>
    ##  1         2        11
    ##  2         4        20
    ##  3         2        33
    ##  4        -1       -18
    ##  5        -6       -25
    ##  6        -4        12
    ##  7        -5        19
    ##  8        -3       -14
    ##  9        -3        -8
    ## 10        -2         8
    ## # … with 336,766 more rows

``` r
select(flights, contains("day"))
```

    ## # A tibble: 336,776 x 1
    ##      day
    ##    <int>
    ##  1     1
    ##  2     1
    ##  3     1
    ##  4     1
    ##  5     1
    ##  6     1
    ##  7     1
    ##  8     1
    ##  9     1
    ## 10     1
    ## # … with 336,766 more rows

``` r
rename(flights, tail_num=tailnum)
```

    ## # A tibble: 336,776 x 19
    ##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
    ##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
    ##  1  2013     1     1      517            515         2      830            819
    ##  2  2013     1     1      533            529         4      850            830
    ##  3  2013     1     1      542            540         2      923            850
    ##  4  2013     1     1      544            545        -1     1004           1022
    ##  5  2013     1     1      554            600        -6      812            837
    ##  6  2013     1     1      554            558        -4      740            728
    ##  7  2013     1     1      555            600        -5      913            854
    ##  8  2013     1     1      557            600        -3      709            723
    ##  9  2013     1     1      557            600        -3      838            846
    ## 10  2013     1     1      558            600        -2      753            745
    ## # … with 336,766 more rows, and 11 more variables: arr_delay <dbl>,
    ## #   carrier <chr>, flight <int>, tail_num <chr>, origin <chr>, dest <chr>,
    ## #   air_time <dbl>, distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>

``` r
mydatabase= flights

mydatabase<-rename(flights, tail_num=tailnum)
```

Moving Variables Forward in columns

``` r
select(flights, dep_delay, arr_delay, everything())
```

    ## # A tibble: 336,776 x 19
    ##    dep_delay arr_delay  year month   day dep_time sched_dep_time arr_time
    ##        <dbl>     <dbl> <int> <int> <int>    <int>          <int>    <int>
    ##  1         2        11  2013     1     1      517            515      830
    ##  2         4        20  2013     1     1      533            529      850
    ##  3         2        33  2013     1     1      542            540      923
    ##  4        -1       -18  2013     1     1      544            545     1004
    ##  5        -6       -25  2013     1     1      554            600      812
    ##  6        -4        12  2013     1     1      554            558      740
    ##  7        -5        19  2013     1     1      555            600      913
    ##  8        -3       -14  2013     1     1      557            600      709
    ##  9        -3        -8  2013     1     1      557            600      838
    ## 10        -2         8  2013     1     1      558            600      753
    ## # … with 336,766 more rows, and 11 more variables: sched_arr_time <int>,
    ## #   carrier <chr>, flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
    ## #   air_time <dbl>, distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>

1.  Brainstorm as many ways as possible to select dep\_time, dep\_delay,
    arr\_time, and arr\_delay from flights.

<!-- end list -->

``` r
select(flights, starts_with('dep'))
```

    ## # A tibble: 336,776 x 2
    ##    dep_time dep_delay
    ##       <int>     <dbl>
    ##  1      517         2
    ##  2      533         4
    ##  3      542         2
    ##  4      544        -1
    ##  5      554        -6
    ##  6      554        -4
    ##  7      555        -5
    ##  8      557        -3
    ##  9      557        -3
    ## 10      558        -2
    ## # … with 336,766 more rows

2.  What happens if you include the name of a variable multiple times in
    a select() call?

<!-- end list -->

``` r
select(flights, air_time, distance, air_time)
```

    ## # A tibble: 336,776 x 2
    ##    air_time distance
    ##       <dbl>    <dbl>
    ##  1      227     1400
    ##  2      227     1416
    ##  3      160     1089
    ##  4      183     1576
    ##  5      116      762
    ##  6      150      719
    ##  7      158     1065
    ##  8       53      229
    ##  9      140      944
    ## 10      138      733
    ## # … with 336,766 more rows

``` r
flights_narrow <- select(flights, year:day, ends_with("delay"), distance, air_time)

flights_narrow
```

    ## # A tibble: 336,776 x 7
    ##     year month   day dep_delay arr_delay distance air_time
    ##    <int> <int> <int>     <dbl>     <dbl>    <dbl>    <dbl>
    ##  1  2013     1     1         2        11     1400      227
    ##  2  2013     1     1         4        20     1416      227
    ##  3  2013     1     1         2        33     1089      160
    ##  4  2013     1     1        -1       -18     1576      183
    ##  5  2013     1     1        -6       -25      762      116
    ##  6  2013     1     1        -4        12      719      150
    ##  7  2013     1     1        -5        19     1065      158
    ##  8  2013     1     1        -3       -14      229       53
    ##  9  2013     1     1        -3        -8      944      140
    ## 10  2013     1     1        -2         8      733      138
    ## # … with 336,766 more rows

``` r
mutate(flights_narrow, gain=dep_delay-arr_delay, speed=distance/air_time * 60)
```

    ## # A tibble: 336,776 x 9
    ##     year month   day dep_delay arr_delay distance air_time  gain speed
    ##    <int> <int> <int>     <dbl>     <dbl>    <dbl>    <dbl> <dbl> <dbl>
    ##  1  2013     1     1         2        11     1400      227    -9  370.
    ##  2  2013     1     1         4        20     1416      227   -16  374.
    ##  3  2013     1     1         2        33     1089      160   -31  408.
    ##  4  2013     1     1        -1       -18     1576      183    17  517.
    ##  5  2013     1     1        -6       -25      762      116    19  394.
    ##  6  2013     1     1        -4        12      719      150   -16  288.
    ##  7  2013     1     1        -5        19     1065      158   -24  404.
    ##  8  2013     1     1        -3       -14      229       53    11  259.
    ##  9  2013     1     1        -3        -8      944      140     5  405.
    ## 10  2013     1     1        -2         8      733      138   -10  319.
    ## # … with 336,766 more rows

You can refer to columns you just created

``` r
mutate(flights, gain=dep_delay-arr_delay, speed=distance/air_time *60, gain_per_hour=gain/air_time)
```

    ## # A tibble: 336,776 x 22
    ##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
    ##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
    ##  1  2013     1     1      517            515         2      830            819
    ##  2  2013     1     1      533            529         4      850            830
    ##  3  2013     1     1      542            540         2      923            850
    ##  4  2013     1     1      544            545        -1     1004           1022
    ##  5  2013     1     1      554            600        -6      812            837
    ##  6  2013     1     1      554            558        -4      740            728
    ##  7  2013     1     1      555            600        -5      913            854
    ##  8  2013     1     1      557            600        -3      709            723
    ##  9  2013     1     1      557            600        -3      838            846
    ## 10  2013     1     1      558            600        -2      753            745
    ## # … with 336,766 more rows, and 14 more variables: arr_delay <dbl>,
    ## #   carrier <chr>, flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
    ## #   air_time <dbl>, distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>,
    ## #   gain <dbl>, speed <dbl>, gain_per_hour <dbl>

``` r
summarize(flights, delay=mean(dep_delay, na.rn=TRUE))
```

    ## # A tibble: 1 x 1
    ##   delay
    ##   <dbl>
    ## 1    NA

``` r
by_day<-group_by(flights, year, month, day)
```

``` r
by_carrier<-group_by(flights, carrier)

summarize(by_carrier, delay=mean(dep_delay, na.rm = TRUE))
```

    ## `summarise()` ungrouping output (override with `.groups` argument)

    ## # A tibble: 16 x 2
    ##    carrier delay
    ##    <chr>   <dbl>
    ##  1 9E      16.7 
    ##  2 AA       8.59
    ##  3 AS       5.80
    ##  4 B6      13.0 
    ##  5 DL       9.26
    ##  6 EV      20.0 
    ##  7 F9      20.2 
    ##  8 FL      18.7 
    ##  9 HA       4.90
    ## 10 MQ      10.6 
    ## 11 OO      12.6 
    ## 12 UA      12.1 
    ## 13 US       3.78
    ## 14 VX      12.9 
    ## 15 WN      17.7 
    ## 16 YV      19.0

##### 6.1 Combining multiple operations with the pipe

Imagine that we want to explore the reltionship between distance and
average delay for each location. Using what you know about dplyr, you
might write code like this.

Group flights by destination

``` r
by_dest<-group_by(flights,dest)
```

Summarize the group, using average distance, average delay, and count:

``` r
delays<-summarize(by_dest,
                  count=n(),
                  dist=mean(distance, na.rm=TRUE),
                  delay=mean(arr_delay, na.rm=TRUE)
)
```

    ## `summarise()` ungrouping output (override with `.groups` argument)

``` r
delays
```

    ## # A tibble: 105 x 4
    ##    dest  count  dist delay
    ##    <chr> <int> <dbl> <dbl>
    ##  1 ABQ     254 1826   4.38
    ##  2 ACK     265  199   4.85
    ##  3 ALB     439  143  14.4 
    ##  4 ANC       8 3370  -2.5 
    ##  5 ATL   17215  757. 11.3 
    ##  6 AUS    2439 1514.  6.02
    ##  7 AVL     275  584.  8.00
    ##  8 BDL     443  116   7.05
    ##  9 BGR     375  378   8.03
    ## 10 BHM     297  866. 16.9 
    ## # … with 95 more rows

Visualize your data:

Make a scatter plot of distance vs. delay, with count mapped to the size
aesthetic.

``` r
ggplot(data=delays)+ geom_point(mapping=aes(x=dist, y=delay, size=count))
```

    ## Warning: Removed 1 rows containing missing values (geom_point).

![](Data-Wrangling_files/figure-gfm/unnamed-chunk-49-1.png)<!-- -->
Remove noisy points

``` r
delays<-filter(delays, count>20, dest!="HNL")
```

``` r
ggplot(data=delays)+ geom_point(mapping=aes(x=dist, y=delay, size=count))
```

![](Data-Wrangling_files/figure-gfm/unnamed-chunk-51-1.png)<!-- -->

Clean up plot

``` r
ggplot(data=delays, mapping=aes(x=dist, y=delay))+geom_point(aes(size=count), alpha=1/3)+geom_smooth(se=FALSE)
```

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

![](Data-Wrangling_files/figure-gfm/unnamed-chunk-52-1.png)<!-- -->

``` r
by_dest<-flights %>% 
  group_by(dest) %>%
  summarize(
    count=n(),
    dist=mean(distance, na.rm=TRUE),
    delay=mean(arr_delay, na.rm=TRUE)
  )%>%
    filter(count>20, dest!="HNL")
```

    ## `summarise()` ungrouping output (override with `.groups` argument)

``` r
ggplot(data=delays, mapping=aes(x=dist, y=delay))+geom_point(aes(size=count), alpha=1/3)+geom_smooth(se=FALSE)
```

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

![](Data-Wrangling_files/figure-gfm/pipe-1.png)<!-- -->

##### 5.6.2 Missing Values

You may have wondered about the na.rm argument we used above. What
happens if we don’t set it?

``` r
flights %>%
  group_by(year, month, day)%>%
  summarize(delay=mean(dep_delay))
```

    ## `summarise()` regrouping output by 'year', 'month' (override with `.groups` argument)

    ## # A tibble: 365 x 4
    ## # Groups:   year, month [12]
    ##     year month   day delay
    ##    <int> <int> <int> <dbl>
    ##  1  2013     1     1    NA
    ##  2  2013     1     2    NA
    ##  3  2013     1     3    NA
    ##  4  2013     1     4    NA
    ##  5  2013     1     5    NA
    ##  6  2013     1     6    NA
    ##  7  2013     1     7    NA
    ##  8  2013     1     8    NA
    ##  9  2013     1     9    NA
    ## 10  2013     1    10    NA
    ## # … with 355 more rows

is.na(variable) asks a question- is the variable NA?

In this case, where missing values represent cancelled flights, we could
also tackle the

``` r
flights
```

    ## # A tibble: 336,776 x 19
    ##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
    ##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
    ##  1  2013     1     1      517            515         2      830            819
    ##  2  2013     1     1      533            529         4      850            830
    ##  3  2013     1     1      542            540         2      923            850
    ##  4  2013     1     1      544            545        -1     1004           1022
    ##  5  2013     1     1      554            600        -6      812            837
    ##  6  2013     1     1      554            558        -4      740            728
    ##  7  2013     1     1      555            600        -5      913            854
    ##  8  2013     1     1      557            600        -3      709            723
    ##  9  2013     1     1      557            600        -3      838            846
    ## 10  2013     1     1      558            600        -2      753            745
    ## # … with 336,766 more rows, and 11 more variables: arr_delay <dbl>,
    ## #   carrier <chr>, flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
    ## #   air_time <dbl>, distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>

``` r
not_cancelled<- flights %>%
  filter(is.na(arr_delay),  is.na(dep_delay))

flights
```

    ## # A tibble: 336,776 x 19
    ##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
    ##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
    ##  1  2013     1     1      517            515         2      830            819
    ##  2  2013     1     1      533            529         4      850            830
    ##  3  2013     1     1      542            540         2      923            850
    ##  4  2013     1     1      544            545        -1     1004           1022
    ##  5  2013     1     1      554            600        -6      812            837
    ##  6  2013     1     1      554            558        -4      740            728
    ##  7  2013     1     1      555            600        -5      913            854
    ##  8  2013     1     1      557            600        -3      709            723
    ##  9  2013     1     1      557            600        -3      838            846
    ## 10  2013     1     1      558            600        -2      753            745
    ## # … with 336,766 more rows, and 11 more variables: arr_delay <dbl>,
    ## #   carrier <chr>, flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
    ## #   air_time <dbl>, distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>

``` r
not_cancelled
```

    ## # A tibble: 8,255 x 19
    ##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
    ##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
    ##  1  2013     1     1       NA           1630        NA       NA           1815
    ##  2  2013     1     1       NA           1935        NA       NA           2240
    ##  3  2013     1     1       NA           1500        NA       NA           1825
    ##  4  2013     1     1       NA            600        NA       NA            901
    ##  5  2013     1     2       NA           1540        NA       NA           1747
    ##  6  2013     1     2       NA           1620        NA       NA           1746
    ##  7  2013     1     2       NA           1355        NA       NA           1459
    ##  8  2013     1     2       NA           1420        NA       NA           1644
    ##  9  2013     1     2       NA           1321        NA       NA           1536
    ## 10  2013     1     2       NA           1545        NA       NA           1910
    ## # … with 8,245 more rows, and 11 more variables: arr_delay <dbl>,
    ## #   carrier <chr>, flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
    ## #   air_time <dbl>, distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>

Now, there are some planes that have an average delay of 5 hours (300
minutes)

``` r
delays<-not_cancelled %>% 
  group_by(tailnum) %>% 
  summarize(delay=mean(arr_delay))
```

    ## `summarise()` ungrouping output (override with `.groups` argument)

``` r
delays
```

    ## # A tibble: 1,450 x 2
    ##    tailnum delay
    ##    <chr>   <dbl>
    ##  1 N0EGMQ     NA
    ##  2 N10156     NA
    ##  3 N10575     NA
    ##  4 N11106     NA
    ##  5 N11107     NA
    ##  6 N11109     NA
    ##  7 N11113     NA
    ##  8 N11119     NA
    ##  9 N11121     NA
    ## 10 N11127     NA
    ## # … with 1,440 more rows

``` r
arrange(delays, desc(delay))
```

    ## # A tibble: 1,450 x 2
    ##    tailnum delay
    ##    <chr>   <dbl>
    ##  1 N0EGMQ     NA
    ##  2 N10156     NA
    ##  3 N10575     NA
    ##  4 N11106     NA
    ##  5 N11107     NA
    ##  6 N11109     NA
    ##  7 N11113     NA
    ##  8 N11119     NA
    ##  9 N11121     NA
    ## 10 N11127     NA
    ## # … with 1,440 more rows

There are some planes that have an average delay of 300+ minutes

``` r
delays<-not_cancelled %>%
  group_by(tailnum) %>%
  summarize(delay=mean(arr_delay), 
            count=n()
  )
```

    ## `summarise()` ungrouping output (override with `.groups` argument)

``` r
arrange(delays, desc(delay))
```

    ## # A tibble: 1,450 x 3
    ##    tailnum delay count
    ##    <chr>   <dbl> <int>
    ##  1 N0EGMQ     NA    17
    ##  2 N10156     NA     7
    ##  3 N10575     NA    17
    ##  4 N11106     NA     3
    ##  5 N11107     NA     8
    ##  6 N11109     NA     5
    ##  7 N11113     NA    12
    ##  8 N11119     NA    10
    ##  9 N11121     NA     6
    ## 10 N11127     NA     5
    ## # … with 1,440 more rows

``` r
geom_point(mapping=aes(x=count, y=delay), alpha=1/3)
```

    ## mapping: x = ~count, y = ~delay 
    ## geom_point: na.rm = FALSE
    ## stat_identity: na.rm = FALSE
    ## position_identity

``` r
delays%>%
  filter(count>25)%>%
  ggplot()+
  geom_point(mapping=aes(x=count, y=delay), alpha=.2)
```

    ## Warning: Removed 6 rows containing missing values (geom_point).

![](Data-Wrangling_files/figure-gfm/unnamed-chunk-58-1.png)<!-- -->
