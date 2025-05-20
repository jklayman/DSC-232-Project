# Model 1: TF-IDF + Random Forest

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

We see above that the model is generalizing well and not overfitting because our AUC for training and testing are both high.

Next, we wanted to tune the number of trees in our random forest. Using the 5000 max features from earlier, we iterated through 50, 100, 150, 250, and 400 trees to look for differences. We employed an additional metric, specifically model accuracy, as a way to define our label prediction score.

![image](https://github.com/user-attachments/assets/95d29a5b-c4d2-4f85-a19d-1117f719e7e1)

We see above that the model is generalizing even better with additonal trees and is not overfitting because our AUC and accuracy for training and testing are both high.

Lastly, we attempted to tune the max depth of each tree. Unfortunately, even with a heavy downsample of 20% of the original data, our machine ran out of space trying to make its way through 400 trees of max depth 9. However, we were able to make trees with 1, 3, 5, and 7 max depth, with AUC increasing with tree depth. This is expected, since deeper trees logically leads to a more precise model. 

![image](https://github.com/user-attachments/assets/d65fc79a-1ae8-4410-a475-d25b540132bd)

At the conclusion of our parameter tuning for the TF-IDF/Random Forest, we ended at 5000 number of features, 400 trees, and 7 tree depth. We theorize additonal computation in the number of trees parameter can yield a more peformant model, but would require either more computing power than we have access to or an unacceptable loss of data. 

Our ground truths:

![image](https://github.com/user-attachments/assets/548f22a0-6528-4d7d-81f8-dc5c978b3bbc)

![image](https://github.com/user-attachments/assets/c8f572c7-8f54-41cc-b3dd-b4ba00202318)


### Next Models:
Due to the context and sentiment lost in our TF-IDF model, we decided that a model that focuses on sentiment analysis would be ideal to analyze review text and determine whether the author liked the game they reviewed. We decided on a pipeline of a Word2Vec model and a logistic regression classifier. 
