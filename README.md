# Construction of a Classification model for machinery oil conditions

## Background

Lubricating oil is necessary for all machinery. It is used not only in the production equipment used in factories but also in various parts and machinery components of transportation vehicles such as cars, trains, ships, and airplanes. Using lubricating oil in machinery helps prevent wear and tear. Therefore, using high-quality lubricating oil can extend the lifespan of machinery, while using poor-quality lubricating oil can shorten it. It is essential to detect and replace lubricating oil with poor quality. Hence, it is necessary to develop a lubricating oil quality classification model that can effectively detect poor-quality lubricating oil.

## Topic

Development of a lubricating oil quality assessment model for real-time monitoring of lubricating oil conditions in construction equipment.

## Data Description

During model training, all 53 features except the given ID variables can be used. However, during the diagnostic test, only a subset of 18 features, excluding the ID variables, can be used. In this Dacon competition, the Y_LABEL variable in the test data is not provided, so the train data is divided into train data, validation data, and test data in a ratio of 6.4:1.6:2. The data classes have a severe class imbalance, with a ratio of 8.54:91.46 between cases where the lubricating oil quality is poor and cases where the lubricating oil quality is good.

Please refer to the data description document in the data folder for detailed definitions and meanings of each variable.

## Experimental Method

Due to the severe class imbalance in the data, class weights and four sampling techniques (SMOTE, SMOTE-Tomek, ADASYN, Random Undersampling) were applied. Additionally, a unique aspect of this competition is that the number of variables in the train and test data differs. To address this issue, Knowledge Distillation was applied.

Process: Step 1: Data Load and Simple Preprocessing

Use only the train.csv data for the process. Remove ID variables and Component_ARBITRARY, which are string data. Remove the Year variable (Year represents the diagnostic year and does not affect the lubricating oil quality). Remove columns with more than 15% missing values. Split the train data into train data, validation data, and test data in a ratio of 6.4:1.6:2. Exclude 16 test data variables (ID, Component_ARBITRARY, Year) and the AL variable, which is suitable for classifying lubricating oil quality, from the train data. Use MinMaxScaler instead of StandardScaler due to the data not following a normal distribution.

Step 2: Address Class Imbalance Issue with Class Weights and Sampling Techniques (as shown in the provided table)

Step 3: Create a Teacher Model using LightGBMClassifier Set the parameters of the LightGBMClassifier model the same for each case to facilitate comparison.

Step 4: Create a Student Model using LightGBMRegressor Set the parameters of the LightGBMRegressor model the same for each case to facilitate comparison.

Step 5: Compare the Macro F-1 Score values of the models for each case (as shown in the provided image)


- Applying class weights without using sampling techniques was the most effective approach.
- The highest Macro F-1 Score was achieved with a threshold of 0.35 and only using class weights for the validation data. For the test data, the highest Macro F-1 Score was achieved with a threshold of 0.3.


