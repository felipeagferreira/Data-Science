# Echocardiogram Survival Prediction


This project is to analyze the echocardiogram dataset https://archive.ics.uci.edu/ml/datasets/echocardiogram to build models that predicts whether a patient has survived or not for at least 1 year. 

https://www.kaggle.com/loganalive/echocardiogram-uci/code?datasetId=23300

## Exploratory Data Analysis

I dropped the meaningless ’name’, ’group’ and ’alive-at-1’ columns because they are less useful to predict whether a patient has survived or not after having their heart attack. Remembering that the patients had their heart attacks at different times and some of them have survived less than one year but are still alive, these data are regarded as censored data because these patients were alive during the data collection period but we do not know their survival months after the data was collected. These censored data were removed from the data frame as such patients cannot be used for the prediction of the model. I checked which columns has null values or anomalies and then split the set of columns into discrete and continuous features in such a way that one can fill the NaN values with median() for the columns that has outliers, and fill the others with mean() for the columns that doesn’t have outliers. The continuous features were also normalized leaving their values in the same scale to facilitate the learning process of the models.


## Target Variable Specification

In the dataset there is only 1 patient that survived for at least 2 years and is still alive. Since this is not enough to train a machine learning model, I started focusing on building models that predict survivals of at least 1 year and thus used "still-alive" as the target variable without making any transformations. The labels of this variable are
highly imbalanced, which required me to use the SMOTE oversampling technique to reduce the imbalance and then improve the training steps of the prediction models.


## Model Construction

After cleaning and preparing the data for the models, I built binary classification models based on the following algorithms: Logistic Regression, Support Vector Machine, Decision Tree Classifier and Random Forest Classifier, besides others which were not better. I choose these algorithms because they are useful for supervised learning
classification problems. After doing some hyperparameter tuning with GridSearchCV technique, the models with better AUC were Decision Tree Random Forest Classifiers with certain parameters. The former one was the starting point because it is faster and easier to be interpreted than the later one for problems with smaller dataset. Whereas, the Random Forest algorithm was used because it can be better in building highly accurate predictions without suffering from overfitting. More details of the results can be seen in my code file. In this project it is important to compare the true positive rate (sensitivity) with the false positive rate (1 - specificity) because here one shall not focus only on the positive class, but also focus on the probability of false predictions. Hence, the best classification metrics to use here are sensitivity, specificity and mainly AUC. Therefore, I used the area under curve score to compare the models. In order to find the best features for the model, I did a Recursive Feature Elimination with cross validation to see what is the best number of optimal features to improve the model performance and compare their importances.


## Model Test

In this part I applied the same data cleaning steps that was done on the ’echocardiogram.data’ on the ’echocardiogram.test’ dataset. Since in this data file the ’survival’ and ’still-alive’ are all null columns and we saw in our Feature Importance Calculation section that the former one is the less important feature that contributes to obtain a better AUC, we removed it before applying our best models to the ’echocardiogram.test’ file. After doing this and applying our best models on the test dataset, we saw some similarities between our predicted results with the actual results, such as the smaller the fractional-shortening higher is the patient’s chance to survive. If I were given the opportunity to add additional features to this dataset, I would add certain medical features (numerical or categorical) that give deeper insight of the patient’s health state related to its heart attack. A good example could be, for example, a feature that indicates the distance between the parietal pericardium and visceral pericardium where the smaller the distance bigger is the patient’s chance of survival.


