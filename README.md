Our Dataset: https://www.kaggle.com/datasets/kieranpoc/steam-reviews/data

# DATA EXPLORATION

## **Preprocessing**

- Translate all foreign languages into English
- Convert all timestamps from Unix time into datetime
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

- Average review length (grouped by language)
- Proportion of languages
- Average number of games owned
- Number of games reviewed
- Number of reviews per review author
  
### Correlation Exploration:
- Playtime at review vs voted_up
- (Playtime forever - playtime at review) vs voted_up
- Review length vs playtime vs voted_up
- Received for free vs voted_up
- Early access vs voted_up
- Number of game reviews vs voted_up

### File Type Exploration

We explored file types .zip vs .csv.gz for Spark workflow. We chose .csv.gz format because it offers native, streamable compatibility with Spark—unlike .zip files which require manual unzipping first. The .gz format allows us to upload once and use directly, uses less cluster disk space since no extra uncompressed copy is needed, and provides faster loading times with a 1-step direct read process compared to the 2-step process required for .zip files.

### Large File Transfer Process

For transferring our 50gb file, we could not use direct upload due to size limitations. We instead used Globus Connect Personal which allowed us to create an endpoint on a team member's local machine and another was created in the SDSC HPC OPS-testing expanse endpoint—then we navigated to the scratch folder to start the 50gb upload. This approach provided reliable transfer with resume capability and verification.

## **Possible Models for Sentiment Analysis:**
- Word embedding using word2vec with logistic regression (less heavy preprocessing due to preserving semantics)
- Term frequency using TFIDF and random forests (heavy preprocessing)
