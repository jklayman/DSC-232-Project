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
- Dropped irrelevant features
- Dropped empty reviews and emoji/ASCII reviews
- Dropped all languages except English due to API limitations on translate package
- Sent all characters to lower
- Converted selected columns to optimized integer types
- Converted playtime to hours
- Dropped discrepancies in forever_playtime and playtime_at_review (only using rows with forever_playtime > playtime at review)
    
## **Feature Exploration**

### Numerical Feature Exploration
- Number of Games Reviewed: 94164
  
- Number of Reviews per Game
  ![Top 10 Reviewed Games](https://github.com/user-attachments/assets/c24b6192-fbcd-4943-bbdb-a2b726df686a)
    
- Average Review Length: 321.23 Characters
  ![Review Length](https://github.com/user-attachments/assets/5c523eeb-0f21-4d0a-a409-a0db9561f1a5)

- Average Number of Games Owned: 171.70
  ![Games Owned](https://github.com/user-attachments/assets/5e0bdd5d-06c2-4742-9fd6-6872d0752895)

### Correlation Exploration:
- Playtime at Review vs Author's Vote

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

- Playtime After Review vs Author's Vote
  ![Playtime After Review vs. Review Vote](https://github.com/user-attachments/assets/efa0566f-f7b8-4232-9ae8-0b78cc777c97)

- Review Length vs Author's Vote
  ![Review Length vs. Review Vote](https://github.com/user-attachments/assets/21fd2432-2479-454e-8216-4e41cf4b30b3)

- Review Length vs Community Votes
  
  ![Review Length vs. Review Upvotes](https://github.com/user-attachments/assets/a58754d8-05b2-4c50-b258-51fb2a6cc630)

- Received for Free vs Voted Up by Author
  
  ![Received for Free vs Voted Up by Author](https://github.com/user-attachments/assets/2834816e-6c2c-465e-8a86-d92f76c5f70a)

- Early Access vs Voted Up by Author
  
  ![Early Access vs Voted Up by Author](https://github.com/user-attachments/assets/0d25699f-1b9d-4683-a146-4c84f2f7f16d)

- Overall Correlation Between Each Feature
  ![All Features Correlation Matrix](https://github.com/user-attachments/assets/d97fafad-d89f-4547-910e-cf38533df97b)

- Games Owned vs Reviews Written
  ![Games Owned vs. Reviews Written](https://github.com/user-attachments/assets/d41c6d10-df4f-4162-8d3a-47ec809bdb22)

## Postprocessing Thoughts
As we continued to compare features to look for correlations, we began to suspect they would be few and far between. Once we made the correlation matrix of all features, we knew for certain that there was virtually no correlation between any of the separate features (excluding playtime features). This meant that the models we planned to make would rely very heavily on the feature extraction and sentiment analysis conducted on the text of the reviews. 

### Applicable to both models:
- Number of vited up reviews: 30,904,853
- Number of voted down reviews: 6,256,826

## **Possible Models for Sentiment Analysis:**
### Word Embedding using word2vec + Logistic Regression
Learns from context and semantic relationships in text. Requires less preprocessing to preserve meaning.

Preprocessing Steps:
- Strip whitespace
- Lightly strip punctuation
- Light tokenization
- Keep stopwords
- Avoid stemming/lemmatization
  
### Term Frequency using TFIDF + Random Forest
Converts text into numerical features based on term importance. Requires heavy preprocessing to reduce noise and dimensionality.

Preprocessing Steps:
- Strip punctuation
- Strip special characters
- Full tokenization
- Remove stopwords
- Apply stemming/lemmatization
