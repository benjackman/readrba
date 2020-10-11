
<!-- README.md is generated from README.Rmd. Please edit that file -->

# readrba

<!-- badges: start -->

[![Lifecycle:
experimental](https://img.shields.io/badge/lifecycle-experimental-orange.svg)](https://www.tidyverse.org/lifecycle/#experimental)
[![R build
status](https://github.com/MattCowgill/readrba/workflows/R-CMD-check/badge.svg)](https://github.com/MattCowgill/readrba/actions)
[![Travis build
status](https://travis-ci.com/MattCowgill/readrba.svg?branch=master)](https://travis-ci.com/MattCowgill/readrba)
[![Codecov test
coverage](https://codecov.io/gh/MattCowgill/readrba/branch/master/graph/badge.svg)](https://codecov.io/gh/MattCowgill/readrba?branch=master)

<!-- badges: end -->

Get data from the [Reserve Bank of
Australia](https://rba.gov.au/statistics/tables/) in a
[tidy](https://cran.r-project.org/web/packages/tidyr/vignettes/tidy-data.html)
[tibble](https://tibble.tidyverse.org).

This package is still in active development. Some aspects of its
functionality are incomplete.

## Installation

The package is not yet on CRAN. Install from GitHub:

``` r
remotes::install_github("mattcowgill/readrba")
```

## Examples

There is one main function in {readrba}: `read_rba()`.

Here’s how you fetch the current version of a single RBA statistical
table: table G1, consumer price inflation:

``` r
cpi <- readrba::read_rba(table_no = "g1")
```

The object returned by `read_rba()` is a tidy tibble (ie. in ‘long’
format):

``` r
dplyr::glimpse(cpi)
#> Rows: 3,843
#> Columns: 11
#> $ date        <date> 1922-06-01, 1922-09-01, 1922-12-01, 1923-03-01, 1923-06-…
#> $ series      <chr> "Consumer price index", "Consumer price index", "Consumer…
#> $ value       <dbl> 2.8, 2.8, 2.7, 2.7, 2.8, 2.9, 2.9, 2.8, 2.8, 2.8, 2.8, 2.…
#> $ frequency   <chr> "Quarterly", "Quarterly", "Quarterly", "Quarterly", "Quar…
#> $ series_type <chr> "Original", "Original", "Original", "Original", "Original…
#> $ units       <chr> "Index, 2011/12=100", "Index, 2011/12=100", "Index, 2011/…
#> $ source      <chr> "ABS / RBA", "ABS / RBA", "ABS / RBA", "ABS / RBA", "ABS …
#> $ pub_date    <date> 2020-07-30, 2020-07-30, 2020-07-30, 2020-07-30, 2020-07-…
#> $ series_id   <chr> "GCPIAG", "GCPIAG", "GCPIAG", "GCPIAG", "GCPIAG", "GCPIAG…
#> $ description <chr> "Consumer price index; All groups", "Consumer price index…
#> $ table_title <chr> "G1 CONSUMER PRICE INFLATION", "G1 CONSUMER PRICE INFLATI…
```

You can also request multiple tables. They’ll be returned together as
one tidy tibble:

``` r
rba_data <- readrba::read_rba(table_no = c("a1", "g1"))

dplyr::glimpse(rba_data)
#> Rows: 17,603
#> Columns: 11
#> $ date        <date> 1994-06-01, 1994-06-08, 1994-06-15, 1994-06-22, 1994-06-…
#> $ series      <chr> "Australian dollar investments", "Australian dollar inves…
#> $ value       <dbl> 13680, 13055, 13086, 12802, 13563, 12179, 14325, 12563, 1…
#> $ frequency   <chr> "Weekly", "Weekly", "Weekly", "Weekly", "Weekly", "Weekly…
#> $ series_type <chr> "Original", "Original", "Original", "Original", "Original…
#> $ units       <chr> "$ million", "$ million", "$ million", "$ million", "$ mi…
#> $ source      <chr> "RBA", "RBA", "RBA", "RBA", "RBA", "RBA", "RBA", "RBA", "…
#> $ pub_date    <date> 2020-10-09, 2020-10-09, 2020-10-09, 2020-10-09, 2020-10-…
#> $ series_id   <chr> "ARBAAASTW", "ARBAAASTW", "ARBAAASTW", "ARBAAASTW", "ARBA…
#> $ description <chr> "Australian dollar investments", "Australian dollar inves…
#> $ table_title <chr> "A1 RESERVE BANK OF AUSTRALIA - LIABILITIES AND ASSETS - …

unique(rba_data$table_title)
#> [1] "A1 RESERVE BANK OF AUSTRALIA - LIABILITIES AND ASSETS - SUMMARY"
#> [2] "G1 CONSUMER PRICE INFLATION"
```

By default, `read_rba()` fetches the current version of whatever table
you request. You can specify the historical version of a table, if it’s
available, using the `cur_hist` argument:

``` r

hist_a11 <- readrba::read_rba(table_no = "a1.1", cur_hist = "historical")

dplyr::glimpse(hist_a11)
#> Rows: 8,889
#> Columns: 11
#> $ date        <date> 1977-07-31, 1977-08-31, 1977-09-30, 1977-10-31, 1977-11-…
#> $ series      <chr> "Australian Government Deposits", "Australian Government …
#> $ value       <dbl> 654, 665, 695, 609, 560, 614, 596, 505, 688, 642, 587, 64…
#> $ frequency   <chr> "Monthly", "Monthly", "Monthly", "Monthly", "Monthly", "M…
#> $ series_type <chr> "Original; average of weekly data for the month", "Origin…
#> $ units       <chr> "$ million", "$ million", "$ million", "$ million", "$ mi…
#> $ source      <chr> "RBA", "RBA", "RBA", "RBA", "RBA", "RBA", "RBA", "RBA", "…
#> $ pub_date    <date> 2015-06-26, 2015-06-26, 2015-06-26, 2015-06-26, 2015-06-…
#> $ series_id   <chr> "ARBALDOGIAG", "ARBALDOGIAG", "ARBALDOGIAG", "ARBALDOGIAG…
#> $ description <chr> "Australian Government Deposits", "Australian Government …
#> $ table_title <chr> "A1.1 RESERVE BANK OF AUSTRALIA - LIABILITIES AND ASSETS"…
```

# Data availability

The `read_rba()` function is able to import most tables on the
[Statistical Tables](https://rba.gov.au/statistics/tables/) page of the
RBA website. These are the tables that are downloaded when you use
`read_rba(cur_hist = "current")`, the default.

`read_rba()` can also download many of the tables on the [Historical
Data](https://rba.gov.au/statistics/historical-data.html) page of the
RBA website. To get these, specify `cur_hist = "historical"` in
`read_rba()`.

## Historical exchange rate tables

The historical exchange rate tables do not have table numbers on the RBA
website. They can still be downloaded, using the following table
numbers:

| Table title                                                                      | table\_no          |
| :------------------------------------------------------------------------------- | :----------------- |
| Exchange Rates – Daily – 1983 to 1986                                            | ex\_daily\_8386    |
| Exchange Rates – Daily – 1987 to 1990                                            | ex\_daily\_8790    |
| Exchange Rates – Daily – 1991 to 1994                                            | ex\_daily\_9194    |
| Exchange Rates – Daily – 1995 to 1998                                            | ex\_daily\_9598    |
| Exchange Rates – Daily – 1999 to 2002                                            | ex\_daily\_9902    |
| Exchange Rates – Daily – 2003 to 2006                                            | ex\_daily\_0306    |
| Exchange Rates – Daily – 2007 to 2009                                            | ex\_daily\_0709    |
| Exchange Rates – Daily – 2010 to 2013                                            | ex\_daily\_1013    |
| Exchange Rates – Daily – 2014 to 2017                                            | ex\_daily\_1417    |
| Exchange Rates – Daily – 2018 to Current                                         | ex\_daily\_18cur   |
| Exchange Rates – Monthly – January 2010 to latest complete month of current year | ex\_monthly\_10cur |
| Exchange Rates – Monthly – July 1969 to December 2009                            | ex\_monthly\_6909  |

Table numbers for historical exchange rate tables

## Non-standard tables

`read_rba()` is currently only able to import RBA statistical tables
that are formatted in a (more or less) standard way. Some are formatted
in a non-standard way, either because they’re distributions rather than
time series, or because they’re particularly old.

Tables that are **not** able to be downloaded are:

| Table title                                                               | table\_no |
| :------------------------------------------------------------------------ | :-------- |
| Daily Foreign Exchange Market Intervention Transactions                   | A5        |
| Household Balance Sheets – Distribution                                   | E3        |
| Household Gearing – Distribution                                          | E4        |
| Household Financial Assets – Distribution                                 | E5        |
| Household Non-Financial Assets – Distribution                             | E6        |
| Household Debt – Distribution                                             | E7        |
| Open Market Operations – 2012 to 2013                                     | A3        |
| Open Market Operations – 2009 to 2011                                     | A3        |
| Open Market Operations – 2003 to 2008                                     | A3        |
| Individual Banks’ Assets – 1991–1992 to 1997–1998                         | J1        |
| Individual Banks’ Liabilities – 1991–1992 to 1997–1998                    | J2        |
| Treasury Note Tenders - 1989–2006                                         | E4        |
| Treasury Bond Tenders – 1982–2006                                         | E5        |
| Treasury Bond Tenders – Amount Allotted, by Years to Maturity – 1982–2006 | E5        |
| Treasury Bond Switch Tenders – 2008                                       | E6        |
| Treasury Capital Indexed Bonds – 1985–2006                                | E7        |
| Capital Market Yields – Government Bonds – Daily – 1995 to 17 May 2013    | F2        |
| Capital Market Yields – Government Bonds – Monthly – 1969 to May 2013     | F2        |
| Indicative Mid Rates of Australian Government Securities – 1992 to 2008   | F16       |
| Indicative Mid Rates of Australian Government Securities – 2009 to 2018   | F16       |
| Zero-coupon Interest Rates – Analytical Series – 1992 to 2008             | F17       |

Tables not currently readable with `read_rba()`
