<img src="images/pokemon_logo.png?raw=true"/>

### Problem Statement

- How can I create a model to accurately predict which subreddit a Pokémon related post/comment came from? I'll be analyzing the data using NLP to try to find any significant patterns in order to try to increase my model's predictive power. 

# Executive Summary
- Pokémon is such a huge phenomenon now with many different related franchises. Competitive gaming (VGC), Trading Card Game (TCG), Non-Competitive gaming, PokémonGo, Pokémon Mystery Dungeon series, Super Smash Bros characters, and Pokken Tournament just to name a few.
- This model could be a great resource for The Pokémon Company to be able to differentiate between any variety of posts/questions/comments for the many different Pokémon categories available today.


### Description of data
- Source: Original source of data came from two subreddits via the API: Pushshift.io;
https://www.reddit.com/r/CompetitivePokemon/

https://www.reddit.com/r/pokemongo/

<img src="images/wooper.png?raw=true"/>

## Process

### Data Collection
- To collect all of my data from Reddit for this project I used the API Pushshift.io. This is a fairly straight forward API to use and I was able to get all my necessary data by using just a single created function.
The data coming from the API was split up into comments and submissions. I was able to get 1,000 comments and 1,000 submissions from each subreddit for a total of 4,000 data points. Once I had all this data into my jupyter notebook I saved it as four separate dataframes.

### EDA/Pre-Processing
- Once I had all four of my dataframes created I did the necessary cleaning on my data. I needed to combine all four dataframes into one dataframe to be able to work with for modeling, so I needed to update a couple features first. I first checked for any null values and filled in all of those with ' ' values for the post category. For the submissions received from the API, they were split up into the Title and the Body of the post (which they called 'selftext'). I combined the title and selftext columns into a single column named 'body' and then dropped both of them. 
- I then created two additional columns, is_comment and is_competitive. The is_comment was to help differentiate whether the text came from a comment or a post. The is_competitive was my feature column for my model. A 1 designated that the post came from the CompetitivePokemon subreddit and a 0 meant it came from the pokemongo subreddit. 
- The last pre-processing step I did was to convert all my text data into a format that would allow me to run NLP on it. I used RegEx to remove all the punctuation and split every word into individual strings and created a new column, 'tokens'. I then lemmatized and stemmed my data into two new columns: 'lemma' and 'stems'.

### Model Creation
- I had many different models to choose from, but I decided to test out my data with a variety of models to see the difference in my results:
Logistic Regression
Naive Bayes 
Bagged Decision Trees
Random Forests
SVM (Support Vector Machine)
I created functions for each of these models testing on both Count Vectorizer and TF-IDF. I train_test_split all of my data and then used pipelines and a Gridsearch, allowing for multiple hyperparameter tuning for each model.


### Model Fine Tuning
- Once I had created all of my functions for each of my chosen model types I ran multiple iterations of each model, tuning each of the parameters as I went. I ran each model on the three different versions of my reddit data: tokenized data, lemmatized data, and stemmed data. My outputs of each function showed me my training score, test score, and a list of the top model's best parameters. This allowed me to more finely tune my hyperparameters to try to increase all my models' performances.



## Conclusions and Recommendations

Findings/Conclusions:
- The final model that I ended up using was: 
SVM using TF-IDF and Lemmatized Data
Train Score: 99.8% Accuracy
Test Score: 91.8% Accuracy
- This model was my best performing on both my training and testing data. It is overfit to the training data, but I feel with continued improvements/time I could resolve this issue.
- Some of the main takeaways of my models were:
- Test score: 91.8% showed great improvement over Baseline Accuracy Score: 50%
- Of all of the best fit models created, half used English stop-words while other half didn’t use any stop words. I'm not super surprised by this since I was dealing with data that revolved around the world of Pokémon, where many of the names/places mentioned aren't found in the English language.

- What could this improved model score mean for The Pokémon Company?
    - Questions/Suggestions/Comments could be entered in one location on company website and model could sort and send them to correct forum/department. correctly leads to a more confident user base
    - Correct sorting would lead to a more confident user base. Extra user trust could allow increased knowledge sharing between differing Pokemon communities
    - Swelling confidence, trust, and community interaction ultimately should bring about higher revenue! 
