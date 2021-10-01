Twitter Trends from the \#DuBoisChallenge
================
by Ctrl Alt Elite

``` r
# Package messages suppressed
library(tidyverse)
library(maps)
library(sf)
library(rnaturalearth)
library(rvest)
library(ggthemes)
library(lubridate)
library(stringr)
library(colorspace)
library(scales)
library(ggrepel)
library(RColorBrewer)
```

``` r
knitr::opts_chunk$set(
  fig.width = 8,
  fig.asp = 0.618,
  fig.retina = 3,
  dpi = 300,
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

## Question 1 \<- Update title to relate to the question you’re answering

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
tweets <- tweets %>%
  mutate(tag_count = str_count(content, "@"))

tweets_locations <- tweets %>%
  filter(!str_detect(location, "@"), !str_detect(location, ":")) %>%
  mutate(
    location_pre_comma = gsub(",.*", "", location),
    location_pre_comma = case_when(
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
    ),
    plot_state = case_when(
      location_pre_comma == "Nashville" ~ "Tennessee",
      location_pre_comma == "Merced" ~ "California",
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
    )
  )

top_10_locations <- tweets_locations %>%
  count(plot_state) %>%
  arrange(desc(n)) %>%
  filter(plot_state != "NA") %>%
  head(10) %>%
  mutate(
    region = case_when(
      plot_state == "New York" ~ "US: Northeast",
      plot_state == "New Jersey" ~ "US: Northeast",
      plot_state == "Pennsylvania" ~ "US: Northeast",
      plot_state == "Massachusetts" ~ "US: Northeast",
      plot_state == "Tennessee" ~ "US: South",
      plot_state == "California" ~ "US: West",
      plot_state == "Austria" ~ "Europe",
      plot_state == "United Kingdom" ~ "Europe",
      plot_state == "Wisconsin" ~ "US: Midwest",
      plot_state == "Minnesota" ~ "US: Midwest"
    ),
    region = fct_relevel(region, c(
      "US: Northeast", "Europe", "US: Midwest", "US: South",
      "US: West"
    )),
    plot_state = fct_relevel(plot_state, c(
      "New York", "New Jersey", "Massachusetts",
      "Pennsylvania", "Austria", "United Kingdom",
      "Minnesota",
      "Wisconsin", "Tennessee", "California"
    )),
    plot_state = fct_rev(plot_state),
    percent_tweets = paste(round(n / sum(n), 4) * 100, "%")
  )

northeast_tweets <- tweets %>%
  filter(
    long <= -70,
    long >= -90,
    lat <= 46,
    lat >= 20
  )
europe_tweets <- tweets %>%
  filter(
    long >= -20 & long <= 45,
    lat >= 30 & lat <= 73
  )

world_map <- ne_countries(
  scale = "medium", type = "map_units",
  returnclass = "sf"
)
us_map <- map_data("state")
canada_map <- map_data("world", "canada")

US_locations <- tribble(
  ~city, ~lat, ~long,
  "NYC", 40.7128, -74.0060,
  "Baltimore", 39.2904, -76.6122
)

EU_locations <- tribble(
  ~city, ~lat, ~long,
  "London", 51.5074, -0.1278,
  "Vienna", 48.2082, 16.3738,
  "Rome", 41.9028, 12.4964
)
```

``` r
ggplot(data = top_10_locations, aes(y = plot_state, x = n, fill = region)) +
  geom_col() +
  geom_text(aes(label = percent_tweets, color = region),
    size = 2.5,
    nudge_x = 8,
    show.legend = FALSE
  ) +
  scale_fill_brewer(palette = "Set1") +
  scale_color_brewer(palette = "Set1") +
  scale_x_continuous(
    breaks = c(0, 20, 40, 60, 80, 100, 120, 140, 160, 180, 200)
  ) +
  labs(
    title = "Top 10 #DuBoisChallenge Tweet Locations",
    caption = "% calculated out of top 10",
    x = "Number of Tweets",
    y = "User Location in Twitter Bio",
    fill = "Location"
  ) +
  theme_minimal() +
  theme(
    plot.title = element_text(hjust = 0.5, face = "bold"),
    plot.caption = element_text(face = "italic"),
    legend.title = element_blank(),
    legend.position = c(.848, .25),
    text = element_text(family = "Times New Roman")
  )
```

<img src="README_files/figure-gfm/question-one-plot-one-1.png" width="90%" />

``` r
set.seed(1234)

ggplot() +
  geom_polygon(
    data = canada_map,
    aes(x = long, y = lat, group = group),
    fill = "#F0F0F0",
    color = "black"
  ) +
  geom_polygon(
    data = us_map,
    aes(x = long, y = lat, group = group),
    fill = "#F0F0F0", color = "black"
  ) +
  geom_jitter(
    data = northeast_tweets,
    width = 0.5, height = .5,
    aes(
      x = long, y = lat, size = tag_count, color = tag_count,
      alpha = tag_count
    )
  ) +
  labs(
    title = "#DuBoisChallenge Tweets",
    subtitle = "in Northeastern U.S. & Canada\n",
    caption = "Based on Location Where Tweet Was Published",
    size = "# of Users Tagged",
    color = "# of Users Tagged",
    alpha = "# of Users Tagged"
  ) +
  theme_void() +
  scale_color_continuous_sequential(
    palette = "OrYel", rev = FALSE,
    breaks = c(0, 2, 4, 6, 8, 10)
  ) +
  scale_size_continuous(range = c(2, 5), breaks = c(0, 2, 4, 6, 8, 10)) +
  scale_alpha_continuous(range = c(.27, 1), breaks = c(0, 2, 4, 6, 8, 10)) +
  guides(
    color = guide_legend(),
    size = guide_legend(),
    alpha = guide_legend()
  ) +
  theme(
    plot.title = element_text(hjust = 0.5, face = "bold"),
    plot.subtitle = element_text(hjust = 0.5, face = "italic"),
    plot.caption = element_text(hjust = 0.5),
    legend.position = "bottom",
    panel.border = element_rect(color = "black", fill = NA, size = .75),
    panel.background = element_rect(color = "black", fill = "lightblue"),
    text = element_text(family = "Times New Roman")
  ) +
  geom_text_repel(
    data = US_locations,
    aes(x = long, y = lat, label = city),
    size = 3, nudge_x = -0.15,
    nudge_y = 0.95,
    segment.linetype = "dotted"
  ) +
  coord_map(
    xlim = c(-80, -65),
    ylim = c(36, 46)
  )
```

<img src="README_files/figure-gfm/question-one-plot-two-1.png" width="90%" />

``` r
ggplot() +
  geom_sf(data = world_map, fill = "#F0F0F0", color = "black") +
  geom_jitter(
    data = europe_tweets,
    aes(
      x = long, y = lat, size = tag_count, color = tag_count,
      alpha = tag_count
    )
  ) +
  coord_sf(xlim = c(-20, 45), ylim = c(30, 73), expand = FALSE) +
  labs(
    title = "#DuBoisChallenge Tweets",
    subtitle = "in Europe\n",
    caption = "Based on Location Where Tweet Was Published",
    size = "# of Users Tagged",
    alpha = "# of Users Tagged",
    color = "# of Users Tagged"
  ) +
  theme_void() +
  scale_color_continuous_sequential(
    palette = "OrYel", rev = FALSE,
    breaks = c(0, 2, 4, 6, 8, 10)
  ) +
  scale_size_continuous(range = c(2, 5), breaks = c(0, 2, 4, 6, 8, 10)) +
  scale_alpha_continuous(range = c(.27, 1), breaks = c(0, 2, 4, 6, 8, 10)) +
  guides(
    color = guide_legend(),
    size = guide_legend(),
    alpha = guide_legend()
  ) +
  theme(
    plot.title = element_text(hjust = 0.5, face = "bold"),
    plot.subtitle = element_text(hjust = 0.5, face = "italic"),
    plot.caption = element_text(hjust = 0.5),
    legend.position = "bottom",
    panel.border = element_rect(color = "black", fill = NA, size = .75),
    panel.background = element_rect(color = "black", fill = "lightblue"),
    text = element_text(family = "Times New Roman")
  ) +
  geom_text_repel(
    data = EU_locations,
    aes(x = long, y = lat, label = city),
    size = 3,
    nudge_x = -14, nudge_y = -0.3,
    segment.linetype = "dotted"
  )
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

tweets_device <- tweets %>%
  filter(devicetype == "iPhone" |
    devicetype == "Web App" |
    devicetype == "Android")

users_and_devicetype <- tweets %>%
  group_by(username) %>%
  select(username, devicetype) %>%
  count(devicetype) %>%
  arrange(desc(n)) %>%
  slice(1) %>%
  select(-n)

users_and_avg_follower <- tweets %>%
  group_by(username) %>%
  summarise(avg_followers = mean(followers))

joined_devices_follower <- inner_join(users_and_devicetype,
  users_and_avg_follower,
  by = "username"
) %>%
  filter(devicetype == "iPhone" |
    devicetype == "Web App" |
    devicetype == "Android")
```

``` r
# Plot drops NAs and tweets with 0 likes since log(0) is undefined
ggplot(data = tweets_device, aes(x = like_count, fill = devicetype)) +
  facet_wrap(~devicetype, ncol = 1) +
  geom_boxplot(show.legend = FALSE) +
  scale_fill_manual(values = c("#a4c639", "#A2AAAD", "#1DA1F2")) +
  scale_x_log10() +
  labs(
    title = "Distribution of Likes",
    subtitle = "by Device Tweet Was Posted From",
    caption = "x-axis uses logarithmic scale (tweets with 0 likes are ommited)",
    x = "Number of Likes",
    y = NULL 
  ) +
  theme_minimal() +
  theme(
    plot.title = element_text(hjust = 0.5, face = "bold"),
    plot.subtitle = element_text(hjust = 0.5, face = "italic"),
    plot.caption = element_text(face = "italic", hjust = 0.5),
    axis.ticks = element_blank(),
    axis.text.y = element_blank(),
    text = element_text(family = "Times New Roman")
  )
```

<img src="README_files/figure-gfm/question-two-plot-one-1.png" width="90%" />

``` r
# Plot drops NAs and tweets with 0 likes since log(0) is undefined
ggplot(
  data = joined_devices_follower,
  aes(x = avg_followers, fill = devicetype)
) +
  geom_density(show.legend = FALSE) +
  facet_wrap(. ~ devicetype, nrow = 3) +
  theme_minimal() +
  scale_x_log10(labels = label_number(big.mark = ",")) +
  scale_fill_manual(values = c("#a4c639", "#A2AAAD", "#1DA1F2")) +
  scale_y_continuous(labels = label_number(accuracy = .1)) +
  labs(
    title = "Distribution of Follower Count",
    subtitle = "by User's Primary Device",
    caption = "x-axis uses logarithmic scale", 
    x = "Average Number of Followers Per User",
    y = NULL
  ) +
  theme(
    plot.title = element_text(hjust = 0.5, face = "bold"),
    plot.subtitle = element_text(hjust = 0.5, face = "italic"),
    plot.caption = element_text(face = "italic"),
    text = element_text(family = "Times New Roman")
  )
```

<img src="README_files/figure-gfm/question-two-plot-two-1.png" width="90%" />

### Discussion

(1-3 paragraphs) In the Discussion section, interpret the results of
your analysis. Identify any trends revealed (or not revealed) by the
plots. Speculate about why the data looks the way it does.

## Presentation

Our presentation can be found [here](presentation/presentation.html).

## Data

Starks, A, Hillery, A, Tyler, S 2021, *Du Bois and Juneteenth revisited:
TidyTuesday week 8 of 2021*, electronic dataset, tidytuesday, retrieved
13 September 2021,
<https://github.com/rfordatascience/tidytuesday/blob/master/data/2021/2021-06-15/readme.md>.

## References

[Data
Source](https://github.com/rfordatascience/tidytuesday/blob/master/data/2021/2021-06-15/readme.md)

[Grammarly
Article](https://www.theladders.com/career-advice/study-you-make-5-times-as-many-errors-writing-on-your-phone-than-on-your-computer)

[Android Logo
Color](https://encycolorpedia.com/a4c639#:~:text=The%20hexadecimal%20color%20code%20%23a4c639,%25%20green%20and%2022.35%25%20blue.)

[Apple Logo Color](https://www.brandcolorcode.com/apple)

[Twitter Logo
Color](https://www.onlinepalette.com/twitter/#:~:text=Twitter%20uses%20the%20colors%20blue,29%2C%20161%2C%20242)

[Europe
Map](https://www.r-bloggers.com/2019/04/zooming-in-on-maps-with-sf-and-ggplot2/)

[Latitude and Longitude of Cities](https://www.google.com/maps)
