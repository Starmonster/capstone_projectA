# capstone_projectA - JMOULDS
Restaurant Recommendation System

Readme Contents:
1. Introduction

2. Data Science Process
    a. Business Understanding
    b. Data Understanding
    c. Data Preparation
    d. Modelling
    e. Evaluation
    f. Deployment

3. Next Steps and Future Improvements

4. Repository Navigation / File Structure

5. Instructions

6. Links
*******************************************************************************************************************************

1. Introduction
    
The objective of this project is to create a restaurant recommendation system that will propose restaurants to tourists or a local population using a variety of machine learning techniques and data analysis skills. The project will utilise data from a variety of sources and all work is written and executed in jupyter notebook. This ReadMe explains the status of the project at Minimum Viable Product stage which will be referenced as 'phase 1' throughout this document. Given the success of 'phase 1' it is anticipated tha 'phase 2' will commence with a focus on improvement, refinement and user friendly deployment. 

*******************************************************************************************************************************


2. Data Science Process

------------CRISP-DM---------- 

2,a. Business Understanding
A new lifestyle startup, Butterly.com,  is seeking to offer restaurant suggestions via their web and mobile applications. They have a growing user base of food lovers and socialites keen to find the latest and greatest places to eat out in cities around the world. They would like a recommendation system to suggest appropriate establishments to its existing users based on their exitsing data and also to engage with new users by recommending restaurants based on minimal feedback or.

The startup has its roots in Scotland so initially they would like to use Edinburgh as a test case before investing in a more global product. 


2,b. Data Understanding
The base data sets used in phase 1 have been sourced from Yelp (via Kaggle.com) and are split into business dataset and a review dataset. The business data set includes information about the businesses including name, location, average rating, number of reviews, and category keywords. The review data set includes every review text and rating listed by user id and business id. 

The raw imported data sets are global in nature and are spread across more than 5 million reviews of businesses world wide. For our project we have filtered out for restaurants based in Edinburgh. This leaves us with the following numbers:

- 28407 reviews
- for 1605 restaurants in Edinburgh
- by 9162 active users

Typically, the reviews are not spread evenly across the user base. In fact only 4 percent of our 9000+ users have racked up more than 10 reviews. This will inevitably lead to some extreme sparseness which will limit the accuracy of our models and also the useful output of our initial recommender. 

We have explicit data in the form of review ratings with marks our of 5 and detailed reviews and tags. And we have implicit data in the form of whether a user has reviewed or not and dates that reviews took place. 

We can also note that as with most product distributions of this kind we have a popular minority that are very well reviewed and a majority that stretch out across ever reducing numbers of reviews. It will be relatively easy to recommend the popular establishments but it is our aim over both phases to locate the hidden gems within the long tail of lesser known eateries. 

------insert long tail-----



2,c. Data Preparation
For phase 1 we have produced 4 '.csv' files to be used variously across our exploratory data analysis and modelling stages. They are as follows: 

- new_collaborative_limit.csv:- This csv file was prepared to be used with our collaborative models. It lists out each and every user instance so we can manipulate the similarities between users. 

- content.csv:- This file was prepared to be used with our content filtering models. It includes the categorical data for each restaurant and also holds all the review text by every user for that restaurant. The data set is designed to be able to compare restaurants with other restaurants based on keyword frequency.  

- edinburgh_reviews.csv:- primarily for analysis and exploration. May be manipulated further in phase 2. 

- new_restaurants.csv:- primarily for analysis and exploration. May be manipulated futher in phase 2.

Other data sources were considered and trialed but remain unused at phase 1. 
- yelp_api:- Whilst were able to successfully call in reviews via the yelp api we were only granted three business reviews per business per call based on yelp's selection criteria. It turned out that they were always the same three reviews so for phase 1 no further investigation of this data source was undertaken. 



2,d. Modelling
We have a number of modelling and filtering approaches: 

- Memory based collaborative filtering using surprise libraries and k-nearest neighbor algorithms KNNWithMeans, KNNBasic, KNNBaseline. We measure similarities using cosine, pearson and msd similarity metrics.  See links section for further reading on the surprise libraries. Our model accuracy was measured using the root mean square error of the model fit. 

a)using indivdual user ratings we compare user similarity based on their common ratings. 
b)using individual user ratings we compare restaurant similarity based on common ratings from users. 


- Model based / matrix factorisation

a)using singular value decomposition (SVD) with parameters tuned through GridSearchCV to reduce the dimensions of the sparse matrix
b)using alternating least squares with hyperparamters tuned through GridSearchCV to reduce dimensionality and minimise the loss functions


- Content based
a) using linear kernel from sklearn pairwaise libraries we measure the similarity of restaurants based on the keywords in key columns. For phase 1 the similarities are based on the category keywords only. The categories give basic key information about a restaurant such as 'chinese', 'bar', 'coffee', 'vegetarian'. 


The best results from our various models are as follows: 

Model type.              Best Train RMSE       Best Test RMSE         Comments / optimum hyperparameters
KNNBasic                 0.7657                0.9987                 user-user, cosine
KNNBaseline              0.7254                0.9500                 user-user, cosine
KNNWithMeans             0.7174                0.9704                 user-user, cosine
MF SVD                   0.6685                0.9048                 5 folds, n_factors=8, n_epochs=20  
MF ALS                   0.7707                0.8908                 8 epochs, user_reg=10, item_reg=3


The content similarity comparisons were run but they do not currently return an error or accuracy metric. It is hoped that they will be used alongside other models to achieve top recommendations. 




2,e. Evaluation

Cold Start / New Users
Because of the sparsness of the current matrices we know that new users are not particularly tailored for. The collaborative recommender essentially acts as a cold start recommender for any user with less than 10 reviews. The recommender merely returns a listed of the best rated and most reviewed resturants. 


Existing Users
Our recommendations predictions are out on average by 0.7 of a rating. The following table shows a small sample of users with their actual rating and the their predicted rating for comparison. The results are mixed but we could argue that they are broadly accurate for the base product across this small sample. 


Table to show model predictions against actual user ratings




2,f. Deployment

Two recommenders are currently deployed with the 'Rec Sys Modelling' jupyter notebook. 
1) Collaborative recommender asks a new user to rate a set number various restaurants and returns the most appropriate alternatives. 

2) Content recommender is currently run through a function. Call the recommender with an argument equal to the name of restaurant form the data set. An appropriate set of n alternative restaurants is returned. 

3) Hybrid recommender is mid build at phase 1 but it would be a product of the recs 1 and 2 when complete. 

********************************************************************************************************************************


3. Next Steps and Future Improvements

As we can see at present there is room for improvement across the board. 

1) We will review the geography of restaurants to see if hotspots can be used to guide a user to a certain part of town.
2) Ask new users if they have a particular preference across a range of keywords... this should narrow the focus of cold start recommendations.
3) We will rerun a content similarity filter with the review text and then a combination of review text and category tags.
4) Bring all the filtering permutations together under one recommender.
5) Ensure access to lesser know (hidden gems) is gained and these form part of the final recommender. 
5) Deploy on a user friendly interface - google streamlit. 




********************************************************************************************************************************


4. Repository Navigation



********************************************************************************************************************************



5. Instructions

Two recommenders are currently deployed with the 'Rec Sys Modelling' jupyter notebook. 
1) Collaborative recommender asks a new user to rate a set number various restaurants and returns the most appropriate alternatives. 

2) Content recommender is currently run through a function. Call the recommender with an argument equal to the name of restaurant form the data set. An appropriate set of n alternative restaurants is returned. 



********************************************************************************************************************************


6. Links

- Surprise prediction algorithm documentation: https://surprise.readthedocs.io/en/stable/prediction_algorithms_package.html

- Presentation: https://docs.google.com/presentation/d/12IsMq6GingPK_GkJOSs8U99t3g5H_SRXGz75lVzg5wo/edit#slide=id.p










To add
link to large files
link to presentation
CRISP-DM flow chart

collaborative prediction table
