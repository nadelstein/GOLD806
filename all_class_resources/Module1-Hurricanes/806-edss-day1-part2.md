Day 1.2): Hurricanes Data
================

## Exploratory Data Analysis with `dplyr`

We want you to get your hands dirty in R as quick as possible, and
perhaps the best way to do this is by jumping right away into the pool.
You’ll have your first contact with tabular data (data arranged in rows
and columns) which is the most common format in which data is handled
for data analysis.

To keep things fiarly simple, we use a data set that comes in one the
most popular R packages for manipulation of tables: `"dplyr"`. The main
reason to start in this mode, is to avoid having to worry about data
importing issues, which we cover later in the course. The other reason
is to have data that is already clean and ready to be analyzed. You will
also have time to learn tools and skills for cleaning data sets in the
next weeks.

### Manipulating tables with `"dplyr"` framework

You will start learning a couple of approaches to manipulate tables and
create basic statistical graphics. To manipulate tables, we are going to
use the functionality of the package `"dplyr"`. This package allows you
to work with tabular data *in a syntactic way*.

Later in the course you will also have the opportunity to learn more
*low-level* manipulation tasks.

We are assuming that you already installed the package `"dplyr"`. If
that’s not the case then **run on the console** the command below (do
NOT include this command in any `Rmd` file or `jupyter` notebook):

``` r
# don't include this command in any Rmd file
# don't worry too much if you get a warning message
install.packages("dplyr")
```

Remember that you only need to install a package once\! After a package
has been installed in your machine, there is no need to call
`install.packages()` again on the same package. What you should always
invoke, in order to use the functions in a package, is the `library()`
function:

``` r
# (you should include this command in your Rmd file)
library(dplyr)
```

**About loading packages:** Another rule to keep in mind is to always
load any required packages at the very top of your script files (`.R` or
`.Rmd` or `.Rnw` files). Avoid calling the `library()` function in the
middle of a script. Instead, load all the packages before anything else.

## Atlantic Hurricane Data

The package `"dplyr"` contains a dataset called `storms` which is a
subset of the NOAA Atlantic hurricane database best track data. This
database is one of several data sets available in the National Hurricane
Center (NHC) Data Archive, which is part of the National Oceanic and
Atmospheric Administration (NOAA).

<a href="http://www.nhc.noaa.gov/data/#hurdat" target="_blank">http://www.nhc.noaa.gov/data/\#hurdat</a>

The data `storms` includes the positions and attributes of 198 tropical
storms, measured every six hours during the lifetime of a storm. When
you type the name of the data object, you whould get something like
this:

``` r
storms
```

    ## # A tibble: 10,010 x 13
    ##    name   year month   day  hour   lat  long status category  wind pressure
    ##    <chr> <dbl> <dbl> <int> <dbl> <dbl> <dbl> <chr>  <ord>    <int>    <int>
    ##  1 Amy    1975     6    27     0  27.5 -79   tropi… -1          25     1013
    ##  2 Amy    1975     6    27     6  28.5 -79   tropi… -1          25     1013
    ##  3 Amy    1975     6    27    12  29.5 -79   tropi… -1          25     1013
    ##  4 Amy    1975     6    27    18  30.5 -79   tropi… -1          25     1013
    ##  5 Amy    1975     6    28     0  31.5 -78.8 tropi… -1          25     1012
    ##  6 Amy    1975     6    28     6  32.4 -78.7 tropi… -1          25     1012
    ##  7 Amy    1975     6    28    12  33.3 -78   tropi… -1          25     1011
    ##  8 Amy    1975     6    28    18  34   -77   tropi… -1          30     1006
    ##  9 Amy    1975     6    29     0  34.4 -75.8 tropi… 0           35     1004
    ## 10 Amy    1975     6    29     6  34   -74.8 tropi… 0           40     1002
    ## # … with 10,000 more rows, and 2 more variables: ts_diameter <dbl>,
    ## #   hu_diameter <dbl>

You can find some technical description of `storms` by taking a peek at
its manual (or help) documentation: `?storms`.

  - `storms` is a **tibble** object, which is one of the data objects in
    R that handles data in tabular format.

  - tibbles are not a native R object—they come from the homonym package
    `"tibble"`—instead they are a modern version of data frames

The way tibbles are *printed* is very interesting.

  - the number of rows that are displayed is limited to 10;

  - depending on the width of the printing space, you will only see a
    few columns shown to fit such width.

  - underneath the name of each column there is a three letter
    abbreviation inside angle brackets

  - this abbreviation indicates the *data type* used by R to store the
    values. For
    
      - `<chr>` stands for *character* data
      - `<dbl>` means double (i.e. real numbers or numbers with decimal
        digits)
      - `<int>` means integer (numbers with no decimal digits)
      - `<ord>` indicates an *ordinal* `factor` which is how R handles
        categorical data

### Your Turn

Take a look at the manual (or *help*) documentation of `storms` (see
command below). Find the description of the variables in data `storms`

    ?storms

## Exploratory Data Analysis

Recall the diagram of the Data Analysis Cycle:

<img src="../images/eda/eda-dac.svg" title="Exploratory Data Analysis in DAC" alt="Exploratory Data Analysis in DAC" width="55%" />

Exploring data is one of those tasks that you will use in both the Data
Preparation stage and the Core Analysis stage. EDA has a main purpose:
**get to know your data.** EDA is very similar to when you go to the
doctor and they do an initial exploration (measure your height, your
weight, temperature, blood pressure; listen to your heart and lungs;
look at your eyes, throat, ears; ask you questions about your eating
habits, physical activity habits, etc).

To keep things relatively simple, we won’t perform a full exploration of
every single variable (i.e. column) in the data. However, we encourage
you to play with the functions to go beyond what we cover in this
module. In real life, you will have to do such exploration.

### Basic Inspection of `year`

When you type `storms`, R displays the first 10 rows, which belong to
storm Amy in 1975. In other words, we know that the data contains at
least one storm from 1975. We also know, from the manual documentation
of `storms`, that there are supposed to be 198 storms. But we don’t know
for what years. So in a more or less arbitrary way, let’s begin
inspecting `storms` by focusing on column `year`. Our first question is:

**What years have the data been collected for?**

There are several ways in R to manipulate a column from a tabular
object. Using `"dplyr"`, there are two basic kinds of functions to
extract variables: `pull()` and `select()`.

<img src="../images/eda/dplyr-extract-column.svg" title="Extracting a column with dplyr functions &quot;pull&quot; and &quot;select&quot;" alt="Extracting a column with dplyr functions &quot;pull&quot; and &quot;select&quot;" width="65%" />

Let’s do a sanity check of years. We can use the function `pull()` that
*pulls* or extracts an entire column. Because there are 10010 elements
in `years`, let’s also use `unique()` to find out the set of year values
in the data. First we pull the year, and then we identify unique
occurrences:

``` r
unique(pull(storms, year))
```

    ##  [1] 1975 1976 1977 1978 1979 1980 1981 1982 1983 1984 1985 1986 1987 1988 1989
    ## [16] 1990 1991 1992 1993 1994 1995 1996 1997 1998 1999 2000 2001 2002 2003 2004
    ## [31] 2005 2006 2007 2008 2009 2010 2011 2012 2013 2014 2015

The same can be accomplished with `select()`. The difference with
`pull()` is in the way the output is handled by `select()`, which
returns output in a table format:

``` r
unique(select(storms, year))
```

    ## # A tibble: 41 x 1
    ##     year
    ##    <dbl>
    ##  1  1975
    ##  2  1976
    ##  3  1977
    ##  4  1978
    ##  5  1979
    ##  6  1980
    ##  7  1981
    ##  8  1982
    ##  9  1983
    ## 10  1984
    ## # … with 31 more rows

Based on the previous answers, we can see that `storms` has records
during a 41-year period since 1975 to 2015.

### Your Turn

  - Use `pull()`, `select()`, and `unique()` to inspect the values in
    column `month`

  - Try also to use `sort()` in order to arrange the unique values of
    `month`

  - Does the unique month values make sense? Are there months for which
    there seem to be no recorded storms?

  - Repeat the same inspection steps with column `day`

  - Use `"dplyr"` functions/commands to display the different (unique)
    types of storm `status`.

  - Use `"dplyr"` functions/commands to display the different types of  
    `category` values.

## Basic inspection of storms in 1975

Let’s focus on those storms recorded in 1975. How do we select them?
Computationally, this operation incolves a logical condition: `year
== 1975`. This condition means that, from all the available year values,
we get those that match 1975. This is done via `"dplyr"` function
`filter()`

<img src="../images/eda/dplyr-filter.svg" title="Extracting a row with dplyr function &quot;filter&quot;" alt="Extracting a row with dplyr function &quot;filter&quot;" width="40%" />

First, let’s create a subset `storms75` by *filtering* those rows with
`year` equal to 1975:

``` r
storms75 <- filter(storms, year == 1975)
storms75
```

    ## # A tibble: 86 x 13
    ##    name   year month   day  hour   lat  long status category  wind pressure
    ##    <chr> <dbl> <dbl> <int> <dbl> <dbl> <dbl> <chr>  <ord>    <int>    <int>
    ##  1 Amy    1975     6    27     0  27.5 -79   tropi… -1          25     1013
    ##  2 Amy    1975     6    27     6  28.5 -79   tropi… -1          25     1013
    ##  3 Amy    1975     6    27    12  29.5 -79   tropi… -1          25     1013
    ##  4 Amy    1975     6    27    18  30.5 -79   tropi… -1          25     1013
    ##  5 Amy    1975     6    28     0  31.5 -78.8 tropi… -1          25     1012
    ##  6 Amy    1975     6    28     6  32.4 -78.7 tropi… -1          25     1012
    ##  7 Amy    1975     6    28    12  33.3 -78   tropi… -1          25     1011
    ##  8 Amy    1975     6    28    18  34   -77   tropi… -1          30     1006
    ##  9 Amy    1975     6    29     0  34.4 -75.8 tropi… 0           35     1004
    ## 10 Amy    1975     6    29     6  34   -74.8 tropi… 0           40     1002
    ## # … with 76 more rows, and 2 more variables: ts_diameter <dbl>,
    ## #   hu_diameter <dbl>

Once we have the set of storms that occurred in 1975, one possible
question to ask is what `unique()` storms happened in that year:

``` r
unique(pull(storms75, name))
```

    ## [1] "Amy"      "Caroline" "Doris"

From the returned output, there are only three storms recorded in 1975.

A similar result can be obtained with `distinct()`, the difference being
the way in which the output is returned, in this case under the format
of a tibble:

``` r
distinct(storms75, name)
```

    ## # A tibble: 3 x 1
    ##   name    
    ##   <chr>   
    ## 1 Amy     
    ## 2 Caroline
    ## 3 Doris

Now that we know there are three storms for 1975, it would be nice to
count the number of rows or records for each of them. `"dplyr"` allows
us to do this with `count()`, passing the name of the table, and then
the name of the column for which we want to get the counts or
frequencies:

``` r
count(storms75, name)
```

    ## # A tibble: 3 x 2
    ##   name         n
    ##   <chr>    <int>
    ## 1 Amy         30
    ## 2 Caroline    33
    ## 3 Doris       23

### Your Turn

  - Repeat the previous exploratory steps but now with storms from year
    1980.

  - Try to find out how to specify a logical condition to filter various
    years: for example, storms from years 1975, 1976, and 1977.

  - Try to find out how to specify a logical condition to filter storms
    from year 1975 with `wind` values less than 100.

  - Use `"dplyr"` functions/commands to create a table (e.g. tibble)
    `storm_names_1980s` containing the name and year of storms recorded
    during the 1980s (i.e. from 1980 to 1989).

## Group-by Operations

Another common task when exploring data has to do with computations
applied on certain groups or categories of data. `"dplyr"` provides the
function `group_by()` which takes a data table, and we specify the
column(s) on which rows will be grouped by:

<img src="../images/eda/dplyr-group-by.svg" title="Group-by operations" alt="Group-by operations" width="80%" />

For example, we may be interested in calculating the average `wind`
speed and average `pressure` of each storm in 1975. First we need to
group by `name`, and then we use `summarise()` to indicate that we want
to get the `mean()` of `wind` and `pressure`, like this:

``` r
summarise(
  group_by(storms75, name),
  avg_wind = mean(wind),
  avg_pressure = mean(pressure)
)
```

    ## # A tibble: 3 x 3
    ##   name     avg_wind avg_pressure
    ##   <chr>       <dbl>        <dbl>
    ## 1 Amy          46.5         995.
    ## 2 Caroline     38.9        1002.
    ## 3 Doris        73.7         983.

Sometimes, you’ll find convenient to assign the output into its own
table:

``` r
avg_wind_pressure_75 <- summarise(
  group_by(storms75, name),
  avg_wind = mean(wind),
  avg_pressure = mean(pressure)
)
```

### Your Turn

  - Use `"dplyr"` functions/commands to create a table (e.g. tibble)
    `max_wind_pressure_75` containing columns: 1)`name` of storm, 2)
    `max_wind` maximum wind speed, and 3) `max_pressure` maximum
    pressure

  - Use `"dplyr"` functions/commands to create a table (e.g. tibble)
    `wind_stats_75` containing columns: 1)`name` of storm, 2) `min_wind`
    minimum wind speed, 3) `avg_wind` mean wind speed,

<!-- end list -->

4)  `med_wind` median wind speed, and 5) `max_wind` maximum wind speed.

## Arrange operations

The table of summary means `avg_wind_pressure_75` is ordered
alphabetically by `name`. But perhaps you may want to organize its
contents by `avg_wind` or by `avg_pressure`. Let’s see how to do this in
the next subsection.

Besides `group_by()` operations, another common type of manipulation is
the arragement of rows based on the values of one or more columns. In
`"dplyr"`, this can easily be achieved with the function `arrange()`.
The way this function works is passing the name of the table, and then
specifying one or more columns to order rows based on such values.

<img src="../images/eda/dplyr-arrange.svg" title="Arranging rows" alt="Arranging rows" width="85%" />

Say you want to arrange the contents of the average summary table, by
taking into account the columnd `avg_wind`:

``` r
arrange(avg_wind_pressure_75, avg_wind)
```

    ## # A tibble: 3 x 3
    ##   name     avg_wind avg_pressure
    ##   <chr>       <dbl>        <dbl>
    ## 1 Caroline     38.9        1002.
    ## 2 Amy          46.5         995.
    ## 3 Doris        73.7         983.

Likewise, you can also arrange the averages by `avg_pressure`:

``` r
arrange(avg_wind_pressure_75, avg_pressure)
```

    ## # A tibble: 3 x 3
    ##   name     avg_wind avg_pressure
    ##   <chr>       <dbl>        <dbl>
    ## 1 Doris        73.7         983.
    ## 2 Amy          46.5         995.
    ## 3 Caroline     38.9        1002.

The default behavior of `arrange()` is to organize rows in increasing
order. But what if you want to organize rows in decreasing order? No
problem, just use the auxiliary function `desc()` to indicate that rows
should be arranged decreasingly:

``` r
arrange(avg_wind_pressure_75, desc(avg_wind))
```

    ## # A tibble: 3 x 3
    ##   name     avg_wind avg_pressure
    ##   <chr>       <dbl>        <dbl>
    ## 1 Doris        73.7         983.
    ## 2 Amy          46.5         995.
    ## 3 Caroline     38.9        1002.

### Inspecting 1975 storm Amy

Let’s focus on a specific storm, for example storm `Amy` in 1975. For
sake of simplicity, we are going to create a table `amy75` containing
the values of this storm:

``` r
amy75 <- filter(storms75, name == "Amy")
amy75
```

    ## # A tibble: 30 x 13
    ##    name   year month   day  hour   lat  long status category  wind pressure
    ##    <chr> <dbl> <dbl> <int> <dbl> <dbl> <dbl> <chr>  <ord>    <int>    <int>
    ##  1 Amy    1975     6    27     0  27.5 -79   tropi… -1          25     1013
    ##  2 Amy    1975     6    27     6  28.5 -79   tropi… -1          25     1013
    ##  3 Amy    1975     6    27    12  29.5 -79   tropi… -1          25     1013
    ##  4 Amy    1975     6    27    18  30.5 -79   tropi… -1          25     1013
    ##  5 Amy    1975     6    28     0  31.5 -78.8 tropi… -1          25     1012
    ##  6 Amy    1975     6    28     6  32.4 -78.7 tropi… -1          25     1012
    ##  7 Amy    1975     6    28    12  33.3 -78   tropi… -1          25     1011
    ##  8 Amy    1975     6    28    18  34   -77   tropi… -1          30     1006
    ##  9 Amy    1975     6    29     0  34.4 -75.8 tropi… 0           35     1004
    ## 10 Amy    1975     6    29     6  34   -74.8 tropi… 0           40     1002
    ## # … with 20 more rows, and 2 more variables: ts_diameter <dbl>,
    ## #   hu_diameter <dbl>

Here’s a coupe of questions that we could investigate:

  - which are the `status` categories for Amy?
  - during which months was Amy active? and for how many days?
  - what are the basic summary statistics for `wind` and `pressure`?

<!-- end list -->

``` r
# which are the `status` categories for Amy?
distinct(amy75, status)
```

    ## # A tibble: 2 x 1
    ##   status             
    ##   <chr>              
    ## 1 tropical depression
    ## 2 tropical storm

``` r
# during which months was Amy active?
distinct(amy75, month)
```

    ## # A tibble: 2 x 1
    ##   month
    ##   <dbl>
    ## 1     6
    ## 2     7

``` r
# for how many days was Amy active?
count(distinct(amy75, day))
```

    ## # A tibble: 1 x 1
    ##       n
    ##   <int>
    ## 1     8

``` r
# summary statistics for wind
summary(select(amy75, wind))
```

    ##       wind      
    ##  Min.   :25.00  
    ##  1st Qu.:31.25  
    ##  Median :50.00  
    ##  Mean   :46.50  
    ##  3rd Qu.:60.00  
    ##  Max.   :60.00

``` r
# summary statistics for pressure
summary(select(amy75, pressure))
```

    ##     pressure     
    ##  Min.   : 981.0  
    ##  1st Qu.: 986.0  
    ##  Median : 987.0  
    ##  Mean   : 995.1  
    ##  3rd Qu.:1005.5  
    ##  Max.   :1013.0

### Summary

So far, we’ve covered several functions from `"dplyr"`, as well as some
other functions in R:

  - functions from `"dplyr"`
      - `pull()` and `select()`
      - `filter()`
      - `group_by()`
      - `arrange()` and `desc()`
      - `count()`, `distinct()`, `summarise()`
  - functions in base R
      - `unique()`, `sort()`, `mean()`, `summary()`

### Your Turn

**1)** Use `"dplyr"` functions/commands to create a table (e.g. tibble)
`storms_per_year` containing the number of storms recorded in each year
(i.e. counts or frequencies of storms in each year). This table should
contain two columns: year values in the first column, and number of
storms in the second column.

**2)** Use `"dplyr"` functions/commands to create a table (e.g. tibble)
`storm_records_per_year` containing three columns: 1) `name` of storm,
2) `year` of storm, and 3) `count` for number of records (of the
corresponding storm).

**3)** Use `"dplyr"` functions/commands to create a table (e.g. tibble)
`storms_categ5` containing the name and year of those storms of category
5.

**4)** Use `"dplyr"` functions/commands to display a table showing the
`status`, `avg_pressure` (average pressure), and `avg_wind` (average
wind speed), for each type of storm `category`. This table should
contain four columns: 1) `category`, 2) `status`, 3) `avg_pressure`, and
4) `avg_wind`.

**5)** Use `"dplyr"` functions/commands to create a table (e.g. tibble)
`max_wind_per_storm` containing three columns: 1) `year` of storm, 2)
`name` of storm, and 3) `max_wind` maximum wind speed record (for that
storm).

**6)** Use `"dplyr"` functions/commands to create a table (e.g. tibble)
`max_wind_per_year` containing three columns: 1) `year` of storm, 2)
`name` of storm, and 3) `wind` maximum wind speed record (for that
year). Arrange rows by wind speed in decreasing order.
