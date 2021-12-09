# Tanzanian_Water_Wells_Status

## Overview
This project uses classificational modeling to accurately predict the current state of water wells that are spread throughout the African nation of Tanzania. The purpose for the prediction model is to aid The Tanzanian Water Project organization on finding water wells that are not functioning for them to go in and  repair as wll as to give indicators on why the well is currently non-functional.

We'll be focusing our efforts on a data set provided by the Tanzanian Ministry of Water. This data set, before cleaning, had roughly 60k data points as well as over 30 features for us to use in an effort to predict the status. 

The steps we'll be taking so solve this problem will be:
* Fully grasping the problem at hand in Tanzania
* Explore and scrub our data
* Use various classification machine learning models
* Anaylyze results and hypertune our models for peak performance


## Business Understanding
 
Currently in Tanzania, there is a massive lack for almost half of their population with access to source of safe drinking water or improved sanitation areas. Already posing an incredibly tough challenege, various organizations across the world have spent hundreds of millions of dollars to try and help tackle this problem from the early 2000s when Tanzania came under incredibly large scrutiny to fix water crisis. Being an issue back then when 73% of locals didn't have any access to drinkable water, we can see we've made decent strides in trying to tackle this problem with the introduction of water wells across the various districts in Tanzania. Unfortunately, due to many different problems such as geographic location of the well, extraction techniques, installation techniques, and the group that manages the well, we're beginning to see that many of these wells require some tender love and care. 

Our goal here is help the Tanzanian government, who is responsible for funding the majority of the wells across their country, to identify the statuses of failing wells in the most highly concentrated areas to ensure that the correct development teams can help get locals access to safe and drinkable water again. 

#### Map of Tanzania and locations showing functional/needswork/non-functional wells(if kept as ternary)


## Data Cleaning

When we first started to take a look at the data set which was given to us in a relatively clean state, contained a few different columns that contained NULL values or values we decided to drop based on our understanding. These columns were the following and we'll explain our approach to each column as to why we took the action we did. 

1. funder
2. installer
3. subvillage
4. public meeting
5. scheme management
6. scheme name
7. permit
8. wpt name

Funder, Installer, Subvillage, WPT Name

After examining the data in these columns, which we noticed were caterogical data types, we quickly realized that the sheer number of unique values each of these predictors contained. In not wanting to over pollute our data set with 2000+ new columns of one hot encoded data, we decided to drop these columns given we weren't able to find a good use for the data contained inside. It's certainly worth noting at this point in time that outside of funder, which was a unique column, the other three columns had **very** similar data inside that was much more useful for our model. An example of this would be scheme_management, scheme_name and management.




## EDA

## Modeling
### K-Nearest Neighbors

### Decision Trees
### Losgistic Regression
### Rain Forest(Possibly)
### Naive Bayes(Possibly)
##### sort in acsending model performance with best model being final



## Evaluation












