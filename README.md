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

Funder, Installer, Subvillage, WPT Name:

After examining the data in these columns, which we noticed were caterogical data types, we quickly realized that the sheer number of unique values each of these predictors contained. In not wanting to over pollute our data set with 2000+ new columns of one hot encoded data, we decided to drop these columns given we weren't able to find a good use for the data contained inside. It's certainly worth noting at this point in time that outside of funder, which was a unique column, the other three columns had **very** similar data inside that was much more useful for our model. An example of this would be scheme management, scheme name and management.

Large number of Repititive data sources:

Like we mentioned above, there were a large number of very repitive columns across our data. To avoid feeding our model with highly correlated items, we dropped the following columns:

1. scheme name
2. scheme management
3. waterpoint type
4. source
5. source class
6. water quality
7. payment
8. quantity
9. management group
10. extraction type
11. extraction type class
12. ward
13. lga

Additionally, there were data points that we viewed would be useless to our model so we decided to drop them altogether. Some of these data points didn't offer what they were pertained to/had no description or would have little to no impact on the model's outcome.

1. public meeting
2. date recorded
3. recorded by
4. num private

This left us with a solid 18 features and ~54k rows of data to begin performing our data anaylsis on. 



## Modeling

Baseline Model - Dummy Regressor

The first step in doing our analysis here was to create a baseline model using SKLearn's DummyClassifier model. This model predicts the target variable based on the mean, which gave us a pretty poor accuracy score of about 44%. This performed slightly worse than 50% because we were trying to predict a ternary model however was close to 50% given the data was heavily imbalanced.

![DummyMatrixProject3](https://user-images.githubusercontent.com/92402366/145499638-14d6474a-4cfd-4901-b984-c011a21d36ba.png)
![dummyroc](https://user-images.githubusercontent.com/92402366/145502511-c009b353-b9a1-4acc-a7ca-30f7da4df39b.png)

### Logistic Regression

Our next model we chose to run was the Logistic Regression model from the SKLearn library. This model certainly improved our score from our baseline however we were confident we could do better. This model struggled greatly to predict wells that were functional however needed repair. This was due to significant class imbalances in that specific class, and despite trying to add weights into, the model seemed to perform best without hypertuned parameters.

![logisticmatrix](https://user-images.githubusercontent.com/92402366/145499888-d8931667-628f-4d2a-98ee-669c2d25cf18.png)
![logroc](https://user-images.githubusercontent.com/92402366/145502519-9e70e3c0-46c7-4840-b6b1-171763dc09a1.png)


### Decision Trees
The following model was that off Decision Tree. Decision Trees when not tuned will tend to overfit the training data. We will get a high score on the training data but when applied to the unseen test data it showed low performance. After running a grid search we found our optimal parameters for our decision tree
* max_depth=30 
* min_impurity_split=0.3
*  min_samples_split= 2

![DecisionTreeMatrix](https://user-images.githubusercontent.com/92402366/145500344-f1f0766b-b19f-4f5a-a3b9-ff4e1c7affe0.png)
![decisiontreeroc](https://user-images.githubusercontent.com/92402366/145502553-08693893-c815-4588-8c6b-b9ea5fff2147.png)


### k Nearest Neighbors

After decision tree, we then decided to try running a k Nearest Neighbor test to see if that model was better suited for our dataset of Tanzanian wells. Not to much surprise, this model outperformed all other models that we had run previously. We ran 3 different KNN models after using gridsearch trying to determine what the best number of neighbors was; we used a rather large range on our first gridsearch to which we found the optimal amount to be 11. We ran another gridsearch of odd values between 1 and 11 after if there was a better optimzied number of neighbors to which we found 7. 

![KNNMatrix](https://user-images.githubusercontent.com/92402366/145500080-ac0a9a7c-39f5-4ef3-bf33-569e6754d91f.png)
![knnroc](https://user-images.githubusercontent.com/92402366/145502526-75ac193a-af55-41fd-88e7-ea86c1409424.png)
 
### Random Forest
We finally come to our ending model that had the greatest prediction accuracy, Random Forest Classifier. Random Forest Classifier uses a bagging techique that breaks up a combination of n rows and m columns from the dataset into different bags to generate decision trees that predicts an outcome. The combination of rows and columns are unique per decision tree. This doesn't mean that once a row or column has been used in one tree that it can't be used in another. Overlap is allowed. Then the random forest classifier will then take the majority prediction value that was generated by the various Decision Trees as the final prediction for that point. After hypertuning our model we found we just had to change one thing from all the default parameters and that was max_depth and change it to 23. We ended with an accuracy score of 80%

![RandomForestMatrix](https://user-images.githubusercontent.com/92402366/145500496-e6795759-928f-41bf-8796-490369659aea.png)
![randomforestroc](https://user-images.githubusercontent.com/92402366/145502544-a4853c29-e975-4bab-b653-b3518248640c.png)



## Next Steps & Conclusion


Our best model ended up being the Random Forest model, which gave us an accuracy rating of 80% as you can see above. When our model was placed against the test data, we can see by the heatmaps below that we ended up being able to predict the regions in which wells needed repairs. This is incredibly helpful in helping guide the Tanzanian goverment's various water divisions in target certain areas to focus.

Actual Well Statuses:

<img width="921" alt="y_true_regions" src="https://user-images.githubusercontent.com/92397698/145596340-e1fff916-182d-42d9-b767-790f6dea4e0f.png">

Our Predictions on training data:

<img width="935" alt="y_pred_regions" src="https://user-images.githubusercontent.com/92397698/145596080-720d4851-4364-49b5-809e-6b1f1acd5fce.png">

Based on the above heatmaps, we can see that our predictions on the training data looked to be greatly inline with the actual statuses of the well. Most importantly given the crisis we're trying to help fix and the implications on Tanzanians, we were able to predic the two statuses of the Mtwara and Lindi regions, which had the highest concentration of wells that were non-functional. 

We then ran our model on roughly 15,000 wells that we didn't know the statuses for. As you can see from our map below, these undocumented wells seem to follow a very similar distribution to both prior maps. It seems that the same regions and different general zones across Tanzania follow the same concentration of well statuses. This is in-line with what we'd expect given that we found geographical features, as James mentioned earlier, to be the highest contributors in all of our models. 

<img width="920" alt="y_unknown_pred" src="https://user-images.githubusercontent.com/92397698/145600716-ec17d6eb-5421-419f-92b5-151c77feaa3b.png">


Based on our model, we’d love to continue to partner with the Tanzanian Ministry of Water on a quarterly basis to focus funding and aid to repairing wells in regions with the highest density of non-functional wells. We dove a bit further to see if there were any regions that matched both high density of non-functional wells as well as areas that were highly populated. One example of this is the city of Mtwara in the Mtwara region which is region located in the most southeast corner of Tanzania. Conversely and something our group found extremely interesting was that the Lindi region, which is just north of Mtwara has the lowest population density however one the highest density of non-functional wells. While it’s hard to make a decision like this, we’d certainly implore the Tanzanian government to focus immediate efforts in remediating issues in the Mtwara region before exploring and helping the Lindi region next. By doing so, you’d effectively resolve the water crisis for ~500k additional people that are in a much more concentrated area. 

In conclusion and while it’s hard to make a decision like this, we’d certainly implore the Tanzanian government to focus immediate efforts in remediating issues in the Mtwara region before exploring and helping the Lindi region next. By doing so, you’d effectively resolve the water crisis for ~500k additional people that are in a much more concentrated area. We'd also like to continue working together in tandem on a quarterly basis to continue our upkeeping of wells across tanzania so we can tackle this water crisis, which has spanned over 20 years, altogether.

Navigating our Repo:

Final Notebook: this contains our final clean notebook explaining how we filtered throughout different models, cleaned our data as well as a variety of scores, confusion matrices as well graphs of the performances of our final model. 

Personal_notebooks: all of our data cleaning and original EDA can be found in our personal notebooks. This includes model creation as well as different graphs and extensive gridsearching to find optimal models.

Data: this folder contains various .csv files we used for this project as well as external .geojson files we used to create heatmaps of tanzania.


External Links:

[Presentation](https://docs.google.com/presentation/d/1Mj393_kqG_fkaspYlm7KJoTdvxPBpcY-81k4w0ZfYyQ/edit?usp=sharing)

[Data Source](https://www.drivendata.org/competitions/7/pump-it-up-data-mining-the-water-table/page/25/)

[Tanzania GEOJSON](https://gadm.org/download_country.html)







