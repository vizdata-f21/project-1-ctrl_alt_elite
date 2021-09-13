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
glimpse(tweets)
```

    ## Rows: 445
    ## Columns: 13
    ## $ datetime      <dttm> 2021-05-07 11:42:29, 2021-05-06 16:14:46, 2021-05-06 16…
    ## $ content       <chr> "@sqlsekou You co-hosted the #DuBoisChallenge. That had …
    ## $ retweet_count <dbl> 0, 0, 0, 0, 0, 0, 3, 0, 1, 1, 0, 0, 1, 0, 0, 1, 1, 0, 0,…
    ## $ like_count    <dbl> 1, 0, 0, 4, 11, 0, 58, 0, 1, 6, 5, 4, 5, 4, 4, 10, 0, 0,…
    ## $ quote_count   <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,…
    ## $ text          <chr> "<a href=\"http://twitter.com/download/android\" rel=\"n…
    ## $ username      <chr> "AlDatavizguy", "AlDatavizguy", "AlDatavizguy", "AlDatav…
    ## $ location      <chr> "New York", "New York", "New York", "New York", "New Yor…
    ## $ followers     <dbl> 1113, 1113, 1113, 1113, 1113, 5413, 3097, 1113, 642, 111…
    ## $ url           <chr> "https://twitter.com/AlDatavizguy/status/139063312920387…
    ## $ verified      <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, …
    ## $ lat           <dbl> 40.71273, 40.71273, 40.71273, 40.71273, 40.71273, 36.220…
    ## $ long          <dbl> -74.00602, -74.00602, -74.00602, -74.00602, -74.00602, -…

## Dataset

For our dataset, we have chosen to examine the 2021 WEB Du Bois &
Juneteenth Twitter data where each observation is a tweet and the
accompanying metadata such as likes, retweets, and location. The dataset
includes 13 different variables and a total of 445 observations. The
data was collected via scraping from Twitter based on the presence of
the WEB Du Bois challenge hashtag.

We chose this dataset because we appreciate the importance of
celebrating W.E.B DuBois’s legacy in data visualization. The \#DuBois
challenge was a Tidy Tuesday challenge that asked users to recreate his
1900 Paris Exposition today. This exposition included visualizations
that described the income, wealth, employment, and mobility of African
Americans during this time period. By recreating these visualizations
through the \#DuBois challenge, users were able to understand the
conditions of former slaves in the United States and honor the legacy
and the progress of this group. Additionally, these sorts of
visualizations are crucial to understanding modern-day issues in
marginalized groups. From this data, we hope to learn how people engage
with data visualization in a social media setting.

We also chose the dataset because it is comprehensive to work with.
There are a variety of different types of variables - logical, double,
and character - and the dataset is well-cleaned.

## Questions

The two questions you want to answer.

## Analysis plan

A plan for answering each of the questions including the variables
involved, variables to be created (if any), external data to be merged
in (if any).
