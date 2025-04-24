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

## **Possible Models for Sentiment Analysis:**
- Word embedding using word2vec with logistic regression (less heavy preprocessing due to preserving semantics)
- Term frequency using TFIDF and random forests (heavy preprocessing)
