# Analysis and Visualization of NFL Draft Prospects
Project by: Cory Rutkowski

## Problem Statement

Using SQL and supervised learning models can you predict and visualize the NFL draft process for collegiate football players? The NFL Draft has become a huge spectacle every year with millions of dollars on the line for prospective athletes.
I set out to create a model that could be a great resource for either NFL teams or draft eligible athletes to determine what position (round) they might get drafted by.


## Executive Summary:


### Goals:

The goals of my project was to be able to collect a data set of past collegiate football players (Division I - Division III) and see if the player's statistics and physical characteristics could help inform what round in the draft they might get chosen. Then using this data, I wanted to try to visualize any relationships that may be evident regarding the draft process.

### Background:

The NFL Draft is an annual event which serves as the league's most common source of player recruitment. Each team is given a position in the drafting order in reverse order relative to its record in the previous year. Then each team can either select a player or trade their position to another team for other draft positions, player(s), or a combination of both.
There are 7 total rounds with each team getting an opportunity to draft each round. To be eligible for the draft, players must have been out of high school for at least three years and must have used up their college eligibility. The difference between getting drafted in the first round vs the later rounds is tens of millions of dollars in their contracts.

Example:

Number 1 Pick: Oklahoma quarterback Kyler Murray Cardinals: $35,035,000

Number 32 Pick: Arizona State wide receiver K'Neal Harry, Patriots: $10,070,227

### Methodology:

In order to complete my goal I followed several steps. 

First, I located a dataset of college football statistics from Kaggle from 2005 - 2013.

I then selected the relevant .csv files and compiled them all into a PostgreSQL database using a combination of python, pandas, and sqlalchemy.

Once I had my dataset, I then used SQL queries to arrange the data into the necessary form for modeling. I used a multi-class classification model to categorize each player as either not being drafted (0) or being drafted and which round they were drafted into (1-7).

Lastly, I used Tableau to visualize any relationships the model and data had to offer.


### Data:

Data for this project was collected from several different sources. 
My data for training my Multi-Class Classification model was taken from a dataset from Kaggle as mentioned above.
https://www.kaggle.com/mhixon/college-football-statistics
I additionally scraped and collected data for each of the relevant NFL draft years from a website called drafthistory.com.
http://www.drafthistory.com/index.php/years/

Once all of the data was collected and imported into SQL and pandas it was cleaned. The data from the draft site was very clean with very few null values. The data from the Kaggle set had a decent amount of null values, but most were related to features that weren't used in the model.

### Model: 
My modeling process was broken into three main steps. First I created additional engineered features as well as dummy features. Then I created and trained a multi-class classification neural network model using Keras. Lastly I assessed the accuracy of eight predicted classes: 1-7 for each round or 0 for not being drafted.

#### Multi-Class Classification Feed Forward Neural Network Model
My biggest takeaway from my model was:

Imbalanced Classes. Only about 1.5% collegiate athletes succeed and go pro and only ~5% of my dataset contained drafted players (classes 1-7). Therefore, I performed Over-sampling/Bootstrapping to the minority classes in order to assist my model during the training process.


#### Model Scores

My best model looked at only a single year's worth of statistics and had training/testing accuracy scores of 95%/90%. However, the most successful classification was of non-drafted class, which wasn't very surprising due to the class imbalance. Also, it tended to predict the round selection as lower than the true values (ie true value was Round 3 but predicted to be Round 1).


### Visualizations:

Using Tableau I was able to visualize different relationships within the data.
You can check these visual out at: https://public.tableau.com/profile/cory.rutkowski#!/vizhome/home_state_2013/coll_comb

<img src="images/project1_visual.png?raw=true"/>


## Conclusions:


This model could be beneficial in the future for collegiate athletes to try to determine if their current body of work is enough for them to continue their dream of playing in the NFL, or if they should continue to try to improve their skills/stats/awards to try to better improve their draft chances.

### Slides

You can view the presenation slides [here] (https://docs.google.com/presentation/d/1DlI68sgSFOUmQatPLTInHjYeIakYXF0UTzCwsduH4-E/edit?usp=sharing)

### References

- [Draft Results History](http://www.drafthistory.com/index.php/years/)
- [Kaggle Collegiate Football Statistics Dataset](https://www.kaggle.com/mhixon/college-football-statistics)
