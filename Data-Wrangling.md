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

    #using to learn the verb filter.

    jun6 <- filter(flights, month == 6, day ==6)
    jun_aug <- filter(flights, month ==6 | month ==8)
    hourlongdelays <- filter(flights, arr_delay > 60 & air_time < 1000 )

    #Exercise 5.2.4

    #1 Arrival delay of two or more hrs.
    filter(flights, arr_delay >= 120)

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

    #2 Flew to Houston
    filter(flights, dest == "IAH" | dest == "HOU")

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

    #6 Delayed by at least an hour, but made up over 30 minutes in the air
    filter(flights, arr_delay >= 60 & dep_delay - arr_delay > 30)

    ## # A tibble: 1,084 x 19
    ##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
    ##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
    ##  1  2013     1     1     2205           1720       285       46           2040
    ##  2  2013     1     1     2326           2130       116      131             18
    ##  3  2013     1     3     1503           1221       162     1803           1555
    ##  4  2013     1     3     1839           1700        99     2056           1950
    ##  5  2013     1     3     1941           1759       102     2246           2139
    ##  6  2013     1     3     2257           2000       177       45           2224
    ##  7  2013     1     4     1917           1700       137     2135           1950
    ##  8  2013     1     4     2010           1745       145     2257           2120
    ##  9  2013     1     4     2058           1730       208        2           2110
    ## 10  2013     1     4     2100           1920       100     2224           2121
    ## # … with 1,074 more rows, and 11 more variables: arr_delay <dbl>,
    ## #   carrier <chr>, flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
    ## #   air_time <dbl>, distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>
