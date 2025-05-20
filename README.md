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

## Results Section
This will include the results from the methods listed above (C). You will have figures here about your results as well. No exploration of results is done here. This is mainly just a summary of your results. The sub-sections will be the same as the sections in your methods section.

## Discussion Section
This is where you will discuss the why, and your interpretation and your though process from beginning to end. This will mimic the sections you have created in your methods section as well as new sections you feel you need to create. You can also discuss how believable your results are at each step. You can discuss any short comings. It's ok to criticize as this shows your intellectual merit, as to how you are thinking about things scientifically and how you are able to correctly scrutinize things and find short comings. In science we never really find the perfect solution, especially since we know something will probably come up int he future (i.e. donkeys) and mess everything up. If you do it's probably a unicorn or the data and model you chose are just perfect for each other!

## Conclusion
This is where you do a mind dump on your opinions and possible future directions. Basically what you wish you could have done differently. Here you close with final thoughts.

## Statement of Collaboration
- Timothy Chang: Title, contribution
- Julia Klayman: Title, contribution
- David Lightfoot: Title, contribution
- Tristan Roman: Title, contribution

## Final Model and Results Summary
### Model 2: Word2Vec
Our second model will be a Word2Vec model, which captures semantic meaning and context from the review text by learning vector representations of words. These embeddings will then be used as features in a logistic regression classifier to predict whether the author liked the game.

### Pre-Processing

### Model Results

### Iterations: Hyperparameter Tuning

