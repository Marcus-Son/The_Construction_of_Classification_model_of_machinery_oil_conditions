# Construction of a Classification Model for Machinery Oil Conditions

## Table of Contents
1. [Background](#background)
2. [Project Objective](#project-objective)
3. [Data Description](#data-description)
4. [Experimental Method](#experimental-method)
5. [Results](#results)
6. [Conclusion](#conclusion)

## Background
Lubricating oil is critical for the efficient functioning of machinery. It is used not only in production equipment but also in various parts of transportation vehicles like cars, trains, ships, and airplanes. The role of lubricating oil is to reduce wear and tear on machinery components, thereby prolonging the equipment's lifespan. Poor-quality oil, on the other hand, can cause significant damage and reduce the machinery’s lifespan. As a result, it is vital to detect and replace low-quality oil before it causes damage.

## Project Objective
This project aims to develop a real-time lubricating oil quality assessment model for construction equipment. By doing so, the model will classify lubricating oil conditions, detecting poor-quality oil to prevent mechanical failures.

## Data Description
- **Train Data**: 53 features are available for training, excluding ID variables.
- **Test Data**: A subset of 18 features is used for evaluation.
- **Class Imbalance**: The data is highly imbalanced with a ratio of 8.54% poor-quality oil to 91.46% good-quality oil.

The full details of the dataset, including feature descriptions, can be found in the data folder provided.

### Data Splitting
The training data was split into three sets:
- **Training Set**: 64%
- **Validation Set**: 16%
- **Test Set**: 20%

## Experimental Method

### Step 1: Data Preprocessing
- **Variable Exclusion**: Removed irrelevant variables like ID, Component_ARBITRARY (string data), and Year. 
- **Missing Data Handling**: Removed columns with more than 15% missing values.
- **Feature Scaling**: MinMaxScaler was used instead of StandardScaler due to the non-normal distribution of the data.

### Step 2: Addressing Class Imbalance
Given the severe class imbalance, the following techniques were applied:
- **Class Weights**: Adjusted for imbalanced data.
- **Sampling Techniques**: Tested four sampling methods: SMOTE, SMOTE-Tomek, ADASYN, and Random Undersampling.

### Step 3: Model Development
- **Teacher Model**: Built using LightGBMClassifier.
- **Student Model**: Built using LightGBMRegressor and applied Knowledge Distillation.

Both models were trained with the same parameters to ensure a fair comparison.

### Step 4: Model Evaluation
The models were evaluated using the Macro F-1 Score. The most effective approach was applying class weights without sampling techniques.

- **Best Validation Score**: Achieved with a threshold of 0.35 using class weights only.
- **Best Test Score**: Achieved with a threshold of 0.3 using class weights only.

## Results
The LightGBMClassifier model, using class weights, achieved the highest Macro F-1 Score, outperforming other models that incorporated sampling techniques.

## Conclusion
Using only class weights without additional sampling techniques yielded the best classification results for lubricating oil quality. Knowledge Distillation was successfully applied to handle the disparity in feature sets between the training and testing data.
