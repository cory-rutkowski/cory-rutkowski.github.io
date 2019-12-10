# Leveraging Twitter to Map Natural Disasters

![Twitter logo](https://www.itsnicethat.com/system/files/062012/4fd07cea5c3e3c0d810000db/images_slice_large/Twitternew.jpg?1438264585)


Project by: Garrett Bradley, Ishan Deulkar, and Cory Rutkowski

## Problem Statement

During a Natural Disaster how can we utilize Social Media to help locate specific areas that require immediate assistance?

During any natural disaster one of the first areas of focus should be on rescuing people in need of immediate assistance. However, due to the nature of these events this isn't always easily possible. This can be on account of downed power and phone lines which make communication near impossible at times. We set out to try to utilize social media, specifically Twitter, to see if its users' tweets can better help inform emergency responders of where to focus their rescue efforts.

## Executive Summary


### Goals:

The goals of our project were to be able to collect a large amount of tweets relating to a certain natural disaster (in our specific case, Hurricane Harvey in 2017) and see if tweets could help inform emergency responders of specific areas to focus on during a true emergency situation. The task was to identify tweets that are requesting urgent assistance. The second task then was to extract geographic information from these tweets and then map the location so that emergency responders can find them.


### Methodology:

In order to complete our goal we followed several steps. 
First, we compiled tweets related to Hurricane Harvey during the time period of August 17, 2017 - September 9, 2017 into a pandas DataFrame. These tweets were to be analyzed for emergency requests and then mapped.
We needed to create a model in order to be able to categorize our tweets as either 'Emergency' or 'Non-Emergency' related. We used a pre-made dataset from figure-eight.com that already classified messages into various emergency related categories to train a classifier model. The message data was unlabeled, so we implemented a loop to label messages are 'Emergency' using those emergency related categories and whether or not a message was direct (rather than being news).
Once our model was successfully trained, we ran it on our Hurricane Harvey tweets dataset to pull out all of the 'Emergency' tweets.
Next we used a series of loops and street name lookup to analyze our 'Emergency' related tweets, looking for those related to requests for aid/assistance, to focus on those tweets containing a physical address in the body of the text. We converted the addresses into Latitude and Longitude coordinates using Google geocoder and then visualized each tweet onto a Houston based map. Our goal was for rescue coordiators to be able to use this map to strategize and implement their best tactics on where to focus their rescue efforts.


### Data:

Data for our project was collected from several different sources. 
Our data for training our Emergency Classification model was taken from a dataset from the Multilingual Disaster Response Messages page on the figure-eight.com website.
Our actual testing data used for tweet mapping was collected from two sources:
- Kaggle: This dataset of tweets collected during the time of Hurricane Harvey and arranged into a Kaggle dataset by Kaggle user: Dan (https://www.kaggle.com/dan195)
- NTU: Our other dataset of tweets came from Mark Edward Phillips at the University of North Texas. He collected Hurricane Harvey tweets as well and was kind enough to allow us access to them for the purposes of this project. (https://digital.library.unt.edu/ark:/67531/metadc993940/: accessed January 29, 2018, University of North Texas Libraries, Digital Library,digital.library.unt.edu)

Once all of the data was collected and imported into pandas it was cleaned. There were only ~1% of null values in the dataset, so those were dropped. In checking for duplicate tweets, it was discovered that there were nearly 200,000 duplicate tweets so those were removed as well.
After all of the pre-processing was complete we had a dataset containing 267,682 Tweets.

### Modeling: 
Our modeling process was broken into three main steps. First we had to create a classification model that would classify the message as either 'Emergency' related or'Non-Emergency' related. The second step was to use this classifier to classify the unlabeled twitter data. Once this was accomplished, we then fed the 'Emergency' related tweets into another function to pull out any specific addresses mentioned within the tweet body. These addresses are what we used to be able to map all of the rescue request tweets.

#### Emergency Classification Model
3 different models were created for this project:
1. TfidfVectorizer and Random Forests
2. TfidfVectorizer and Naive Bayes
3. TfidfVectorizer and Logistic Regression

We utilized a GridSearchCV to generate optimal parameters for our models. We excluded english stop-words from our models to decrease noise. We used a lemmatizing function to pass through the vectorizer to focus just on the roots of the words. A parameter was set as well to include not use the idf (CountVectorizer).

Best model features:

1.Random Forests
  - rf__max_depth: 6
  - rf__max_features: None
  - rf__n_estimators: 5
  - tf__max_features: 3000
  - tf__ngram_range: (1, 2),
  - tf__stop_words: 'english'
  - tf__use_idf: True 

2. Naive Bayes
  - nb__alpha: 1
  - tf__max_features: 5000
  - tf__ngram_range: (1, 2)
  - tf__stop_words: 'english'
  - tf__use_idf': False

                
#### Model Scores

Baseline accuracy for the dataset was 81%

*Accuracy is not a definitive metric as the labels are not true but rather generated through the loop*

1. Random Forests - accuracy score of 87%
3. Naive Bayes - accuracy score of 90%

Our best classification model was the Naive Bayes. 

#### A note on the Logistic Regression

The Logistic Regression was trained without a gridsearch in order to generate a coefficient dataframe from the message data. It was not used to predict the labels of the twitter data.

#### Predicting on tweet data

The Naive Bayes and Random Forest classifiers were used to predict the urgency of the unlabeled twitter data. These labels were then saved to the labels section.

#### Address Model

Once we sorted our tweets into ‘Emergency’ related, we then investigated whether the tweets contained physical address components and how to extract this text from the tweet. Using geographic.org’s list of Google street view streets we created a database by scraping all street names for the greater Houston area. Using a combination of methods, we were able to filter the emergency tweets and extract the physical street and street number address from tweets with addresses. We did this through the following:

- Cross referencing tweets against known streets list

- Custom regular expressions & string manipulation

- Pre-built open source address parsing tool: Libpostal 

- Passing partial addresses into multiple geocoders for validation and lat/long generation: Google and Geocodio

Ultimately, the open source address parsing tools were instrumental in extracting enough of the address to pass into the geocoders for successful geo coordinate generation. This allowed us to translate the address into Latitude and Longitude coordinates for mapping.  In the end, we identified 8 emergency tweets from which we were able to pull location data.


### Mapping:

Using all of the Lat. and Long. coordinates we marked them all on a map using Google Maps and the Folium visualization library in python to help notify rescue teams which areas required assistance. 

The interactive map html file can be found in the Mapping subfolder.

Example maps

Zoomed out map of Houston with clusters

![Zoomed out map of Houston with clusters](Images/map_zo.PNG)

Zoomed in map with specific location and tooltip

![Zoomed in map with specific location and tooltip](Images/map_zi.PNG)


### Constraints/Limitations

Twitter’s free API will only allow you to scrape the past 7 days worth of tweets. Location data for  tweets is very hard to find due to the majority of users not turning the feature on as well as Twitter removing this feature recently. This causes a major challenge when trying to come up with user's locations if they don't specify the address directly in the tweet.

For the address verification, address street name length presents a challenge. It is easy to match up single word street names (ie Oak St or Jade Blvd) but names with more than two words or a cardinal direction tended to cause additional problems for our functions (ie Palm Harbor Drive or E Oak Street).


### Conclusions and Recommendations:

Based on our results this project is something that could be implemented into future rescue operations with some additional added features/changes. For different locations affected by a natural disaster, we would need to either have beforehand, or soon after the disaster occurs, create a database of city street names for our function to reference.

### Slides

You can view the [presentation slides](https://docs.google.com/presentation/d/10ZApvLVwmNZgL5VeSmSjSGsa6tB28ApVWLPaOXeqW7s/edit?usp=sharing)

### References

- [Geolocational Tweet Information](https://codete.com/blog/observing-world-tweeting-tendencies-in-real-time-part-2/)
- [Twitter Removing Geolocational Information as a Feature](https://www.niemanlab.org/2019/06/twitter-is-turning-off-location-data-on-tweets-a-small-win-for-privacy-but-a-small-loss-for-journalists-and-researchers/)
- [Kaggle Hurricane Harvey Tweets Dataset](https://www.kaggle.com/dan195/hurricaneharvey)
- [NTU Hurricane Harvey Tweets Dataset](Phillips, Mark Edward. Hurricane Harvey Twitter Dataset, dataset, 2017-08-18/2017-09-22; https://digital.library.unt.edu/ark:/67531/metadc993940/: accessed January 29, 2018, University of North Texas Libraries, Digital Library,digital.library.unt.edu)
- [Model Training Dataset](https://www.figure-eight.com/dataset/combined-disaster-response-data/)
- [Libpostal Address Parsing Library](https://github.com/openvenues/pypostal)
