Project proposal
================
ctrl\_alt\_elite

``` r
library(tidyverse)
```

``` r
tweets <- read.csv("tweets_proposal.csv")
```

## Dataset

Our dataset was collected from [the tidytuesday GitHub Repository of
DuBois Challenge
tweets](https://github.com/rfordatascience/tidytuesday/blob/master/data/2021/2021-06-15/readme.md).

``` r
glimpse(tweets)
```

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

We chose this dataset because we appreciate the importance of
celebrating W.E.B Du Bois’s legacy in data visualization. The \#DuBois
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

We also chose this data set due to its ease of use; there are a variety
of different types of variables that can be used for analysis - logical,
double, and character - and the data is well-cleaned. Since the data set
is extensive, we know we will be able to answer a variety of
non-overlapping questions that are pertinent to the world we live in
today.

## Questions and Analysis Plan

### Question One

The first question we want to answer is:

*How does general interest of the \#DuBois challenge vary
geographically?*

We will first create a bar plot that maps `location` on the y-axis and
the number of tweets from that location on the x-axis. This will require
creating a new variable `location_tweet_count`. As a user in one
location can tweet multiple times, we will consider all their tweets
cumulatively to look at total geographical engagement of users on this
plot. This is due to the fact that we consider multiple tweets by a user
to be a sign of engagement, and thus want to understand engagement on
the whole, versus on a per user basis. We will also need to mutate a new
variable `location_state`, based off of `location`, where location names
are modified to be a state in the United States, or categorized as an
international country. If there happen to be many international
locations in the data set from different counties, we will consider
having two bar plots - one for U.S. states, and one for other countries,
so that the visualization does not become too overcrowded. We could also
only display the top 10 or so locations on the bar plot, if need be for
simplification purposes (we do not want too much overwhelming info). We
will also need to look through the location data to remove any
observations which do not represent actual names of geographical
locations. By looking at this data and organizing it on a bar plot, we
will be able to compare engagement in the TidyTuesday challenge
geographically, to see which locations produced the most number of
tweets. The advantage of using a bar plot is that each location is very
distinguishable from the next, and the data can be ordered such that
it’s obvious visually which locations have many tweets, versus very
few tweets coming from them. This will also inform our next
visualization, as we will know which areas of the global map to
emphasize, if need be.

To next explore this inquiry, we will use the `lat`, `long`, and
`retweet_count` variables. To display the geographical distribution of
the data, we will use an external map of the world, on which we will
layer a scatter plot of the location of each tweet (where it was tweeted
from). We will use the latitude and longitude variables to accomplish
this, along with the world map coordinates that come from the
map\_data() function in tidyverse. Then, we will display the count of
retweets for each tweet by mapping the `retweet_count` variable to
color, size, or another scatterplot aesthetic, such that all of the
information is very clearly displayed on the world map. Since it will be
important to consider the population differences of the locations on the
map when analyzing the occurrence of tweets, we will create a separate
heatmap of the world (we will use external data of the 2021 population
of the world’s countries), to be displayed next to this map, to help
readers take population discrepancies into account. Thus, this
visualization will most likely employ color mapping.

We employ a comprehensive analysis of geographical interest, as the
first visualization looks at the location tweeted from, while the second
visualization examines the location of the actual user, presumably based
on their profile.

### Question Two

The second question we want to answer is:

*How does Twitter verification status affect user construction of
\#DuBois challenge tweets and how the audience reacts to those tweets?*

For the first visualization, we will use `verified` and `content` to
examine the length of tweets of verified and unverified users. First,
we’ll compute the length of each tweet’s content, operationalized by
the number of characters, including spaces and emojis, in the tweet, and
assign it to a numeric variable `tweet_length`. Then, we’ll create a
ridge plot to succinctly compare the densities of tweet lengths between
verified and unverified users. After seeing how verified users might
write their tweets differently, we would then like to look into how
users react to their tweets.

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

We believe that by taking the various aforementioned variables into
account, our data analysis and visualizations will be comprehensive and
take full advantage of the data set.
