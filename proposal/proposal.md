Project proposal
================
ctrl\_alt\_elite

``` r
library(tidyverse)
```

    ## Warning in system("timedatectl", intern = TRUE): running command 'timedatectl'
    ## had status 1

``` r
tweets <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2021/2021-06-15/tweets.csv')
```

    ## Rows: 445 Columns: 13

    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (5): content, text, username, location, url
    ## dbl  (6): retweet_count, like_count, quote_count, followers, lat, long
    ## lgl  (1): verified
    ## dttm (1): datetime

    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
tweets
```

    ## # A tibble: 445 × 13
    ##    datetime            content     retweet_count like_count quote_count text    
    ##    <dttm>              <chr>               <dbl>      <dbl>       <dbl> <chr>   
    ##  1 2021-05-07 11:42:29 "@sqlsekou…             0          1           0 "<a hre…
    ##  2 2021-05-06 16:14:46 "#DuBoisCh…             0          0           0 "<a hre…
    ##  3 2021-05-06 16:14:45 "#DuBoisCh…             0          0           0 "<a hre…
    ##  4 2021-05-06 16:14:45 "Was doing…             0          4           0 "<a hre…
    ##  5 2021-04-30 18:48:18 "@ClintSmi…             0         11           0 "<a hre…
    ##  6 2021-04-30 14:41:53 "@johnspac…             0          0           0 "<a hre…
    ##  7 2021-04-22 15:01:44 "For #Port…             3         58           0 "<a hre…
    ##  8 2021-04-12 00:39:44 "@zanmuny …             0          0           0 "<a hre…
    ##  9 2021-04-09 12:39:16 "Just lear…             1          1           0 "<a hre…
    ## 10 2021-04-08 22:00:21 "Honorable…             1          6           0 "<a hre…
    ## # … with 435 more rows, and 7 more variables: username <chr>, location <chr>,
    ## #   followers <dbl>, url <chr>, verified <lgl>, lat <dbl>, long <dbl>

## Dataset

A brief description of your dataset including its provenance,
dimensions, etc. as well as the reason why you chose this dataset.

Make sure to load the data and use inline code for some of this
information.

For our dataset, we have chosen to examine the 2021 WEB Du Bois &
Juneteenth Twitter data where each observation is a tweet and the
accompanying metadata such as likes, retweets, and location. The dataset
includes 13 different variables and a total of 445 observations.

## Questions

The two questions you want to answer.

## Analysis plan

A plan for answering each of the questions including the variables
involved, variables to be created (if any), external data to be merged
in (if any).
