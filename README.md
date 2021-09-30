Project title
================
Ctrl Alt Elite

``` r
library(tidyverse)
```

    ## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.1 ──

    ## ✓ ggplot2 3.3.5     ✓ purrr   0.3.4
    ## ✓ tibble  3.1.4     ✓ dplyr   1.0.7
    ## ✓ tidyr   1.1.3     ✓ stringr 1.4.0
    ## ✓ readr   2.0.1     ✓ forcats 0.5.1

    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
library(maps)
```

    ## 
    ## Attaching package: 'maps'

    ## The following object is masked from 'package:purrr':
    ## 
    ##     map

``` r
library(sf)
```

    ## Linking to GEOS 3.8.0, GDAL 3.0.4, PROJ 6.3.1

``` r
library(rnaturalearth)
library(rvest)
```

    ## 
    ## Attaching package: 'rvest'

    ## The following object is masked from 'package:readr':
    ## 
    ##     guess_encoding

``` r
library(ggthemes)
library(lubridate)
```

    ## 
    ## Attaching package: 'lubridate'

    ## The following objects are masked from 'package:base':
    ## 
    ##     date, intersect, setdiff, union

``` r
library(stringr)
library(colorspace)
```

``` r
knitr::opts_chunk$set(
  fig.width = 8, # 8"
  fig.asp = 0.618, # the golden ratio
  fig.retina = 3, # dpi multiplier for displaying HTML output on retina
  dpi = 300, # higher dpi, sharper image
  out.width = "90%"
)
```

``` r
tweets <- read.csv("data/tweets.csv")
```

## Introduction

(1-2 paragraphs) Brief introduction to the dataset. You may repeat some
of the information about the dataset provided in the introduction to the
dataset on the TidyTuesday repository, paraphrasing on your own terms.
Imagine that your project is a standalone document and the grader has no
prior knowledge of the dataset.

    ## Rows: 445
    ## Columns: 13
    ## $ datetime      <fct> 2021-05-07T11:42:29Z, 2021-05-06T16:14:46Z, 2021-05-06T1…
    ## $ content       <fct> "@sqlsekou You co-hosted the #DuBoisChallenge. That had …
    ## $ retweet_count <int> 0, 0, 0, 0, 0, 0, 3, 0, 1, 1, 0, 0, 1, 0, 0, 1, 1, 0, 0,…
    ## $ like_count    <int> 1, 0, 0, 4, 11, 0, 58, 0, 1, 6, 5, 4, 5, 4, 4, 10, 0, 0,…
    ## $ quote_count   <int> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,…
    ## $ text          <fct> <a href="http://twitter.com/download/android" rel="nofol…
    ## $ username      <fct> AlDatavizguy, AlDatavizguy, AlDatavizguy, AlDatavizguy, …
    ## $ location      <fct> "New York", "New York", "New York", "New York", "New Yor…
    ## $ followers     <int> 1113, 1113, 1113, 1113, 1113, 5413, 3097, 1113, 642, 111…
    ## $ url           <fct> https://twitter.com/AlDatavizguy/status/1390633129203879…
    ## $ verified      <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, …
    ## $ lat           <dbl> 40.71273, 40.71273, 40.71273, 40.71273, 40.71273, 36.220…
    ## $ long          <dbl> -74.00602, -74.00602, -74.00602, -74.00602, -74.00602, -…

\#date and device type \#followers and device type \#faceted density
plots of like count

W.E.B DuBois was a famed data visualisation expert who conducted data
analysis to challenge the racist notions regarding African Americans in
the early 20th century. The \#DuBoisChallenge celebrates the legacy of
W.E.B DuBois by encouraging users to creatively recreate various
visualizations from the 1900 Paris Exposition using modern data
visualization tools.

For our dataset (tweets), we have chosen to examine the 2021 WEB Du Bois
& Juneteenth Twitter data where each observation is a tweet from 2021
that uses the DuBoisChallenge hashtag, and the variables are the
accompanying metadata such as likes, retweets, and location. The dataset
includes 13 different variables and a total of 445 observations. The
data was collected via scraping from Twitter based on the presence of
the WEB Du Bois challenge hashtag.

The following variables are included in the data set:

  - `datetime` (double): Date and time of tweet
  - `content` (character): Text for tweet
  - `retweet_count` (double): Retweet count for tweet
  - `like_count` (double): Like count for tweet
  - `quote_count` (double): Quote tweet count for tweet
  - `text` (character): Where tweet was posted from
  - `username` (character): Username of Tweeter
  - `location` (character): Location tweeted from
  - `followers` (double): Followers of the tweeter
  - `url` (character): Canonical url of tweet
  - `verified` (logical): Is user verified?
  - `lat` (double): Latitude of user
  - `long` (double): Longitude of user

### Introduction

(1-2 paragraphs) Introduction to the question and what parts of the
dataset are necessary to answer the question. Also discuss why you’re
interested in this question.

Since users in this challenge were from all across the world we wanted
to create visualizations to gauge interest and participation by
geographical locations. Specifically, there are 2 ways that we
operationalized geographical locations: home location provided manually
by the twitter user (`lat`and `long`) and the location at time of tweet
(`location`). We wanted to quantify geographical interest by the number
of retweets (`retweet_count`) and number of tweets coming from a given
location (mutated variable - n), which we will use as our dependent
variables in the 2 visualizations.

The reason we were very interested in this questions is because we
assumed that DuBois’ work would mostly be studied and know by Americans,
as it depicts experiences of African Americans. However, since people
are participating in the TidyTuesday challenge across the globe, we
wanted to understand if and where there is widespread interest in
DuBois’ work from an international perspective.

### Approach

(1-2 paragraphs) Describe what types of plots you are going to make to
address your question. For each plot, provide a clear explanation as to
why this plot (e.g. boxplot, barplot, histogram, etc.) is best for
providing the information you are asking about. The two plots should be
of different types, and at least one of the two plots needs to use
either color mapping or facets.

For our 1st visualization, we wanted to understand what the most popular
locations at the time of tweets were for the \#DuBoisChallenge. To do
so, we first cleaned the `location` column to ensure that we only had
valid locations. For a few cases, we had to manually either drop the
data point or rename them appropriately. After this, we added the
corresponding state/international country for each data point in the
`location` column. We then ranked the top 10 states/international
countries (excluding NA’s) based on the number of total acuumulated in
that location. To visualize this, we decided to utilize a bar plot since
it is easy to visualize the number of tweets from locations in
descending order. We also decided to fill the bars based on if they were
in the US or not, which allows users to distinguish the different types
of locations. We also decided to state the percentages on top of each
bar plot to make it more intuitive and accessible.

For our 2nd visualization, we wanted to physically map out the trends in
terms of the home locations of the twitter users. Based on the results
of our 1st visualization, we wanted to specifically look into the
locations that seemed to be the most active in terms tweeting with the
\#DuBoisChallenge. Since we had `lat` and `long`, we wanted to visualize
these points on a map, so that we could have more precise home locations
of the twitter users and readers can better spatially understand
participation across the world. We also wanted to understand the sharing
of tweets, and therefore, we sized each point on the map based on
`retweet_count`.

### Analysis

(2-3 code blocks, 2 figures, text/code comments as needed) In this
section, provide the code that generates your plots. Use scale functions
to provide nice axis labels and guides. You are welcome to use theme
functions to customize the appearance of your plot, but you are not
required to do so. All plots must be made with ggplot2. Do not use base
R or lattice plotting functions.

``` r
tweets_locations <- tweets %>% 
  filter(!str_detect(location, "@"), !str_detect(location, ":")) %>% 
  mutate(location_pre_comma = gsub(",.*","", location)) %>%
  mutate(location_pre_comma = case_when(
    location_pre_comma == "北京" ~ "Beijing",
    location_pre_comma == "God's earth" ~ "NA",
    location_pre_comma == "The City College of New York" ~ "New York",
    location_pre_comma == "World" ~ "NA",
    location_pre_comma == "he/they" ~ "NA",
    location_pre_comma == "At the home office" ~ "NA",
    location_pre_comma == "Deutschland" ~ "Germany",
    location_pre_comma == "Distrito Federal" ~ "Mexico City",
    location_pre_comma == "down in dey wid em" ~ "NA",
    location_pre_comma == "Forde-Obama Hall" ~ "NA",
    location_pre_comma == "France & UK" ~ "NA",
    location_pre_comma == "Lil’ Rudyshire" ~ "NA",
    location_pre_comma == "MIT" ~ "Boston",
    location_pre_comma == "New Yorker" ~ "New York",
    location_pre_comma == "OAK / NYC / ATL / The World" ~ "NA",
    location_pre_comma == "SP" ~ "NA",
    location_pre_comma == "Toronto || Ottawa" ~ "NA",
    location_pre_comma == "Tx" ~ "Texas",
    location_pre_comma == "UK" ~ "United Kingdom",
    location_pre_comma == "USA" ~ "United States",
    location_pre_comma == "Worldwide" ~ "NA",
    TRUE ~ location_pre_comma
  ))
  
tweets_locations <- tweets_locations %>%
  mutate(plot_state = case_when(
    location_pre_comma == "Nashville" ~ "Tennessee",
    location_pre_comma =="Merced" ~ "California",
    location_pre_comma == "Vienna" ~ "Austria",
    location_pre_comma == "Madison" ~ "Wisconsin",
    location_pre_comma == "Minneapolis" ~ "Minnesota",
    location_pre_comma == "Philadelphia" ~ "Pennsylvania",
    location_pre_comma == "Boston" ~ "Massachusetts",
    location_pre_comma == "London" ~ "United Kingdom",
    location_pre_comma == "Edinburgh" ~ "United Kingdom",
    location_pre_comma == "Amherst" ~ "Massachusetts",
    location_pre_comma == "Buffalo" ~ "New York",
    location_pre_comma == "Cambridge" ~ "Massachusetts",
    location_pre_comma == "San Diego" ~ "California",
    TRUE ~ as.character(location_pre_comma)
  ))

top_10_locations <- tweets_locations %>%
  count(plot_state) %>%
  arrange(desc(n)) %>%
  filter(plot_state != "NA") %>%
  head(10) %>%
  mutate(internat = case_when(
    plot_state == "United Kingdom" ~ "International",
    plot_state == "Austria" ~ "International",
    TRUE ~ "Domestic"),
    plot_state = fct_reorder(plot_state, n),
    percent_tweets = paste(round(n / sum(n), 4) * 100, "%"))


tweets <- tweets %>%
  mutate(devicetype = case_when(
    str_detect(text, "Twitter Web App") ~ "Web App",
    str_detect(text, "Twitter for iPhone") ~ "iPhone",
    str_detect(text, "Twitter for Android") ~ "Android",
    str_detect(text, "Buffer") ~ "Buffer",
    str_detect(text, "Twitter for iPad") ~ "iPad",
    str_detect(text, "TweetDeck") ~ "TweetDeck",
    str_detect(text, "Crowdfire App") ~ "Crowdfire App",
    str_detect(text, "Twitter for Mac") ~ "Mac"
  ))

tweets <- tweets %>%
  mutate(tag_count = str_count(content, "@"))
```

``` r
ggplot(data = top_10_locations, aes(y = plot_state, x = n, fill = internat)) +
  geom_col() +
  geom_text(aes(label = percent_tweets, color = internat), 
            size = 2.5,
            nudge_x = 8, 
            show.legend = FALSE) +
  scale_fill_colorblind() +
  scale_color_colorblind() +
  scale_x_continuous(breaks = c(0, 20, 40, 60, 80, 100, 120, 140, 160, 180, 200)) +
  labs(
    y = "Region",
    x = "Number of Tweets",
    title = "Top 10 Most Popular #DuBoisChallenge Tweet Locations",
    fill = "Location"
  ) +
  theme_minimal() +
  theme(text = element_text(family = "Times New Roman"))
```

<img src="README_files/figure-gfm/question-one-plot-one-1.png" width="90%" />

``` r
northeast_tweets <- tweets %>%
  filter(long <= -70,
         long >= -90,
         lat <= 46,
         lat >= 20)

europe_tweets <- tweets %>% 
  filter(long >= -20 & long <= 45,
         lat >= 30 & lat <= 73)
```

``` r
# world map/eu code taken from https://www.r-bloggers.com/2019/04/zooming-in-on-maps-with-sf-and-ggplot2/
world_map <- ne_countries(scale = 'medium', type = 'map_units',
                         returnclass = 'sf')
us_map <- map_data("state")
canada_map <- map_data("world","canada")
```

``` r
# Add labels for states and Europe countries

# Do we need to address showing relative populations like we said we would in 
# proposal cause of peer feedback??

# Need to address why there are so many tweets from NY twitter location but
# based on long lat there are hardly any in NY in map

ggplot() +
  geom_polygon(data = canada_map, 
               aes(x = long, y = lat, group = group), 
               fill = "lightgray", 
               color = "black") +
  geom_polygon(data = us_map, 
               aes(x = long, y = lat, group = group), 
               fill = "lightgray", color = "black") +
  geom_point(data = northeast_tweets, 
             aes(x = long, y = lat, size = tag_count, color = tag_count)) +
  coord_map(xlim = c(-80, -65),
            ylim = c(36, 46)
            ) +
  labs(title = "#DuBoisChallenge Tweets in the North-East and Canada",
       size = "Number of Retweets",
       alpha = "Number of Retweets") +
  theme_void() +
  theme(text = element_text(family = "Times New Roman")) +
  scale_color_continuous_sequential(palette = "YlGnBu", rev = TRUE)
```

<img src="README_files/figure-gfm/question-one-plot-two-1.png" width="90%" />

``` r
ggplot() + 
  geom_sf(data = world_map, fill = "#F0F0F0", color = "black") +
  geom_jitter(data = europe_tweets, 
             aes(x = long, y = lat, size = tag_count, color = tag_count)) +
  labs(title = "#DuBoisChallenge Tweets in Europe",
       size = "Number of Retweets") +
  theme_void() +
  coord_sf(xlim = c(-20, 45), ylim = c(30, 73), expand = FALSE) +
  theme(text = element_text(family = "Times New Roman")) +
  scale_color_continuous_sequential(palette = "YlGnBu", rev = TRUE)
```

<img src="README_files/figure-gfm/question-one-plot-two-2.png" width="90%" />

### Discussion

(1-3 paragraphs) In the Discussion section, interpret the results of
your analysis. Identify any trends revealed (or not revealed) by the
plots. Speculate about why the data looks the way it does.

## Question 2 \<- Update title to relate to the question you’re answering

The second question we want to answer is:

*How does the tweet timing affect user construction of \#DuBois
challenge tweets and how the audience reacts to those tweets?*

### Introduction

(1-2 paragraphs) Introduction to the question and what parts of the
dataset are necessary to answer the question. Also discuss why you’re
interested in this question.

### Approach

(1-2 paragraphs) Describe what types of plots you are going to make to
address your question. For each plot, provide a clear explanation as to
why this plot (e.g. boxplot, barplot, histogram, etc.) is best for
providing the information you are asking about. The two plots should be
of different types, and at least one of the two plots needs to use
either color mapping or facets.

### Analysis

(2-3 code blocks, 2 figures, text/code comments as needed) In this
section, provide the code that generates your plots. Use scale functions
to provide nice axis labels and guides. You are welcome to use theme
functions to customize the appearance of your plot, but you are not
required to do so. All plots must be made with ggplot2. Do not use base
R or lattice plotting functions.

``` r
tweets %>%
  count(devicetype)
```

    ##      devicetype   n
    ## 1       Android  25
    ## 2        Buffer   1
    ## 3 Crowdfire App   2
    ## 4          iPad   7
    ## 5        iPhone  39
    ## 6           Mac   2
    ## 7     TweetDeck  16
    ## 8       Web App 352
    ## 9          <NA>   1

``` r
tweets_time <- tweets %>% 
  filter(!is.na(datetime)) %>%
  mutate(tag_count = str_count(content, "@"),
         time = as.numeric(str_sub(datetime, start=12, end=13)),
         date = ymd(str_sub(datetime, start = 1, end = 11)),
         month = month(date),
         month_name = case_when(
           month == 2 ~ "February",
           month == 3 ~ "March",
           month == 4 ~ "April",
           month == 5 ~ "May"),
         month_name = fct_reorder(month_name, month)
         )
```

``` r
tweets_time %>% 
  filter(month_name != "May") %>% 
ggplot(aes(x = month_name, y = tag_count, color = month_name)) + 
  geom_violin(show.legend = FALSE) +
  scale_color_colorblind() +
  scale_y_continuous(breaks = c(0, 2, 4, 6, 8, 10, 12)) +
  labs(
    x = "Month Tweeted",
    y = "Number of People Tagged",
    title = "When #DuBoisChallenge Tweeters Tagged People"
  ) +
  theme(text = element_text(family = "Times New Roman"))
```

<img src="README_files/figure-gfm/question-two-plot-one-1.png" width="90%" />

``` r
tweets_time %>% 
  filter(month_name != "May") %>% 
ggplot(aes(x = like_count)) + 
  facet_wrap(~ month_name, scales = "free_x") +
  geom_histogram() +
  scale_color_colorblind() 
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

<img src="README_files/figure-gfm/question-two-plot-two-1.png" width="90%" />

``` r
tweets %>%
  mutate(tag_count = str_count(content, "@")) %>%
  filter(devicetype == "iPhone" | devicetype == "Web App"| devicetype == "Android") %>%
ggplot(aes(x = followers)) + 
  geom_density() + 
  facet_wrap(.~devicetype, scales = "free_y", nrow = 3) +
  scale_x_continuous(limit = c(0,8000))
```

    ## Warning: Removed 9 rows containing non-finite values (stat_density).

<img src="README_files/figure-gfm/unnamed-chunk-2-1.png" width="90%" />

``` r
tweets %>%
  filter(!is.na(like_count)) %>%
  summarise(IQR = IQR(like_count), Q1 = quantile(like_count, 0.25), Q3 = quantile(like_count, 0.75), 
            boundary = Q3 + 2 * IQR) 
```

    ##   IQR Q1 Q3 boundary
    ## 1  14  2 16       44

``` r
tweets %>%
  filter(like_count <= 44) %>%
  filter(devicetype == "iPhone" | devicetype == "Web App"| devicetype == "Android") %>%
  ggplot(aes(y = like_count)) + 
  facet_wrap(~devicetype) +
  geom_boxplot() +
  labs(x = "Device Type", y = "Like Count", 
       title = "Like Count based on Device Type")
```

<img src="README_files/figure-gfm/unnamed-chunk-3-1.png" width="90%" />

``` r
tweets %>%
  mutate(tag_count = str_count(content, "@"),
         time = as.numeric(str_sub(datetime, start=12, end=13)),
         date = ymd(str_sub(datetime, start = 1, end = 11)),
         month = month(date),
         month_name = case_when(
           month == 2 ~ "February",
           month == 3 ~ "March",
           month == 4 ~ "April",
           month == 5 ~ "May"),
         month_name = fct_reorder(month_name, month)
         ) %>%
  filter(devicetype == "iPhone" | devicetype == "Web App"| devicetype == "Android") %>%
  ggplot(aes(x = date, y = tag_count)) + 
  facet_wrap(~month) +
  geom_line()
```

<img src="README_files/figure-gfm/unnamed-chunk-4-1.png" width="90%" />

### Discussion

(1-3 paragraphs) In the Discussion section, interpret the results of
your analysis. Identify any trends revealed (or not revealed) by the
plots. Speculate about why the data looks the way it does.

## Presentation

Our presentation can be found [here](presentation/presentation.html).

## Data

Include a citation for your data here. See
<http://libraryguides.vu.edu.au/c.php?g=386501&p=4347840> for guidance
on proper citation for datasets. If you got your data off the web, make
sure to note the retrieval date.

## References

List any references here. You should, at a minimum, list your data
source.
