Data transformation
===================

Introduction
------------

Visualisation is an important tool for insight generation, but it is
rare that you get the data in exactly the right form you need. Often
you’ll need to create some new variables or summaries, or maybe you just
want to rename the variables or reorder the observations in order to
make the data a little easier to work with. You’ll learn how to do all
that (and more!) in this chapter, which will teach you how to transform
your data using the dplyr package and a new dataset on flights departing
New York City in 2013.

### Prerequisites

In this chapter we’re going to focus on how to use the dplyr package,
another core member of the tidyverse. We’ll illustrate the key ideas
using data from the nycflights13 package, and use ggplot2 to help us
understand the data.

    library(nycflights13)
    library(tidyverse)

Take careful note of the conflicts message that’s printed when you load
the tidyverse. It tells you that dplyr overwrites some functions in base
R. If you want to use the base version of these functions after loading
dplyr, you’ll need to use their full names: `stats::filter()` and
`stats::lag()`.

### nycflights13

To explore the basic data manipulation verbs of dplyr, we’ll use
`nycflights13::flights`. This data frame contains all 336,776 flights
that departed from New York City in 2013. The data comes from the US
[Bureau of Transportation
Statistics](http://www.transtats.bts.gov/DatabaseInfo.asp?DB_ID=120&Link=0),
and is documented in `?flights`.

    flights

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
