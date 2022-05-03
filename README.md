# Hey, What Game Are You Playing?

Reddit is a popular forum with many active sub-forums (otherwise known as subreddits). This project will take data from two subreddits to feed into a classifier model, meant to predict which game subreddit the user post or comment originated from.

## Subreddits

The model will look at two game subreddits:

- Stardew Valley
- Elden Ring 

![stardewvalley](images/stardew.png)

Stardew Valley is a calming, farm and dating simulator. In Stardew Valley, players create a character who receives an overgrown farm owned by their late grandfather. They will cultivate the farm, and get to know the locals in Pelican Town. Stardew Valley increased in popularity during the COVID Pandemic because of its relaxing properties.

![elden](images/elden.jpeg)

Elden Ring is a dark fantasy, action role-playing game. Elden Ring descends from the Souls series games, which are notorious for their difficulty. As opposed to Stardew Valley, there will be few to no friendly non-playable characters in Elden Ring as players combat escalatingly more difficult bosses and monsters to level up.

The data analyst in this project has played neither games. Given the different tones of each game, a classifier model will be built to see if the games can be distinguished through their Reddit posts.

## Data Collection

6000 comments and posts were collected in total from both subreddits.  The following items were scraped using PushShiftAPI:
- author
- subreddit
- self text
- title
- body

Since the subreddit posts primarily consisted of images and memes, the post title was collected along with the post body, and combined into one data point for analysis. Since posts often contained images instead of text, a greater amount of comments were also scraped. Within the 6000 pieces of data, 4000 (66%) of data came from comments.

## Data Dictionary

|Feature|Type|Description|
|---|---|---|
|subreddit|object| Subreddit origin|
|body|object| Body of comment, or combined title and body of post|
|lemmatized|object| Lemmatized body of comment, or combined title and body of post|
|stemmed|object| Stemmed body of comment, or combined title and body of post|
|type|object| Post or comment|

## Preprocessing

In initial exploratory analysis, it was discovered there were multiple overlapping words that appeared commonly in both subreddits. These overlapping words were added as stopwords before the data was transformed by the CountVectorizer and TFIDFVectorizer. In different iterations, it was found that there was decreased model performance with TFIDFVectorizer. Using CountVectorizer, there was also minimal model performance difference between using stemmed, lemmatized, or the original text. The originl text was utilized for model training.

## Model Performance

Using GridSearch, the following models were produced.

|Model|Best Train Score|Best Test Score|Best Parameters|
|---|---|---|---|
|Logistic Regression|92.0|82.0|{'C': 0.1, 'max_iter': 1000}|
|KNN Classifier|95.0|64.0|{'n_neighbors': 30, 'p': 2, 'weights': 'distance'}|
|Multinomial Naive Bayes|91.0|82.0|{'alpha': 0.5}|
|Random Forest Classifier|95.0|80.0|{'max_depth': None, 'n_estimators': 400}|


## Best Model Selection

As is, it's suggested to go with the Multinomial Naive Bayes model. It has high predictive power and the lowest case of overfitting between all classification models.

## Follow-Ups on Increasing Model Performance

- While accuracy was high for some models, overfitting was generally an issue across the board. Performance changed based on targeted stopwords. Additional analysis on what additional stopwords can be added, or what stopwords should be removed, could be used to potentially increase model performance.

- Although TFIDFVectorizer did help model performance, finding other means to weight certain words related to specific gameplay (agricultural terms were unique to Stardew Valley, and "dark souls" or "souls" were unique to Elden Ring) in addition to excluding stopwords could potentially increase model performance as well.