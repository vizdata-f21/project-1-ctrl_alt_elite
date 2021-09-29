# 2021 WEB Du Bois & Juneteenth Twitter data

Each observation is a tweet from 2021 that used the #DuBoisChallenge tag and the
variables are the accompanying metadata such as likes, retweets, and location. 
The dataset includes 13 different variables and a total of `r nrow(tweets)` 
observations. The data was collected via web scraping from Twitter based on the 
presence of the WEB Du Bois challenge hashtag. 


## "tweets.csv"

|variable         |description                           |type |
|:----------------|:-------------------------------------|-----|
|datetime         |Date and time of tweet                |dbl  |
|content          |Text for tweet                        |chr  |
|retweet_count    |Number of tweet retweets              |dbl  |
|like_count       |Number of tweet likes                 |dbl  |
|quote_count      |Number of tweet quotes                |dbl  |
|text             |Tweet origin (iPhone/Android/Web App) |chr  |
|username         |Username of poster's Twitter account  |chr  |
|location         |Self-reported post location           |chr  |
|followers        |Number of followers (of user)         |dbl  |
|url              |Web link to tweet                     |chr  |
|verified         |Is user verified?                     |log  |
|lat              |Latitude coordinate of user           |dbl  |
|long             |longitude coordinate of user          |dbl  |
