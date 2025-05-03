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
  - Successfully reduced file size from 50GB to 20GB (60% reduction)

### Enhanced Processing Strategy
To handle the remaining data volume efficiently:

- Converting all data to Parquet format for optimized columnar storage
- Implementing targeted preprocessing to reduce dataset size further:
  - Removing reviews containing only emojis
  - Filtering out reviews with ASCII art
- Maximizing cluster resources by configuring for maximum memory and cores

## **Preprocessing** *done*

- Translate all foreign languages into English
- Send all characters to lower
- Create new features as necessary

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

To get basic summary statistics, we can use `describe()` to get basic summary statistics of the various numerical features. 

### Non-Numerical Feature Exploration

- Average review length (grouped by language) *done*
- Average number of games owned *done*
- Number of games reviewed *done*
  
### Correlation Exploration:
- Playtime at review vs voted_up
- (Playtime forever - playtime at review) vs voted_up
- Review length vs playtime vs voted_up
- Received for free vs voted_up
- Early access vs voted_up
- Number of game reviews vs voted_up

## **Possible Models for Sentiment Analysis:**
- Word embedding using word2vec with logistic regression (less heavy preprocessing due to preserving semantics)
- Term frequency using TFIDF and random forests (heavy preprocessing)
