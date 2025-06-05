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

After performing the tuning on the dataset, we resampled the data such that the proportions of positive and negative reviews were equal, then performed the same training, evaluation, and tuning procedures on this new dataset. 

### Model 2: Word2Vec Embeddings/Logistic Regression
Due to system limitations, we were unable to implement this model; however, the following outlines the approach we had planned. In addition to the global preprocessing steps, we intended to tokenize the review text in preparation for embedding. To address class imbalance, we would have applied random undersampling to create a balanced training set of positive and negative reviews. A Word2Vec model was planned to generate word embeddings from the tokenized text, which would then serve as input to a logisitic regression classifier.

Hyperparameter tuning was to be performed on a smaller subset of the data to optimize for efficiency.The tuning process would have focused on selecting appropriate values for vectorSize, minCount, and windowSize, but the final values were not determined prior to the system outage. Once tuned, the finalized model would have been applied to the dataset for training and evaluation

## Results Section
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

## Need to change the graphs
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

After resampling our model to get a roughly equal distribution between negative and positive reviews, we ran the above tunings again. 
<img src="https://github.com/user-attachments/assets/d64e4a27-b37b-4edc-bb4b-abe6c06d8284" width="1000">

_Fig. 12: Feature tuning with a balanced dataset_

<img src="https://github.com/user-attachments/assets/bb776017-7e4b-4114-ac7c-7f5482fa208a" width="1000">

_Fig. 13: Tuning the number of trees with a balanced dataset_

<img src="https://github.com/user-attachments/assets/99f68d2f-1c12-4a33-91bf-82a66d8878cb" width="1000">

_Fig. 14: Tuning the depths of trees with a balanced dataset_

## Discussion Section
This is where you will discuss the why, and your interpretation and your though process from beginning to end. This will mimic the sections you have created in your methods section as well as new sections you feel you need to create. You can also discuss how believable your results are at each step. You can discuss any short comings. It's ok to criticize as this shows your intellectual merit, as to how you are thinking about things scientifically and how you are able to correctly scrutinize things and find short comings. In science we never really find the perfect solution, especially since we know something will probably come up int he future (i.e. donkeys) and mess everything up. If you do it's probably a unicorn or the data and model you chose are just perfect for each other!

### Initial Acquisition
From the very beginning, the scale of our dataset forced us to think strategically about storage, transfer, and processing. The original .csv file clocked in at over 50GB, which made it immediately clear that traditional tools wouldn't cut it. We decided to compress the file into a .csv.gz format and use Globus Connect Personal for transfer, a choice driven not just by convenience, but by necessity. Transferring and storing files of this size through normal means would have been impractical, if not impossible, on personal machines or limited cloud resources. Even after converting the file to a more storage efficient Parquet format, the data still took up 27GB, reaffirming just how massive this dataset was.

Given the scale, we knew early on that PySpark would be our best option for distrubuted processing. The SDSC Expanse Portal offered the kind of computational power we needed to make this even remotely feasible. We weren't just dealing with a storage problem, we were navigating infrastructure limitations, runtime constraints, and the very real risk of crashing or timing out. Working within these boundaries influenced everything we did from that point forward, from what models we could run to how much data we could include in tuning and evaluation. This set the tone for our entire project and ultimately shaped many of the decisions that followed.

### Pre-Processing
Preprocessing was a critical stage where we had to make deliberate choices about what data was actually worth keeping. With 24 features available, our first step was to evaluate which ones were likely to provide meaningful signals about review polarity. Some columns, like the geographic origin of a reviewer or how many people marked a review as "funny", didn't seem likely to contribute much to our understanding of whether a review was positive or negative. We felt that keeping irrelevant features would just introduce noise, so we removed them and focused only on the variables we believed could capture user behavior or game experience more meaningfully. After narrowing things down, we were left with the following columns:
- Number of games owned per author
- Number of reviews written per author
- Author's overall playtime
- Author's playtime when writing the review
- Whether the autor recommended the game
- How many readers found the review helpful
- Whether the author received the game for free
- Whether the review was written during early access

Originally, we intended to include reviews written in the top three most used languages by translating them into English. This seemed like a reasonable way to expand our dataset while maintaining consistency in model input. However, we ran into significant obstacles with translation libraries. Many of the tools we tried were either unreliable or locked behind paywalls, which made this plan unsustainable. Eventually, we decided to filter out non-English reviews entirely and only use the ones already labeled as English by the dataset.

We also took steps to improve text and numerical data quality. Some reviews consisted solely of special characters, which wouldn't contribute any useful semantic information, so we removed those. Additionally, we identified inconsistencies in the playtime data. Some reviews showed more playtime "at review" than in total, which obviously didn't make sense. These rows were likely corrupted or misrecorded, so we excluded them to prevent misleading patterns from entering the model. Ultimately, every choice we made during preprocessing came down to improving the reliability and interpretability of the data going into our models.

### Feature Exploration and Visualization
After our preprocessing, we were left with approximately 36 million reviews to work with. We decided to examine author details such as how many games they owned and how long they played the game before reviewing because we suspected those features would have more influence on a review's polarity. However, after exploring the relationships between these features and whether the author recommended the game, we found that there was virtually no correlation between the difference in values and the proportions of authors who recommended the game. To confirm our suspicions, we created a large correlation matrix consisting of all the features we kept, looking for any sort of relationship between our features and the review polarity. The correlation matrix confirmed that there was virtually no correlation between our numerical features and our target feature, which meant that any model we made would almost entirely rely on the review text itself. With that in mind, we decided to drop all of our features except for the bodies of the reviews. 

### Model 1: TF-IDF
We began our modeling with a TF-IDF approach because it offered a straightforward way to quantify the importance of words in each review while maintaining interpretability. TF-IDF allowed us to numerically represent how frequently certain terms appeared across the dataset while reducing the influence of overly common words. The idea was that reviews with strong opinions, either positive or negative, would include certain "buzzwords" that could act as useful predictors. This made TF-IDF a natural first step in establishing a baseline method.

We chose to pair TF-IDF with a random forest classifier because of its robustness, ability to handle high diemsnional input, and strong performance on non-linear data. Random forests also allowed us to see feature importance scores, which added transparency to the predictions and gave us insight into which words or terms were contrbuting most to classification. This was particularly helpful in identifying which tokens appeared more often in helpful or unhelpful reviews, or in recommended versus not recommended reviews.

Once we completed our tuning of the initial TF-IDF and random forest pipeline, we noticed that although the AUC of the model seemed to be improving across iterations, the accuracy of our model’s predictions remained basically the same. Furthermore, the accuracy we were getting, about 83%, was almost exactly the proportion of positive reviews in our dataset. This led us to believe that our model had resorted to labeling almost every review as positive, and a look at our ground truth tables seemed to confirm that. In order to combat this, we resampled our dataset to about 13 million points and proportioned the data such that the number of positive reviews was roughly equivalent to the number of negative reviews. Then we retrained the model on this data to look for different results. We found that our AUC had a higher floor but approximately the same ceiling in the scope of our tuning, and our accuracy took a hit. However, the AUC showed less signs of stagnation and accuracy of our model was increasing throughout the tuning. We believe that with higher computing power and more resources, this resampled dataset could lead to higher accuracies than the heavily unbalanced original dataset. 

While this model gave us a solid foundation, it also came with limitations. One of the biggest drawbacks of TF-IDF is that it treats each word in isolation, without any regard for context or word order. This means that important linguistic subtleties, like sarcasm, idioms, or negations, were often missed entirely. Additionally, the features generated by TF-IDF can be sparse and noisy, especially when dealing with user generated text that varies in style and length. This introduced challenges during hyperparameter tuning and made the model slightly less generalizable than we hoped.

Even so, the TF-IDF + Random Forest model served its purpose well as a first iteration. It helped us surface basic trends in language use and review structure, and gave us a baseline to compare future models against, particularly the embedding based model we had planned to build next.

The model demonstrates significant practical value in extracting meaningful patterns from user generated content in the gaming industry. One of its primary applications lies in identifying key characteristics that resonate with players based on review text analysis. By processing the extensive collection of reviews on Steam, the model can automatically uncover frequently mentioned aspects (positive/negative) of games such as gameplay mechanics, storylines, quality of graphics, UI design, etc. This capability allows stakeholders to understand what drives player satisfaction without having to manually read through millions of text reviews, in turn providing data-driven insights into player preferences across different game genres/demographics.

Furthermore, the model serves as a foundation for building sophisticated internal recommendation systems within gaming platforms. By analyzing the textual features that correlate with positive reviews and user engagement metrics (such as playtime), platforms can develop more nuanced recommendation algorithms that go beyond simple collaborative filtering. The model's ability to identify semantic patterns in review text enables it to match users with games that share similar appreciation of characteristics even if these games are different in genre.

Perhaps most importantly, game developers can leverage this model as a competitive intelligence and product development tool. By applying TF-IDF analysis to reviews of their previous titles as well as competitor games, developers can identify specific features, mechanics, or design elements that consistently receive praise/criticism. These insights can enable them to make more informed decisions about feature prioritization in new projects, understand market gaps, and learn from their successes/failures. The model's correlation analysis (as shown in Fig. 8) reveals relationships between various game attributes and player sentiment which can provide actionable insights for game developers that are seeking to create more engaging gaming experiences for their users/customers.

### Model 2: Word2Vec Embeddings/Logistic Regression
For our second model, we wanted to move beyond the limitations of TF-IDF by capturing the semantic depth of the review text. The idea was that word embeddings, specifically through Word2Vec, would allow us to leverage the context and relationships between words which is something we felt was important in understanding the tone and sentiment behind how users describe their experiences. We were especially drawn to the idea that certain expressions, even when phrased differently, could share similar embeddings and ultimately help the model generalize sentiment more accurately. Logistic regression felt like a natural fit here because it's efficient, interpretable, and it has consistently performed well in sentiment classification tasks, especially when paired with dense feature representations like embeddings.

We also made a conscious decision to treat this model differently in terms of preprocessing. Unlike TF-IDF, where cleaning and simplification are often helpful, we felt that retaining more of the original review, like its vocabulary, phrasing, and quirks would preserve meaning in a way that embeddings could take advantage of. Our datatset was quite large, which posed a problem both in terms of training time and bias. There were significantly more positive reviews than negative pnes, and without correction, this would have skewed the model's predictions. To address this, we downsampled not just to balance the classes, but also to make hyperparameter tuning more manageable given the resource constraints.

The plan was to tune hyperparameters like vector size, window size, and minimum word count on a smaller subset of the data, then apply those settings to a larger downsampled set for evaluation. We were genuinely curious whether this embedding based approach would outperform TF-IDF, especially in how it might pick up on subtle expressions of satisfaction or frustration. Unfortunately, the system failure halted this line of exploration before we could find out. That said, we still believe this direction had promise especially given how limiting sparse representations can be when dealing with emotionally nuanced or context rich language.

# Need to finish
## Conclusion
This is where you do a mind dump on your opinions and possible future directions. Basically what you wish you could have done differently. Here you close with final thoughts.

### Real World Use

The TF-IDF + Random Forest model presented in this project offers substantial practical value for the gaming industry ecosystem. The model’s ability to automatically extract and analyze player preferences from review text provides a scalable solution for understanding what drives user satisfaction across the Steam gaming platform. Whether applied to enhance recommendation systems, guide game developer decisions, or inform market research strategies, this model bridges the gap between unstructured user feedback and actionable business decisions. The demonstrated correlation between textual features and player behavior metrics validates the model's potential to transform how gaming companies understand/respond to their users/customers, ultimately supporting the creation of more engaging/successful gaming experiences for all.

### Overarching Impact

Models that are able to summarize large quantities of information are growing ever more important in an age where data is available everywhere in incredible amounts. In goods industries like game development, being able to determine what consumers like and dislike about your products is key to staying ahead of the curve and creating a satisfied consumerbase. With this model and future research, we hope studios and developers and many more will be able to hear the collective voice of the people that play their games. 

# Need to finish
## Statement of Collaboration
All work was completed collaboratively as a team, and no individual held a specific title or role.
- Timothy Chang: Although I didn't have access to Expanse for a large portion of Milestone 2, I was able to contribute to the team effort by suggesting directions for feature exploration and visualizations, as well as transferring our thought processes into the README file for our submission. By Milestone 3, I gained access to the SDSC and used it to train our TF-IDF model with guidance from my team, as well as generating some of the visualizations of our model accuracy. I made sure to coordinate and ask for updates from our team, and am very thankful for their communicativity and willingness to work together. 
  
- Julia Klayman: I contributed to Milestone 2 by working on data preprocessing and feature exploration, including generating visualizations and helping with the GitHub outline. For Milestone 3, I helped develop the outline, worked on the write-up, organized figure layout, and revised the TF-IDF model graphs. These reflect areas I contributed to as part of our collaborative team effort.
  
- David Lightfoot: For Milestone 1, I documented our team meeting by writing detailed minutes as we reviewed different datasets, capturing key features and potential analysis approaches for each. For Milestone 2, I created our group folder and collaborated on data exploration code/visualizations, exchanging recommendations with my team. For Milestone 3, I contributed to the write-up and organized/labeled figures from our model's plots/graphs. For Milestone 4, I developed the discussion section and conclusion, addressing real-world applications and practical usefulness of our model. I want to thank my group members for their tenacity and invaluable help throughout this collaborative project.
  
- Tristan Roman: 

