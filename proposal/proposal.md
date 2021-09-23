Project proposal
================
ctrl\_alt\_elite

``` r
library(tidyverse)
```

``` r
tweets <- readr::read_csv(
  "https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2021/2021-06-15/tweets.csv"
)
```

## Dataset

Our dataset was collected from [the tidytuesday GitHub Repository of
DuBoisChallenge
tweets](https://github.com/rfordatascience/tidytuesday/blob/master/data/2021/2021-06-15/readme.md).

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

For our dataset (Tweets), we have chosen to examine the 2021 WEB Du Bois
& Juneteenth Twitter data where each observation is a tweet that uses
the DuBoisChallenge hastag, and the accompanying metadata such as likes,
retweets, and location. The dataset includes 13 different variables and
a total of 445 observations. The data was collected via scraping from
Twitter based on the presence of the WEB Du Bois challenge hashtag.

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
with history and data visualization in a social media setting.

We also chose the dataset because it is comprehensive to work with.
There are a variety of different types of variables that can be used for
analysis - logical, double, and character - and the dataset is
well-cleaned. Since the data set is extensive, we know we will be able
to answer a variety of non-overlapping questions that are pertinent to
the world we live in today.

## Questions and Analysis Plan

### Question One

The first question we want to answer is:

*How does general interest of the \#DuBois challenge vary
geographically?*

We will first create a bar plot that maps `location` on the y-axis and
the number of tweets from that location on the x-axis. This will require
creating a new variable `location_tweet_count`. Since a user in one
location can tweet multiple times, we will only consider one tweet per
user to look at geographical engagement of users on this plot. We will
also need to mutate a new variable `location_state`, based off of
`location`, where location names are modified to either be a state in
the United States, or categorized as an international country. If there
happen to be many international locations in the data set from different
counties, we will consider having two bar plots - one for U.S. states,
and one for other countries, so that the visualization does not become
too overcrowded. We will also need to look through the location data to
remove any observations which do not represent actual names of
geographical locations. By looking at this data on a bar plot, we will
be able to compare engagement in the TidyTuesday challenge
geographically, to see which locations produced the most number of
tweets. The advantage of using a bar plot is that each location is very
distinguishable from the next, and the data can be ordered such that
it’s obvious visually which locations have many tweets, versus very
few tweets. This will also inform our next visualization, as we will
know which areas of the global map to emphasize.

In order to next explore this inquiry, we will use the `lat`, `long`,
and `retweet_count`variables. To display the geographical distribution
of the data, we will use an external map of the world, on which we will
layer a scatterplot of the location of each tweet. We will use the
latitude and longitude variables to accomplish this, along with the
world map coordinates that come from the map\_data() function in
tidyverse. Then, we will display the count of retweets for each tweet by
mapping the `retweet_count` variable to color, size, or another
scatterplot aesthetic, such that all of the information is very clearly
displayed on the world map. Since it will be important to consider the
population differences of the locations on the map when analyzing the
occurrence of tweets, we will either include this information in our
narrative, or color the world map by population, so that the information
is readily available to viewers. Thus, this visualization will most
likely employ color mapping.

### Question Two

The second question we want to answer is:

*How do the number of likes and followers of a \#DuBois challenge
Twitter user vary based on user verification status?*

ADD FIRST PLOT WE WILL DO

We will use the variables `followers`, `verified` and `like_count`. We
will map `followers` to the x axis, and `like_count` to the y axis, and
then display each point using geom\_point. Since some users may have
tweeted multiple times, we will take calculate `average_like_count` per
tweet for each user, and use that number. This means for users who only
tweeted once, we will use the like count for that tweet, while for users
who tweeted multiple times, we will use the average like count across
all of their challenge tweets. Then, we will use color mapping to map
the verification status of the tweet author, such that viewers can
easily distinguish the two groups. If the colors look messy or
indistinguishable, we will consider faceting by verification status,
instead, or can also employ geom\_smooth to point out noticeable trend
lines, and how they differ between verified and unverified users.
