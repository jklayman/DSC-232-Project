# DSC 232R Group Project
## Milestone 1 - Abstract, Data, and Group Selection

### Group Members
Timothy Chang

Julia Klayman

David Lightfoot

Tristan Roman


### Abstract:

Steam user reviews offer a rich source of data combining free-form sentiment, user behavior, and platform engagement. In this project, we aim to build a high-accuracy classification model that can predict the sentiment of a review based on both the review text and accompanying metadata. After preprocessing a dataset of over 113 million reviews, such as by filtering incomplete entries and translating non-English reviews, we will conduct extensive exploratory correlation analysis to identify which features—such as playtime, review length, formatting, purchase status, and user engagement metrics—are most predictive of sentiment. By understanding which variables correlate strongly with review polarity (positive vs. negative), we can refine our feature set and improve model performance. Sentiment will be derived both from user-provided labels and natural language processing techniques applied to the review text. Our pipeline will be built in PySpark to handle the dataset’s scale, and we will experiment with multiple supervised learning models including logistic regression, random forests, and gradient-boosted trees. The goal is to create a robust, interpretable model that not only classifies sentiment effectively but also reveals underlying patterns in user behavior and review dynamics on the Steam platform.

### Data Description:
The "100 Million+ Steam Reviews" dataset on Kaggle comprises over 113 million user reviews from the Steam gaming platform. Each review includes details such as the author's Steam ID, the number of games owned, the number of reviews written, and total playtime. This extensive dataset is valuable for analyzing user behavior, sentiment analysis, and recommendation systems in the gaming industry.

### Collection Methodology: 
Game IDs were processed individually, with 10 IDs handled simultaneously per machine. API requests were made, raw responses saved as JSON, and data parsed. Once processed, a SQL database was created with the review data, and smaller CSV files were generated using SQLite for faster download/upload and training.

Link to our data: https://www.kaggle.com/datasets/kieranpoc/steam-reviews/data
