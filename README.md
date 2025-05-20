# Model 1: TF-IDF

Our first model will be a TF-IDF model, which takes the frequency of the tokens within the reviews and creates numerical features that correspond to importance. We will be using a random forest classifier. 

### Pre-Processing
To turn the review text into usable data, we applied a regex tokenizer to remove special characters and split the bodies into individual words. Then we employed a basic stop word remover to discard words that don't naturally hold meaning. Lastly, we lemmatized the words to better group tokens with similar meaning. 

### Model Results
Our initial pass of the model gave us a training AUC of 0.7741 and a test AUC of 0.7747. The closeness was encouraging, but the low value was unideal since the proportion of positive reviews in our data was around 0.83. This meant that our classifier was underfitted, and performed worse than one that predicted a positive review every time. This could be due to a couple of factors:
- Our lemmatization could have been weak, so the tokens were improperly grouped. We thought about using POS tagging to to establish tokens' parts of speech, allowing for lemmatization to pay attention to sentence structure, but had difficulty running that operation in the notebook.
- Our model only used 50 trees, which could limit the fitting of the model
- The 5:1 imbalance between the two classes could be biasing our model to predict more positive than it should, we could over/undersample to get a better sample of negative reviews, or use SMOTE
- TF-IDF does not have context, limiting its ability to predict sentiment

### Iterations: Hyperparameter Tuning
We decided to work with our hyperparameters, iteratively training models with differing values. For our TF-IDF algorithm, we wanted to limit the max number of features. We also discovered Weighted Class TF-IDF, which is meant to be used on imbalanced datasets like ours. 

We also wanted to tune the random forest hyperparameters, specifically the number of trees and the max depth of the trees. 

We ran nested loops to train multiple models to compare their accuracies. The graph below details how our training and test accuracy differed between models. 

Our first tune was on the number of features taken in by our pipeline. We tested on 50, 1000, 2000, 5000, and 10000 features and found that 5000 features gave us the best AUC without diminishing returns. 
![image](https://github.com/user-attachments/assets/0d1e143d-002a-42ae-ab20-0b8853bc554b)

Next, we wanted to tune the number of trees in our random forest. Using the 5000 max features from earlier, we iterated through 50, 100, 150, 250, and 400 trees to look for differences. 


Lastly, we tuned the max depth of each tree using the above parameters. 

[[insert analysis of hyperparameter tuning here]]

### Next Models:
Due to the context and sentiment lost in our TF-IDF model, we decided that a model that focuses on sentiment analysis would be ideal to analyze review text and determine whether the author liked the game they reviewed. We decided on a pipeline of a Word2Vec model and a logistic regression classifier. 
