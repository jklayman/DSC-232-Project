# DSC 232R Group Project

## Introduction to Our Project
The Steam gaming platform hosts one of the largest online communities for video game players, where user-generated reviews provide an invaluable stream of feedback, sentiment, and behavioral insight. We chose the "100 Million+ Steam Reviews" dataset because of its vast scale, rich metadata, and real-world application in understanding how players engage with games. Unlike many datasets that focus solely on text or numeric features, this one offers a unique combination of both, allowing for a multi-dimensional analysis of sentiment and behavior.

What makes this dataset especially compelling is its relevance not only to game developers but also to researchers in natural language processing, behavioral analytics, and recommendation systems. A well-performing predictive model built on this data could help platforms like Steam automatically detect spam or troll reviews, recommend games more intelligently, or highlight trending titles based on authentic positive feedback. Additionally, identifying which variables most influence sentiment, such as playtime, purchase behavior, or review formatting, provides actionable insight into user engagement and content quality.

The broader impact of this project extends beyong gaming. Techniques developed here can be applied to other domains where user feedback shapes product visibility and platform trust, such as Amazon, Yelp, or social media. Building a reliable, interpretable sentiment classifier supports the creation of safer, more personalized, and data-informed digital environments.

## Methods Section
### Initial Acquisition
The dataset was uploaded in .csv.gz fromat to SDSC Expanse using Globus. A local machine was configured as a Globas endpoint, and the 50GB dataset was transferred to the Expanse scratch directory. After transfer, the data was converted to Parquet format. Spark was configured with maximum available resources for processing. The dataset contains approximately 113 million rows and 24 columns, including game identifiers, user behavior metrics, playtime data, and free-form review text.

### Pre-Processing
The dataset was filtered to include only English-language reviews. Reviews that were empty or composed entirely of symbols or ASCII characters were removed. All review text was converted to lowercase, and excess whitespace and selected punctuation were cleaned. Playtime columns were converted from minutes to hours. Rows with inconsistencies between lifetime playtime and playtime at review were dropped. Boolean columns were cast to integers, and columns deemed irrelevant or mostly empty were removed from the dataset.

### Feature Exploration and Visualization
The pre-processed dataset included 94,164 unique games with user reviews. Some games had significantly higher review counts, with titles such as Counter-Strike 2 receiving over 1 million reviews. The average review length was approximately 300 characters, with some extending beyond 5,000 characters. The distribution of review lengths was heavily right-skewed. A similar pattern was observed in the number of games owned by users, with an average of around 170 games and some users owning more than 5,000.

Feature correlations were explored using variables such as playtime at review, playtime after review, review length, whether the game was received for free, and whether it was played in early access. These were compared against the binary recommendation outcome. A correlation matrix was constructed and examined for notable relationships among these variables. Based on this analysis, features with minimal or no correlation to the recommendation variable were identified and subsequently removed. The remaining features used in modeling focused on the review text.

### Model 1: TF-IDF/Random Forest
The review text was tokenized, and common stopwords and special characters were removed. A simple lemmatizer was applied to group words with similar roots. The data was then split into training and test sets. A binary classification pipeline ws constructed using TF-IDF for feature extraction and a random forest classifier. The model was trained on the training set, and evaluation metrics, including AUC, were computed on both training and test data. Hyperparameter tuning was performed by iterating over the maximum number of TF-IDF features, the number of trees in the forest, and the maximum tree depth.

# Need to finish
### Model 2: Word2Vec Embeddings/Logistic Regression
The global preprocessing steps were applied along with additional preprocessing specific to this model. Review text was tokenized in preparation for embedding. Class balancing was performed using random undersampling to create a balanced training set of positive and negative reviews. A Word2Vec model was used to generate word embeddings, which were then used as input into a logisitic regression classifier. Hyperparameter tuning was conducted on a smaller subset of the data to improve efficiency. The tuning process focused on vectorSize, minCount, and windowSize, and the final model will use the selected parameters: vectorSize = [input], minCount = [input], and windowSize = [input]. The finalized model is applied to the full dataset for training and evaluation.

What Tristan wrote: After the initial TF-IDF/random forest model, we chose to employ Word2Vev word embeddings/logistic regression in an effort to increase the accuracy. To begin, the global preprocessing detailed above was used in tandem with seperate model preprocessing shown below. We used seperate preprocessing for the word embeddings driven model because we wanted to preserve as much semantic meaning as possible. Several cleaning and transformation steps were applied to get the dataset ready for modeling: Tokenization. We then employed random undersampling for class balancing so that our resultant model is less biased. Ultimately, our model used X million positive and X million negative reviews for training. In the training phase, we used Word2Vec embeddings alongside a Logistic Regression classifier. We progressed with hyperparameter tuning on an even more downsampled dataset at 3 million positive and negative reviews for quicker tunes. Word2Vec has a plethora of tuning possabilties, but we focused on the following high priority parameters: vectorSize, minCount, windowSize. After hyperparameter tuning, we chose [input final parameters]. We then ran the final model on the 14M row dataset.

## Results Section
This will include the results from the methods listed above (C). You will have figures here about your results as well. No exploration of results is done here. This is mainly just a summary of your results. The sub-sections will be the same as the sections in your methods section.

# Need to finish

### Feature Exploration and Visualization
The preprocessed dataset contained 94,164 unique games with user reviews. Some games were reviewed far more frequently than others, with a noticeable concentration of reviews on top titles. Counter-Strike 2, for instance, accumulated over 1 million reviews, making it one of the more frequently reviewed games in the dataset (Fig. 1).
<img src="https://github.com/user-attachments/assets/6c7f6d6b-d54c-47cf-9002-26de91e37902" width="1000">
_Fig. 1: The number of reviews on the top 10 most reviewed games._

The average review length was approximately 300 characters. However, there was a substantial variation, with some users writing reviews that exceeded 5,000 characters. The distribution of review lengths was heavily right-skewed, indicating that most reviews were relatively short while a small proportion were much longer (Fig. 2).
<img src="https://github.com/user-attachments/assets/9e1612cb-978a-4c05-816d-1e5050e5a2a3" width="1000">
_Fig. 2: The distribution of the lengths of the reviews._ 

A similar right-skewed distribution was observed in the number of games owned by reviewers. The average number of owned games was approximately 170, but some users reported owning over 5,000 games (Fig. 3)
<img src="https://github.com/user-attachments/assets/61f2e4a4-ea5a-4c6d-ab4c-457f6a750777" width="1000">
_Fig. 3: The distribution of the amount of games owned by review authors._ 

We then explored potential relationships between review recommendations and other metadata. These included playtime after submitting a review (Fig. 4), the length of the review (Fig. 5), whether the game was received for free (Fig. 6), and whether it was played in early access (Fig. 7). Each of these comparisons was visualized with respect to the binary recommendation value.
<img src="https://github.com/user-attachments/assets/0a826549-5202-441a-91a5-52cbd470fb90" width="1000">
_Fig. 4: The relationship between the playtime after review and the author's review._ 

<img src="https://github.com/user-attachments/assets/314e957e-a146-47e8-b338-d330762b0a93" width="1000">

_Fig. 5: The relationship between the review length and the author's review._ 

<img src="https://github.com/user-attachments/assets/04655370-dda2-4e60-965d-3c040ce67a3e" width="1000">

_Fig. 6: The relationship between whether or not the game was received for free and the author's review._ 

<img src="https://github.com/user-attachments/assets/d1ff4e6b-48ad-4712-bf63-b065b392242a" width="1000">

_Fig. 7: The relationship between whether the game was played in early access and the author's review._ 

To supplement these visual comparisons, a correlation matrix was generated to examine relationships among all non-linguistic features. The matrix displayed low correlation scores across most variables, confirming that the features were largely uncorrelated with each other and with the recommendation outcome (Fig. 8).
<img src="https://github.com/user-attachments/assets/d8b4731e-72a0-4af1-ad6c-8c7bab7c565c" width="1000">
_Fig. 8: The correlation matrix between all the non-linguistic features._ 

### Model 1: TF-IDF/Random Forest
Hyperparameter tuning was conducted to improve the performance of the TF-IDF and random forest classification pipeline. The tuning process began with adjusting the number of features used by the HashingTF component. Several values were tested and compared to identify trends in AUC performance across diferent feature dimensions (Fig. 9).
<img src="https://github.com/user-attachments/assets/5edf09dc-c1f6-4eb6-810b-4a26a5ab09be" width="1000">
_Fig. 9: Tuning the number of features in HashingTF_

Next, the number of trees in the random forest classifer was varied. A range of tree counts was evaluated to determine the relationship between ensemble size and prediction accuracy, with corresponding AUC scores plotted for comparison (Fig. 10).
<img src="https://github.com/user-attachments/assets/b2a6bfca-9012-430b-8f8f-e0d838dd4f1f" width="1000">
_Fig. 10: Tuning the number of trees in the random forest_

Lastly, maximum tree depth was tuned to explore its effect on model generalization. Multiple depth values were tested, and their influence on AUC metrics was recorded to observe any overfitting or underfitting behavior at extreme values (Fig. 11).
<img src="https://github.com/user-attachments/assets/407e16fa-8749-47ac-8baf-e4519ebaaa93" width="1000">
_Fig. 11: Tuning the depth of trees in the random forest_

# Need to finish
### Model 2: Word2Vec Embeddings/Logistic Regression

# Need to finish
## Discussion Section
This is where you will discuss the why, and your interpretation and your though process from beginning to end. This will mimic the sections you have created in your methods section as well as new sections you feel you need to create. You can also discuss how believable your results are at each step. You can discuss any short comings. It's ok to criticize as this shows your intellectual merit, as to how you are thinking about things scientifically and how you are able to correctly scrutinize things and find short comings. In science we never really find the perfect solution, especially since we know something will probably come up int he future (i.e. donkeys) and mess everything up. If you do it's probably a unicorn or the data and model you chose are just perfect for each other!

### Initial Acquisition
When stored as a .csv, our file was over 50GB in size. AS such, we chose to upload the dataset as a .csv.gz through Globus Connect Personal. Even after that and converting the file to Parquet format, the size remained enormous at 27GB. We leveraged the resources of the SDSC and PySpark in order to manage our data. 

### Pre-Processing
We looked through the 24 different features and decided to drop columns that didn't seem like they would contribute to the polarity of a review. For instance, a feature that stated what part of China a review originated in was unlikely to be relevant, as would the number of readers who voted the review funny. After dropping irrelevant features, we were left with the following columns:

- Number of games owned per author
- Number of reviews written per author
- Author's overall playtime
- Author's playtime when writing the review
- Whether the author recommended the game
- How many readers found the review helpful
- Whether the author received the game for free
- Whether the review was written during early access

At first, we wanted to translate the top three most used languages into English for use in our dataset. However, we ran into multiple issues with various Python packages, the most egregious of all being one that required a subscription in order to properly use. Since we were unable to find a sustainable translation library that fit our needs, we eventually settled for using only the reviews written in English as labeled by the dataset. Furthermore, we also removed any reviews made purely of special characters since they wouldn't contribute any meaning to our model. We looked through the playtimes of each review and found some inconsistencies between authors's playtime at review and their playtime overall. Specifically, some reviews had higher playtime at review than playtime overall, which didn't make sense. As such, we also removed those reviews. 

### Feature Exploration and Visualization
After our preprocessing, we were left with approximately 36 million reviews to work with. We decided to examine author details such as how many games they owned and how long they played the game before reviewing because we suspected those features would have more influence on a review's polarity. However, after exploring the relationships between these features and whether the author recommended the game, we found that there was virtually no correlation between the difference in values and the proportions of authors who recommended the game. To confirm our suspicions, we created a large correlation matrix consisting of all the features we kept, looking for any sort of relationship between our features and the review polarity. The correlation matrix confirmed that there was virtually no correlation between our numerical features and our target feature, which meant that any moodel we made would almost entirely rely on the review text itself. With that in mind, we decided to drop all of our features except for the bodies of the reviews. 
### Model 1: TF-IDF

### Model 2: Word2Vec Embeddings/Logistic Regression
why:
-We moved onto word embeddings as the richer semantic meanings held within the review text could posasibly drive classification accuracy up.
-We chose logistic regression as it is well suited for text based sentiment analysis.
-We specifically chose to use a sepereate, reduced preprocessing for thw embeddings model due to requiring us to maintain as much of the original review as we could.
-We underampled so that we would not have a postitive review bias within the model.
-Further downsamplign was for faster hyperparamter tuning.
-We employed hyperparameter tuning to make more accurate our model.
-We retested on our full downsampled dataset to understand how well our tuned model performed.

# Need to finish
## Conclusion
This is where you do a mind dump on your opinions and possible future directions. Basically what you wish you could have done differently. Here you close with final thoughts.

# Need to finish
## Statement of Collaboration
All work was completed collaboratively as a team, and no individual held a specific title or role.
- Timothy Chang: 
- Julia Klayman: 
- David Lightfoot: 
- Tristan Roman: 

# Need to finish
## Final Model and Results Summary
Our Dataset: https://www.kaggle.com/datasets/kieranpoc/steam-reviews/data

Our Setup Requirements: 

### Model 2: Word2Vec

### Pre-Processing

### Model Results

### Iterations: Hyperparameter Tuning

