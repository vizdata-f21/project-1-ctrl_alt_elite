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
# cleanFun <- function(htmlString) {
#   return(gsub("<.*?>", "", htmlString))
# }
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

## Question 1 \<- Update title to relate to the question you’re answering

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
  theme_minimal()
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
             aes(x = long, y = lat, size = retweet_count, alpha = retweet_count), 
             color = "red") +
  coord_map(xlim = c(-80, -65),
            ylim = c(36, 46)) +
  scale_alpha_continuous(range = c(.4, 1)) +
  labs(title = "#DuBoisChallenge Tweets in the North-East and Canada",
       size = "Number of Retweets",
       alpha = "Number of Retweets") +
  theme_minimal() +
  theme_void()
```

<img src="README_files/figure-gfm/question-one-plot-two-1.png" width="90%" />

``` r
ggplot() + 
  geom_sf(data = world_map, fill = "lightgray", color = "black") +
  geom_point(data = europe_tweets, 
             aes(x = long, y = lat, size = retweet_count, alpha = retweet_count), 
             color = "red") +
  scale_alpha_continuous(range = c(.4, 1)) +
  labs(title = "#DuBoisChallenge Tweets in Europe",
       size = "Number of Retweets",
       alpha = "Number of Retweets") +
  theme_void() +
  coord_sf(xlim = c(-20, 45), ylim = c(30, 73), expand = FALSE)
```

<img src="README_files/figure-gfm/question-one-plot-two-2.png" width="90%" />

### Discussion

(1-3 paragraphs) In the Discussion section, interpret the results of
your analysis. Identify any trends revealed (or not revealed) by the
plots. Speculate about why the data looks the way it does.

## Question 2 \<- Update title to relate to the question you’re answering

The second question we want to answer is:

*How does the time of day affect user construction of \#DuBois challenge
tweets and how the audience reacts to those tweets?*

First, we’ll compute the length of each tweet’s content, operationalized
by the number of characters, including spaces and emojis, in the tweet,
and assign it to a numeric variable `tweet_length`.

``` r
library(lubridate)
```

    ## 
    ## Attaching package: 'lubridate'

    ## The following objects are masked from 'package:base':
    ## 
    ##     date, intersect, setdiff, union

``` r
# tweets <- tweets %>%
#   filter(!is.na(datetime))%>%
#   mutate(time = (as.numeric(str_sub(datetime, start=12, end=13))),
#         date = ymd(str_sub(datetime, start = 1, end = 11)),
#         content = str_replace_all(as.character(content), "&amp", ""),
#         content = str_replace_all(as.character(content), "\\^@", ""),
#         tweet_length = nchar(as.character(content)))

#We intend to plot number of @'s in a tweet


# tweets %>%
#   arrange(desc(tweet_length)) %>%
#   select(content, tweet_length) %>%
#   head(5)
# 
# ggplot(tweets, aes(x=date, y = tweet_length)) + 
#   geom_col()
```

``` r
tweets %>%
  filter(verified == TRUE) %>%
  nrow()
```

    ## [1] 4

Thus, for a second visualization, we will use the variables `followers`,
`verified` and `like_count`. We will map `followers` to the x axis, and
`like_count` to the y axis, and then display the data for each twitter
user using geom\_point. Since some users may have tweeted multiple
times, we will calculate `average_like_count` per tweet for each user,
and use that number in place of the actual `like_count` variable. This
means for users who only tweeted once, we will use the like count for
that tweet, while for users who tweeted multiple times, we will use the
average like count across all of their challenge tweets. We chose to
focus on individual users since in the previous visualization we were
looking into cumulative tweets, on a non per-user basis. This way, for
this question, we will be able to gauge Twitter engagement in the Dubois
challenge on a per user basis, marking a contrast between our questions.
Then, we will use color mapping to map the verification status of the
tweet author to the colors of the points, such that viewers can easily
distinguish between the two groups (if the groups are not
distinguishable, another option is to facet by verification status - we
will make a choice about this after seeing how the data looks). Further,
we’ll display a geom\_smooth regression line for verified and unverified
users (if these lines become visually distracting, we will reconsider
using them). With these lines, we’ll be able to examine the interaction
between verification and followers and its effect on the number of likes
a tweet can generate.

``` r
# tweets <- tweets %>%
#   group_by(username) %>%
#   mutate(average_like_count = mean(like_count))
# 
# 
# tweets_time<- tweets%>%
#   mutate(time_range=case_when(time<=7~"Time 1", 
#                               time>7 & time <=15~ "Time 2", 
#                               time >15~"Time 3"))
# ggplot(tweets_time, aes(followers, average_like_count))+
#   geom_point()+
#   facet_grid(~time_range)+
#   scale_x_continuous(limits=c(0, 10000))+
#   scale_y_continuous(limits=c(0, 200))
#   
```

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
