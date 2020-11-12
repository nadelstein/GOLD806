Day 3.1): Analyzing Hurricanes
================

## Motivation

In this part, the goal is to perform an exploratory analysis of
**hurricanes data in the North Atlantic from years 1980 to 2019**,
focusing on four provided research claims.

### Research Claims

Below, we provide a list of four claims that you will have to either
confirm, deny, or clarify, using the results (numeric and visual
evidence) from your analysis.

1)  A typical hurricane season (during a calendar year) runs from June
    through November, but occasionally storms form outside those months.

2)  A typical year has 12 named storms, including six hurricanes of
    which three become major hurricanes (category 3, 4, and 5).

3)  September is the most active month (where most of the hurricanes
    occur), followed by August, and October.

4)  During the analyzed period (1980-2019), no hurricanes made U.S.
    landfall before June and after November.

## Data IBTrACS

The data comes from the *International Best Track Archive for Climate
Stewardship* (IBTrACS) website:

<https://www.ncdc.noaa.gov/ibtracs/index.php?name=ib-v4-access>

The specific dataset is part of **IBTrACS v04**, and you will be working
with its Comma Separated Values (CSV) file `ibtracs.NA.list.v04r00.csv`.
The link to download the CSV file is available in the following page:

<https://www.ncei.noaa.gov/data/international-best-track-archive-for-climate-stewardship-ibtracs/v04r00/access/csv/>

The associated **data dictionary** is in the following pdf file. This is
the document that provides detailed descriptions about the fields
(i.e. columns) of the CSV data file.

<https://www.ncdc.noaa.gov/ibtracs/pdf/IBTrACS_v04_column_documentation.pdf>

## 1\) Importing Data in R

You will have to download the CSV file to your computer.

You are allowed to use any data-table importing function, either with
base R functions e.g. `read.table()`, or with functions from other
packages: e.g. `read_table()` from `"readr"`.

Regardless of the importing approach you decide to use, you have to
import the data following these specifications:

  - Import only the first 16 columns (from `SID` to `LANDFALL`)

  - Specify data types for the 16 imported columns as:
    
      - `SEASON`, `NUMBER`, `WMO_WIND`, `WMO_PRES`, `DIST2LAND`, and
        `LANDFALL` must be of type `"integer"`
      - `LAT` and `LON` must be of type `"double"` or `"real"`
      - the rest of the columns must be of type `"character"`

  - Look at the **IBTrACS version 4** website and find how missing
    values are encoded. Not encoding missing values properly while
    importing the data in R might produce nonsensical results.

We want to make emphasis on the fact that the imported data frame (or
tibble) produced by the reading table function must return a table with
16 columns. In other words, you are NOT allowed to import the data,
obtain a table, and then select *a posteriori* the first 16 columns.

Below we provide auxiliary code that you can use as an optional template
to help you import the data into R. Keep in mind that there are multiple
ways in which you can perform the importing operation. You are allowed
to use other approaches if you want.

``` r
# ---------------------------------------------------------
# Optional code to guide your importing data steps.
# (BTW: there are other ways to achieve the same result)
# Notice that some lines are incomplete. If you decide to 
# use these commands, you will have to fill-in the blanks.
# ---------------------------------------------------------

# vector of names for first 16 columns
col_names <- c(
  "SID",
  ...  # fill-in with the rest of names!
  "LANDFALL"
)

# vector of data-types for first 16 columns
col_types <- c(
  "character",   # data-type of SID
  ...  # fill-in with the rest of data-types!
  "integer"      # data-type of LANDFALL
)

# suggestion for importing CSV file with "read.csv()"
dat <- read.csv(
  file = ...,   # specify name of file!
  colClasses = c(col_types, rep("NULL", 147)),
  stringsAsFactors = FALSE, 
  skip = 86272, # we're not interested in hurricanes before 1979
  na.strings = ...  # specify how mising values are encoded!
)

# renaming columns using vector col_names
colnames(dat) <- col_names
```

### 1.1) Adding a `MONTH` column

After importing the data, use the following code to add a new column
`MONTH` by extracting the month number from column `ISO_TIME`. You will
need this `MONTH` column to perform various analysis taking months into
account. This code assumes that your imported data is called `dat`; if
this is not the case then modify the code according to your own
preferences:

``` r
# adding month 
# (you may need to change the name of data.frame "dat")
dat$MONTH <- as.numeric(substr(dat$ISO_TIME, 6, 7))
```

Include the following code chunk to display the structure of your
imported data table. Again, this code assumes that the data frame is
called `dat`, feel free to change it according to your needs:

``` r
# display structure of your data with this command
# (your data object may have a different name)
str(dat, vec.len = 1)
```

## 2\) Univariate Exploratory Data Analysis

Following the descriptions and details of the **data dictionary**
document, perform an exploratory data analysis (eda) to check that
data/values in the columns make sense. For example, the fourth column is
`BASIN`, and it could include one or more of the following 7 categories:
`NA`, `EP`, `WP`, `NI`, `SI`, `SP`, `SA`. Based on this information, you
would need to explore what categories are in `BASIN`, and see if
everything makes sense.

We recommend exploring the following columns:

  - `SEASON`
  - `BASIN`
  - `SUBBASIN`
  - `ISO_TIME` (epxlore a handful of values along the rows of the data
    table and make sure they all have the adequate format)
  - `NATURE`
  - `LAT`
  - `LON`
  - `WMO_WIND`
  - `WMO_PRES`
  - `DIST2LAND`
  - `LANDFALL`

The type of analysis will depend on the nature of each column
(i.e. variable). For those categorical variables, perhaps you may want
to identify the unique categories, and obtain their frequencies (or also
relative frequencies). For those variables that have a more quantitative
flavor (e.g. `WMO_WIND`), it would be good if you look at their summary
statistics, and visualize their distribution (e.g. boxplots, histograms,
density curves). Keep in mind that this univariate EDA is intended to
“get to know the data, and have a sanity check of the available
values”.

## 3\) Further Analysis

Taking into account the four research claims, you will have to analyze
the data in order to provide numeric and visual evidence that support
your posture “in favor” or “against” each claim. You are allowed to use
summary tables, statistical charts, and of course maps. Keep in mind
that this analysis is decisively exploratory. We are not expecting that
you apply predictive models, or perform hypothesis tests, or other type
of inferential task. Having said that, you analysis, interpretation, and
conclusions should be sound.

## 4\) Report (`Rmd` + `html`)

Your report should include all your code, analysis, results,
interpretations, descriptions, etc., as well as your narrative. Above
all, do not simply include code chunks, with minimal descriptions, and a
boring list of conclusions for each research claim. We want to see your
“thinking process” and how you organize your workflow, all of this by
reading and looking at your report, without you there to explain it to
us. Therefore, your report must “speak for itself”.

  - What was your methodology/approach towards addressing the research
    claims?

  - Describe your data manipulation and exploration process, as well as
    your analytical steps.

  - Do not be afraid to use as many code chunks as necessary. Your code
    should be easy to read and understand, using descriptive names, well
    commented, and well organized.

  - For visualizations, describe motivations behind the particular ones
    built and what they illuminate. Make sure any visualizations are
    functionally labeled (e.g. title, possibly a subtitle, axis labels,
    units of measurement when applicable).

  - You must communicate your key findings and insights clearly.

  - Keep the content length of your report to no more than 4000 words.
    To give you a rough idea: single spaced, 4000 words yields about 8
    pages (excluding images). Obviously there are no pages in an html
    document, but use this guideline to keep track of your code and
    narrative.
