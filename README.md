Our Dataset: https://www.kaggle.com/datasets/kieranpoc/steam-reviews/data

# DATA EXPLORATION

### File Type Exploration

We explored file types .zip vs .csv.gz for Spark workflow. We chose .csv.gz format because it offers native, streamable compatibility with Spark—unlike .zip files which require manual unzipping first. The .gz format allows us to upload once and use directly, uses less cluster disk space since no extra uncompressed copy is needed, and provides faster loading times with a 1-step direct read process compared to the 2-step process required for .zip files.

### Large File Transfer Process

For transferring our 50gb file, we could not use direct upload due to size limitations. We instead used Globus Connect Personal which allowed us to create an endpoint on a team member's local machine and another was created in the SDSC HPC OPS-testing expanse endpoint—then we navigated to the scratch folder to start the 50gb upload. This approach provided reliable transfer with resume capability and verification.

### Large Dataset Management

Our dataset handling presented significant performance challenges:

- Size Challenge: The original CSV file (50GB) required maxing out cluster configurations
- Performance Bottleneck: Processing times were extremely slow due to the massive data volume
- Conversion Solution: Implemented CSV to Parquet conversion to address performance issues
  - Parquet format delivers 25-40x faster processing speeds

### Enhanced Processing Strategy
To handle the remaining data volume efficiently:

- Converting all data to Parquet format for optimized columnar storage
- Implementing targeted preprocessing to reduce dataset size further:
- Maximizing cluster resources by configuring for maximum memory and cores

## Dataset Information
- 113m rows, 24 columns
- Columns:
  - recommendationid: The unique ID of the recommendation
  - appid: The unique app ID for every app on Steam
  - game: The game name corresponding to the appid
  - author_steamid: The user's SteamID
  - author_num_games_owned: Number of games owned by the user
  - author_num_reviews: Number of reviews written by the user
  - author_playtime_forever: Lifetime playtime tracked in this app (minutes)
  - author_playtime_last_two_weeks: Playtime tracked in the past two weeks for this app
  - author_playtime_at_review: Playtime when the review was written
  - author_last_played: Time for when the user last played
  - language: Language the user indicated when authoring the review
  - review: Text of written review
  - timestamp_created: Date the review was created (unix timestamp)
  - timestamp_updated: Date the review was last updated (unix timestamp)
  - voted_up: True means it was a positive recommendation
  - votes_up: The number of users that found this review helpful
  - votes_funny: The number of users that found this review funny
  - weighted_vote_score: Helpfulness score
  - comment_count: Number of comments posted on this review
  - steam_purchase: True if the user purchased the game on Steam
  - received_for_free: True if the user checked a box saying they got the app for free
  - written_during_early_access: True if the user posted this review while the game was in Early Access
  - hidden_in_steam_china: True if invisible in China
  - steam_china_location: Location in China (mostly null values)
- Very few missing data points except for steam_china_location

## **Preprocessing**
- Dropped empty reviews and emoji/ASCII reviews
- Dropped irrelevant features
- Dropped all languages except English due to API limitations on translate package
- Sent all characters to lower
- Dropped discrepancies in forever_playtime and playtime_at_review (only using rows with forever_playtime > playtime at review)
- Converted playtime to hours

### Word Embedding
- Learns from context, less preprocessing needed
  - Strip whitespace
  - Lightly strip punctuation
  - Light tokenization
  - Keep stopwords
  - Avoid stemming/lemmatization
    
### Term Frequency
- Heavy preprocessing needed
  - Strip punctuation
  - Strip special characters
  - Full tokenization
  - Remove stopwords
  - Stemming/lemmatization
    
## **Feature Exploration**

### Numerical Feature Exploration

- Average review length (grouped by language) 321.19 characters
- Average number of games owned: 171.65
- Number of games reviewed: 94159

- number of reviews per game
  ![Top 10 Reviewed Games](https://github.com/user-attachments/assets/c24b6192-fbcd-4943-bbdb-a2b726df686a)
    
### Correlation Exploration:
- Overall correlation between each feature
  ![All Features Correlation Matrix](https://github.com/user-attachments/assets/d97fafad-d89f-4547-910e-cf38533df97b)

- Playtime at review vs voted_up (author voting on game)

|                game|at_review_hours|voted_up|
|--------------------|---------------|--------|
|     Team Fortress 2|          79609|    true|
|     Team Fortress 2|          66027|    true|
|    Counter-Strike 2|          64963|    true|
|Half-Life Deathma...|          61516|    true|
|    Counter-Strike 2|          60231|    true|
|Sid Meier's Civil...|          59038|    true|
|       Day of Defeat|          59005|   false|
|         Garry's Mod|          58742|    true|
|Sid Meier's Civil...|          57197|    true|
|Sid Meier's Civil...|          53351|    true|

- Number of games owned vs number of reviews written
  ![Games Owned vs. Reviews Written](https://github.com/user-attachments/assets/d41c6d10-df4f-4162-8d3a-47ec809bdb22)

- (Playtime forever - playtime at review) vs voted_up (author voting on game)
  ![Playtime After Reviewing vs. Review Vote](https://github.com/user-attachments/assets/78b56bc0-29e3-4a52-b946-bf2509e0a493)
  
- Review length vs voted_up (author voting on game)
  ![Review Length vs. Review Vote](https://github.com/user-attachments/assets/21fd2432-2479-454e-8216-4e41cf4b30b3)

- Review length vs playtime vs votes_up (users voting on a review)
  ![Review Length vs. Review Upvotes](https://github.com/user-attachments/assets/b6298567-b4d1-4fe7-aaed-41126ee0cddc)

## **Possible Models for Sentiment Analysis:**
- Word embedding using word2vec with logistic regression (less heavy preprocessing due to preserving semantics)
- Term frequency using TFIDF and random forests (heavy preprocessing)
