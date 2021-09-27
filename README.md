Project title
================
by Team name

    ## Warning in system("timedatectl", intern = TRUE): running command 'timedatectl'
    ## had status 1

    ## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.1 ──

    ## ✓ ggplot2 3.3.5     ✓ purrr   0.3.4
    ## ✓ tibble  3.1.4     ✓ dplyr   1.0.7
    ## ✓ tidyr   1.1.3     ✓ stringr 1.4.0
    ## ✓ readr   2.0.1     ✓ forcats 0.5.1

    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

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

``` r
tweets %>%
  group_by(location) %>%
  mutate(location_tweet_count = n())
```

    ## # A tibble: 445 × 14
    ## # Groups:   location [102]
    ##    datetime content retweet_count like_count quote_count text  username location
    ##    <fct>    <fct>           <int>      <int>       <int> <fct> <fct>    <fct>   
    ##  1 2021-05… "@sqls…             0          1           0 "<a … AlDatav… New York
    ##  2 2021-05… "#DuBo…             0          0           0 "<a … AlDatav… New York
    ##  3 2021-05… "#DuBo…             0          0           0 "<a … AlDatav… New York
    ##  4 2021-05… "Was d…             0          4           0 "<a … AlDatav… New York
    ##  5 2021-04… "@Clin…             0         11           0 "<a … AlDatav… New York
    ##  6 2021-04… "@john…             0          0           0 "<a … etmckin… Nashvil…
    ##  7 2021-04… "For #…             3         58           0 "<a … AdamMic… Madison…
    ##  8 2021-04… "@zanm…             0          0           0 "<a … AlDatav… New York
    ##  9 2021-04… "Just …             1          1           0 "<a … lisakth… Saint L…
    ## 10 2021-04… "Honor…             1          6           0 "<a … AlDatav… New York
    ## # … with 435 more rows, and 6 more variables: followers <int>, url <fct>,
    ## #   verified <lgl>, lat <dbl>, long <dbl>, location_tweet_count <int>

``` r
tweets %>%
  count(location) %>%
  arrange(desc(n))
```

    ##                         location   n
    ## 1                       New York 125
    ## 2                           <NA>  59
    ## 3                  Nashville, TN  33
    ## 4                New Jersey, USA  30
    ## 5                     Merced, CA  15
    ## 6                Vienna, Austria  11
    ## 7           Kevin.Elder@GCSU.edu  10
    ## 8                    Madison, WI  10
    ## 9                Minneapolis, MN  10
    ## 10  iPhone: 34.704040,-86.722909  10
    ## 11                  New York, NY   9
    ## 12              Philadelphia, PA   5
    ## 13               California, USA   4
    ## 14                    Boston, MA   3
    ## 15                     Edinburgh   3
    ## 16                        London   3
    ## 17                United Kingdom   3
    ## 18                       he/they   3
    ## 19                   Amherst, MA   2
    ## 20            At the home office   2
    ## 21                 Baltimore, MD   2
    ## 22          Basingstoke, England   2
    ## 23                   Buffalo, NY   2
    ## 24                    Dallas, TX   2
    ## 25                      Delaware   2
    ## 26                Derbyshire, UK   2
    ## 27              Forde-Obama Hall   2
    ## 28                Nairobi, Kenya   2
    ## 29                 New York, USA   2
    ## 30                          Roma   2
    ## 31               Uppsala, Sweden   2
    ## 32                Washington, DC   2
    ## 33                     Worldwide   2
    ## 34               Albuquerque, NM   1
    ## 35         Arlington Heights, IL   1
    ## 36                    Arvada, CO   1
    ## 37  Belgrade, Republic of Serbia   1
    ## 38              Calgary, Alberta   1
    ## 39                    California   1
    ## 40                 Cambridge, MA   1
    ## 41                      Canberra   1
    ## 42                     Cape Town   1
    ## 43            Caracas, Venezuela   1
    ## 44                  Carrboro, NC   1
    ## 45                  Catawba land   1
    ## 46               Chapel Hill, NC   1
    ## 47           Charlottesville, VA   1
    ## 48                   Chicago, IL   1
    ## 49                      Colorado   1
    ## 50                  Columbus, OH   1
    ## 51                    Costa Rica   1
    ## 52                   Deutschland   1
    ## 53      Distrito Federal, México   1
    ## 54                    Eugene, OR   1
    ## 55                   France & UK   1
    ## 56                       Germany   1
    ## 57                   God's earth   1
    ## 58                 Hertfordshire   1
    ## 59                Huntington, NY   1
    ## 60                     Hurst, TX   1
    ## 61                         India   1
    ## 62          Jaboatão, PE, Brasil   1
    ## 63                    Kazakhstan   1
    ## 64             Kingston, Ontario   1
    ## 65       Köln/Osnabrück, Germany   1
    ## 66          Lafayette, Louisiana   1
    ## 67                Lil’ Rudyshire   1
    ## 68              Lisboa, Portugal   1
    ## 69               London, England   1
    ## 70                MIT, Cambridge   1
    ## 71          Melbourne, Australia   1
    ## 72                     Miami, FL   1
    ## 73                    New Yorker   1
    ## 74                Northridge, CA   1
    ## 75   OAK / NYC / ATL / The World   1
    ## 76               Ottawa, Ontario   1
    ## 77                 Paris, France   1
    ## 78                  Philadelphia   1
    ## 79                  Portland, ME   1
    ## 80                  Porto Alegre   1
    ## 81             Querétaro, México   1
    ## 82                            SP   1
    ## 83         Saint Louis, Missouri   1
    ## 84                     San Diego   1
    ## 85                       Seattle   1
    ## 86                   Seattle, WA   1
    ## 87                    Shropshire   1
    ## 88                  Stanwood, WA   1
    ## 89        Stuttgart, Deutschland   1
    ## 90  The City College of New York   1
    ## 91             Toronto || Ottawa   1
    ## 92              Toronto, Ontario   1
    ## 93                            Tx   1
    ## 94                            UK   1
    ## 95            USA, Brazil, Spain   1
    ## 96                 United States   1
    ## 97                         World   1
    ## 98                 York, England   1
    ## 99      andrew.tran@washpost.com   1
    ## 100           down in dey wid em   1
    ## 101                     İstanbul   1
    ## 102                         北京   1

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

## Question 2 \<- Update title to relate to the question you’re answering

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

``` r
tweets <- tweets %>%
  mutate(tweet_length = nchar(as.character(content)))

ggplot(tweets, aes(x = tweet_length)) + 
  facet_wrap(~verified) + 
  geom_density()
```

    ## Warning: Removed 1 rows containing non-finite values (stat_density).

![](README_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

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
tweets %>%
  group_by(username) %>%
  mutate(average_like_count = mean(like_count))
```

    ## # A tibble: 445 × 15
    ## # Groups:   username [167]
    ##    datetime content retweet_count like_count quote_count text  username location
    ##    <fct>    <fct>           <int>      <int>       <int> <fct> <fct>    <fct>   
    ##  1 2021-05… "@sqls…             0          1           0 "<a … AlDatav… New York
    ##  2 2021-05… "#DuBo…             0          0           0 "<a … AlDatav… New York
    ##  3 2021-05… "#DuBo…             0          0           0 "<a … AlDatav… New York
    ##  4 2021-05… "Was d…             0          4           0 "<a … AlDatav… New York
    ##  5 2021-04… "@Clin…             0         11           0 "<a … AlDatav… New York
    ##  6 2021-04… "@john…             0          0           0 "<a … etmckin… Nashvil…
    ##  7 2021-04… "For #…             3         58           0 "<a … AdamMic… Madison…
    ##  8 2021-04… "@zanm…             0          0           0 "<a … AlDatav… New York
    ##  9 2021-04… "Just …             1          1           0 "<a … lisakth… Saint L…
    ## 10 2021-04… "Honor…             1          6           0 "<a … AlDatav… New York
    ## # … with 435 more rows, and 7 more variables: followers <int>, url <fct>,
    ## #   verified <lgl>, lat <dbl>, long <dbl>, tweet_length <int>,
    ## #   average_like_count <dbl>

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
