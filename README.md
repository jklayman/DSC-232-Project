# Model 1: TF-IDF

Our first model will be a TF-IDF model, which takes the frequency of the tokens within the reviews and creates numerical features that correspond to importance. We will be using a random forest classifier. 

### Pre-Processing
To turn the review text into usable data, we applied a regex tokenizer to remove special characters and split the bodies into individual words. Then we employed a basic stop word remover to discard words that don't naturally hold meaning. Lastly, we lemmatized the words to better group tokens with similar meaning. 

### Model Results
Our initial pass of the model gave us a training error of 0.7741 and a test error of 0.7747. While the closeness of the accuracies indicated our model wasn't over or underfitted, this was unideal since the proportion of positive reviews in our data was around 0.83, and thus our classifier performed worse than one that predicted a positive review every time. This could be due to a couple of factors:
- Our lemmatization could have been weak, so the tokens were improperly grouped. We thought about using POS tagging to to establish tokens' parts of speech, allowing for lemmatization to pay attention to sentence structure, but had difficulty running that operation in the notebook.
- Our model only used 50 trees, which could limit the fitting of the model
- The 5:1 imbalance between the two classes could be biasing our model to predict more positive than it should, we could over/undersample to get a better sample of negative reviews, or use SMOTE
- TF-IDF does not have context, limiting its ability to predict sentiment. This is ultimately why we decided to create a second model on Word2Vec. 

Improvement Strategies: POS tagging to increase lemmatization, hyperparameter tuning (number of trees, max depth of trees)

# Model 2: Word2Vec
