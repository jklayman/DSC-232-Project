# DSC 232R Group Project

## Introduction to Our Project
The Steam gaming platform hosts one of the largest online communities for video game players, where user-generated reviews provide an invaluable stream of feedback, sentiment, and behavioral insight. We chose the "100 Million+ Steam Reviews" dataset because of its vast scale, rich metadata, and real-world application in understanding how players engage with games. Unlike many datasets that focus solely on text or numeric features, this one offers a unique combination of both, allowing for a multi-dimensional analysis of sentiment and behavior.

What makes this dataset especially compelling is its relevance not only to game developers but also to researchers in natural language processing, behavioral analytics, and recommendation systems. A well-performing predictive model built on this data could help platforms like Steam automatically detect spam or troll reviews, recommend games more intelligently, or highlight trending titles based on authentic positive feedback. Additionally, identifying which variables most influence sentiment, such as playtime, purchase behavior, or review formatting, provides actionable insight into user engagement and content quality.

The broader impact of this project extends beyong gaming. Techniques developed here can be applied to other domains where user feedback shapes product visibility and platform trust, such as Amazon, Yelp, or social media. Building a reliable, interpretable sentiment classifier supports the creation of safer, more personalized, and data-informed digital environments.

## Figures
Your report should include relevant figures of your choosing to help with the narration of your story, including legends (similar to a scientific paper). For reference you search machine learning and your model in google scholar for reference examples.

## Methods Section
This section will include the exploration results, preprocessing steps, models chosen in the order they were executed. You should also describe the parameters chosen. Please make sub-sections for every step. i.e Data Exploration, Preprocessing, Model 1, Model 2, additional models are optional. Please note that models can be the same i.e. DNN but different versions of it if they are distinct enough. Changes can not be incremental. You can put links here to notebooks and/or code blocks using three ` in markup for displaying code. so it would look like this: ``` MY CODE BLOCK ```
Note: A methods section does not include any why. the reason why will be in the discussion section. This is just a summary of your methods

### Initial Acquisition
We first evaluated data formats and transfer methods. We selected the .csv.gz format to upload our dataset to SDSC Expanse. 

We used Globus to transfer the 50GB dataset to SDSC Expanse. A local machine was configured as an endpoint and the file was successfully moved to the Expanse scratch directory.

The data was then converted to Parquet. We configured Spark with maximum available resources to further streamline processing.

The dataset consists of approximately 113 million row and 24 columns, including game identifiers, user behavior metrics, playtime data, and free-form review text.

### Pre-Processing
Several cleaning and transformation steps were applied to get the dataset ready for modeling:
- Non-English reviews were filtered out entirely to maintain consistency and avoid translation overhead.
- Empty reviews and those containing only symbols or ASCII characters were removed.
- All review text ws converted to lowercase, with light cleaning to remove excess whitespace and selected punctuation.
- Playtime columns were converted from minutes to hours for better interpretability.
- Rows with logical inconsistencies in playtime values (e.g., lifetime playtime being less than playtime at review) were dropped.
- Boolean columns were cast to integers, and irrelevant or mostly empty columns were removed from the final dataset.

These steps allowed us to reduce noise and streamine the data while preserving the features most likely to influnce model performance.

### Feature Exploration and Visualization

We found there were a total of 94164 unique games that had reviews in our pre-processed dataset. Of those games, several were reviewed more than others, with a notable example being Counter-Strike 2, boasting over 1 million reviews to its name (Fig. 1). The average review length was around 300 characters, but some reviewers waxed eloquent about their games for over 5000 characters (Fig. 2). The distribution of review lengths was heavily right skewed, a trend which was mirrored in the amount of games owned by each reviewer. While the average number of games owned was around 170, some individuals owned over 5000 games (Fig. 3). 

After diving into reviews, their lengths, and some characteristics about the reviewers, we decided to explore correlations between features. We analyzed authors' playtime when they reviewed the game, their playtime after they reviewed the game, how long their review was, whether the game was received for free, and whether or not the authors played the game in early access and compared those statistics to whether or not the author recommended the game (Figs. 4-7). We made the discovery that there was little correlation between each of these features and the author's recommendation. To verify this, we created a comprehensive correlation matrix and visually examined it to look for notable correlation scores (Fig. 8). Our figure revealed what we had already suspected--these categorical features had little to no correlation with each other, much less to the author's recommendation. With this in mind, we realized that if we decided to use these features in conjunction with the text of the review, any model would mostly or completely rely on the review body and would likely disregard any remaining features. As a result, we dropped these features and chose to create models that analyzed the text of the reviews. 

<table>
  <tr>
    <td align="center">
      <img src="https://github.com/user-attachments/assets/6c7f6d6b-d54c-47cf-9002-26de91e37902" width="500" height="300">
      <br>
      <em>Fig. 1</em>
    </td>
    <td align="center">
      <img src="https://github.com/user-attachments/assets/9e1612cb-978a-4c05-816d-1e5050e5a2a3" width="500" height="300">
      <br>
      <em>Fig. 2</em>
    </td>
  </tr>
</table>
<table>
  <tr>
    <td align="center">
      <img src="https://github.com/user-attachments/assets/61f2e4a4-ea5a-4c6d-ab4c-457f6a750777" width="500" height="300">
      <br>
      <em>Fig. 3</em>
    </td>
    <td align="center">
      <img src="https://github.com/user-attachments/assets/0a826549-5202-441a-91a5-52cbd470fb90" width="500" height="300">
      <br>
      <em>Fig. 4</em>
    </td>
  </tr>
</table>
<table>
  <tr>
    <td align="center">
      <img src="https://github.com/user-attachments/assets/314e957e-a146-47e8-b338-d330762b0a93" width="500" height="300">
      <br>
      <em>Fig. 5</em>
    </td>
    <td align="center">
      <img src="https://github.com/user-attachments/assets/04655370-dda2-4e60-965d-3c040ce67a3e" width="500" height="300">
      <br>
      <em>Fig. 6</em>
    </td>
  </tr>
</table>
<table>
  <tr>
    <td align="center">
      <img src="https://github.com/user-attachments/assets/d1ff4e6b-48ad-4712-bf63-b065b392242a" width="500" height="300">
      <br>
      <em>Fig. 7</em>
    </td>
    <td align="center">
      <img src="https://github.com/user-attachments/assets/d8b4731e-72a0-4af1-ad6c-8c7bab7c565c" width="500" height="500">
      <br>
      <em>Fig. 8</em>
    </td>
  </tr>
</table>

_Fig. 1: The number of reviews on the top 10 most reviewed games._

_Fig. 2: The distribution of the lengths of the reviews._ 

_Fig. 3: The distribution of the amount of games owned by review authors._ 

_Fig. 4: The relationship between the playtime after review and the author's review._ 

_Fig. 5: The relationship between the review length and the author's review._

_Fig. 6: The relationship between whether or not the game was received for free and the author's review._ 

_Fig. 7: The relationship between whether the game was played in early access and the author's review._ 

_Fig. 8: The correlation matrix between all the non-linguistic features._ 

## Model 1: TF-IDF

We first chose to make a TF-IDF model, which would quantify the words of the review text and search for words that tended to appear more in positive reviews than negative reviews, and vice versa. We hoped that this model would reveal common buzzwords that were often used to either praise or criticize games. To prepare our data for this model, we tokenized the review text, removed common stopwords and special characters, and ran a simple lemmatizer to group words with similar roots. Then we split our data into training and test sets and got to training. We used a pipeline of TF-IDF and a random forest classifier for binary classification, trained the model on our training data, then evaluated our training data and test data for the AUC of our model's predictions. After our initial pass with the model, we conducted hyperparameters, iterating over the maximum number of features, the number of trees in the forest, and the maximum depth of those trees (Figs 1-3). 

![image](https://github.com/user-attachments/assets/5edf09dc-c1f6-4eb6-810b-4a26a5ab09be)

_Fig. 1: Tuning the number of features in HashingTF_

![image](https://github.com/user-attachments/assets/b2a6bfca-9012-430b-8f8f-e0d838dd4f1f)

_Fig. 2: Tuning the number of trees in the random forest_

![image](https://github.com/user-attachments/assets/407e16fa-8749-47ac-8baf-e4519ebaaa93)

_Fig. 3: Tuning the depth of trees in the random forest_

## Model 2

## Results Section
This will include the results from the methods listed above (C). You will have figures here about your results as well. No exploration of results is done here. This is mainly just a summary of your results. The sub-sections will be the same as the sections in your methods section.

## Discussion Section
This is where you will discuss the why, and your interpretation and your though process from beginning to end. This will mimic the sections you have created in your methods section as well as new sections you feel you need to create. You can also discuss how believable your results are at each step. You can discuss any short comings. It's ok to criticize as this shows your intellectual merit, as to how you are thinking about things scientifically and how you are able to correctly scrutinize things and find short comings. In science we never really find the perfect solution, especially since we know something will probably come up int he future (i.e. donkeys) and mess everything up. If you do it's probably a unicorn or the data and model you chose are just perfect for each other!

### Initial Acquisition
To support efficient processing of our large dataset, we first evaluated data formats and transfer methods. We selected the .csv.gz format for its compatibility with Spark and its ability to stream directly without decompression. This eliminated the need for storing redundant uncompressed copies and allowed faster load times.

Due to file size limitations, we used Globus to transfer the 50GB dataset to SDSC Expanse. A local machine was configured as an endpoint and the file was successfully moved to the Expanse scratch directory, ensuring reliable upload and resumption support.

Initial attempts to process the full CSV file were limited by cluster performance. To address this, the data was converted to Parquet, significantly improving read speed and reducing I/O bottlenecks. We configured Spark with maximum available resources to further streamline processing.

The dataset consists of approximately 113 million row and 24 columns, including game identifiers, user behavior metrics, playtime data, and free-form review text.

### Pre-Processing
Several cleaning and transformation steps were applied to get the dataset ready for modeling:
- Non-English reviews were filtered out entirely to maintain consistency and avoid translation overhead.
- Empty reviews and those containing only symbols or ASCII characters were removed.
- All review text ws converted to lowercase, with light cleaning to remove excess whitespace and selected punctuation.
- Playtime columns were converted from minutes to hours for better interpretability.
- Rows with logical inconsistencies in playtime values (e.g., lifetime playtime being less than playtime at review) were dropped.
- Boolean columns were cast to integers, and irrelevant or mostly empty columns were removed from the final dataset.

These steps allowed us to reduce noise and streamine the data while preserving the features most likely to influnce model performance.

### Feature Exploration and Visualization

We found there were a total of 94164 unique games that had reviews in our pre-processed dataset. Of those games, several were reviewed more than others, with a notable example being Counter-Strike 2, boasting over 1 million reviews to its name. The average review length was around 300 characters, but some reviewers waxed eloquent about their games for over 5000 characters. The distribution of review lengths was heavily right skewed, a trend which was mirrored in the amount of games owned by each reviewer. While the average number of games owned was around 170, some individuals owned over 5000 games. 

After diving into reviews, their lengths, and some characteristics about the reviewers, we decided to explore correlations between features. We analyzed authors' playtime when they reviewed the game, their playtime after they reviewed the game, how long their review was, whether the game was received for free, and whether or not the authors played the game in early access and compared those statistics to whether or not the author recommended the game. We made the discovery that there was little correlation between each of these features and the author's recommendation. To verify this, we created a comprehensive correlation matrix and visually examined it to look for notable correlation scores. Our figure revealed what we had already suspected--these categorical features had little to no correlation with each other, much less to the author's recommendation. With this in mind, we realized that if we decided to use these features in conjunction with the text of the review, any model would mostly or completely rely on the review body and would likely disregard any remaining features. As a result, we dropped these features and chose to create models that analyzed the text of the reviews. 

## Model 1: TF_IDF

We first chose to make a TF-IDF model, which would quantify the words of the review text and search for words that tended to appear more in positive reviews than negative reviews, and vice versa. We hoped that this model would reveal common buzzwords that were often used to either praise or criticize games. To prepare our data for this model, we tokenized the review text, removed common stopwords and special characters, and ran a simple lemmatizer to group words with similar roots. Then we split our data into training and test sets and got to training. We used a pipeline of TF-IDF and a random forest classifier for binary classification, trained the model on our training data, then evaluated it for AUC

## Conclusion
This is where you do a mind dump on your opinions and possible future directions. Basically what you wish you could have done differently. Here you close with final thoughts.

## Statement of Collaboration
All work was completed collaboratively as a team, and no individual held a specific title or role.
- Timothy Chang: 
- Julia Klayman: 
- David Lightfoot: 
- Tristan Roman: 

## Final Model and Results Summary
Our Dataset: https://www.kaggle.com/datasets/kieranpoc/steam-reviews/data

Our Setup Requirements: 110 cores, 220GB memory per node

### Model 2: Word2Vec
Our second model will be a Word2Vec model, which captures semantic meaning and context from the review text by learning vector representations of words. These embeddings will then be used as features in a logistic regression classifier to predict whether the author liked the game.

### Pre-Processing

### Model Results

### Iterations: Hyperparameter Tuning

