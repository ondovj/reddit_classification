# Subreddit Classification: r/gaming and r/pcgaming

## Problem Statement
Reddit can sometimes be a confusing place. With so many communities specializing in so many different topics, it can easily be overwhelming to keep different subreddits straight. To this end, it could be useful to have a model that could take a specific post, and predict which subreddit it came from. Specifically, we want a model to be able to classify posts between two gaming subreddits, r/gaming and r/pcgaming. Gaming is a general gaming subreddit, mostly centered around video games, whereas PCgaming is cetnered specifically around video games played on PCs, and the PCs that are built to play those games. 

In order to build this model, posts from each subreddit will be required, and the lanuage in those posts will need to be analyzed. This is specifcally a binary classification problem, so the modeling techniques used must be able to make this kind of prediction. In order to compare the differnet models, the accuracy metric will be utilized. Ideally, this model will have a significant boost in performance compared to a baseline mode prediction, and adapt to new data well.

## Executive Summary
The first step of this project was to acquire the necessary data, and attempt to pull 1000 posts from each targeted subreddit. In the end, we found that 40-50% of the posts that we pulled from each subreddit had been repeated.

The real work began when the target information (the post title) had been isolated. Each of the posts had unique words and language patterns that could help to decipher which subreddit it had been from. In order to fully analyze this information we had to use NLP (natural language processing). This was an iterative and multi-step process that included stemming words down to their base roots, removing so-called "stop words" in order to access the words conveying more meaning, and even removing some of the words that were very frequent in both of the subreddits (such as "game", "gaming, and "new"). 

As there are many modeling techniques available to solve a classification problem, it was important to be able to optimize each model, and then compare the best models against each other. To this end, we used pipelines and gridsearches to perform hundreds of fits at a time. As always, a `train_test_split` was used to keep a holdout dataset from the model for testing purposes. Two vectorizers were utilized, `CountVectorizer` and `TfidfVectorizer` in order to optimize the way that each title's words were incorported. For the classifiers, five different types were utilized: `LogisticRegression`, `KNearestNeighbors`, `MultinomialNB`, `RandomForestClassifier`, and `AdaboostClassifier`. Each of these models have certian benefits and pitfalls, and the results of each were somehwat unexpected. 

In the end, The model that was able to optimize accuracy, and still have a reasonable balance between bias and variance, utilized `TfidfVectorizer` and `AdaboostClassifier`. This model was able to give a 78.4% accuracy on the testing set. We believe this model is effective enough to continue classifying other psots between these two subreddits, mainly due tot eh fact that this case has fairly low stakes for misclassifications.

## Datasets
There are two original datasets made from the inital post pulls: gaming_posts.csv and pcgaming_posts.csv. 
After these were made, the desired information (the post title) was isolated from them, and stored in new datasets: gaming_titles.csv and pcgaming_titles.csv. These latter datasets were then used in the second notebook for the majority of the project.

## Conclusions
This project has proven that natural language processing is a difficult tool to effectively wield. It is highly important to have a knowledge of the text that is being processed in order to have the most effective results, and it can be difficult to determine when and how certain steps need to be taken.

In the end, we were able to create a fairly effective boosted decision tree model to classify subreddit posts between the two target subreddits, r/gaming and r/pcgaming. At above 78% accurate, it is a significant step up from a coin flip or the base model, though it is still far from perfect. A decision to use a model of this performance to classify these subreddits (or another pair of classifications) greatly depends on the risk involved in those classifications. For this project, the stakes seem quite low, but in other situations a 78.4% accuracy may be completely unacceptable.

It is unfortunate that this model type leaves almost no transparency in which words were or were not important in making the classification. 

It also eis important to note that this model will only be effective when classfying posts between these two specific subreddits. The model is built on the specific words that are used in these subreddits, and other subreddits will most likely have many differences in the words that are used, and how they are being used.

The words being used even within these two subreddits is likely to evolve over time, especially when important events happen in the gaming industry, so the effectiveness of this model may not last a long time. The data that this model was built on was pulled from a specific period of time, on or around July 9, 2019.

## Recommendations
It does appear that there may be some further optimization to be done with this model. Also with our testing score still being below 80%, it would be helpful to build something a bit more accurate, possibly by fitting another type of model, such as a support vector machine. 

With the issue that was found regarding the `n_estimators` hyperparameter in the boosted models, we see an opportunity to perform and even deeper dive into the hyperparameters that were used, and even those that had not been changed here. There may be a model that has similar or better performance to our final selected model, when a very specific combination of parameters are used, which may not be immediately noticable when using a typical gridsearch. There is also a wealth of additonal information on each of the posts from the initial pull. While we isolate the title from each post, in the original datasets there was additional information such as selftext, number of comments, and more. These could all prove to be useful pieces of information for this classification problem.

Additionally, in order to keep the model as accurate as possible, new data should be pulled periodcially to add to the dataset, so as to account for changing trends in post language.