# Predicting-Energy-Meter
Many companies and organizations are improving the efficiency of their buildings to reduce energy use. When doing this, there is a needs criteria for evaluating the efficiency improvements. Current methods require models specific to the area and building being worked on. By using machine learning, our research targets the ability to predict the energy use of any building based on energy usage history.
Predicting Energy Meter Readings
1st George Liu
California State University, Sacramento
Abstract—Many companies and organizations are improving the efficiency of their buildings to reduce energy use. When doing this, there is a needs criteria for evaluating the efficiency improvements. Current methods require models specific to the area and building being worked on. By using machine learning, our research targets the ability to predict the energy use of any building based on energy usage history.
Index Terms—energy, green, machine learning I. INTRODUCTION
Whether it is for marketing purposes or financial purposes, going green is on many organizations’ minds. One of the methods is to improve the efficiency of buildings to reduce energy use. Even though doing this is quite common, the methods to evaluate the actual improvement leaves a lot to be desired.
There are many things to consider when predicting energy use. Energy use is measured using a variety of methods includ- ing electric, chilled water, steam, and hot water. The weather has a great effect on the usage of energy. The difference in the buildings themselves also contribute.
Many past attempts of predicting energy use predict it based on short term forecasts and with a single type of energy meter or single type of building [2]. Many methods were also used, including statistical and machine learning methods [3] [4].
In this paper, we will use machine learning to predict the energy use of a variety of building based on past weather, building, and energy readings.
II. THE DATA
The data for this project was sourced from Kaggle [1].
A. Data Explanation
1) Building id: The building index used to index the data.
2) site id: This is the index of the site of the building. Multiple buildings share the same site, though some sites only have one building associated with it. Because it is highly correlated with the Building ID, this is not a useful training feature. It is discarded after using it as an index to merge the separate data sets.
3) Meter: The meter type of the reading. The four types in this data are electric, chilled water, steam, and hot water. There are no missing data and it is negatively correlated to the air temperature.
4) Meter Reading: The meter reading value. It is the target variable. It is measured in units of kWh, with the exception in the case of site 0 which is measured in kBTU.
2nd James Douglas
California State University, Sacramento
 Fig. 1.
Correlation of features
 Fig. 2. Correlation of features

5) Year Built: Year built is the year in which the building was built. More than 50% of the data is missing. There is also no easy way to impute the data.In addition, there is a clear outlier in the there are many more building built in 1976 than other years. If we wanted to use it, we would have converted it to how many years since it was built, since older building are less efficient.
Fig. 3. Analysis of Year Built
6) Timestamp: The time when the measurement was taken.
7) Square Feet: This is the total square area of the building. The square area of the building is correlated to the energy use. There are no missing values for this feature.
8) Floor Count: The number of floors in the building. The proportion of buildings missing this feature is a very high 75%. In addition, this feature is implied in the square feet feature as that includes all the area of all the floors. In addition, it is missing for entire building types.
9) Cloud Coverage: The cloud coverage is measured in Okta. The range is from 0 to 9 with 0 being complete clear and 8 being completely cloudy with 9 being reserved for sky being obstructed from view. This is negatively correlated to energy usage.
10) Precipitation Depth 1 Hour: The depth of liquid pre- cipitation that would cover an area in an hour if it did not drain or evaporate. More than 75% of the data has a value of 0. Even with a log transform, it is not a very strong feature.
11) Air Temperature: The ambient temperature of the site. This feature is highly negatively correlated to the energy usage. 12) Wind Speed: The speed of the wind. It is lightly
correlated.
13) Wind Direction: The direction of the wind. It is lightly
correlated.
14) Sea Level Pressure: The sea level pressure. It is lightly
correlated.
15) Dew Temperature: The temperature where the water
vapor condenses into a liquid. it is somewhat correlated.
B. Data Tranformation
1) Imputation: To streamline this though process it is useful to know the 3 categories in which missing data can be classified into:
• Missing Completely at Random (MCAR) • Missing at Random (MAR)
• Missing Not at Random (MNAR)
We imputed the missing data with the mean values.
2) Data Processing: We split the data with 70% of the data for training and 30% for testing. Using standard scaler, we scaled the data. This is required as many machine learning models assume features are centered at 0 with similar variance between the features.
III. MODELS
A. Linear Regression
1) Procedure: A basic linear regression was chosen as a
baseline for the simplest of predictions.
Results
Mean Squared Error 0.870
B. Ridge Regression
1) Procedure: Using Ridge regression, we first use grid search on a variety of alpha parameters. We then calculate the root mean squared error of the predicted output compared to actual output.
2) Tuned Hyper Parameters:
• Alpha: The regularization strength parameter. This re- duces variance. The larger the value, the stronger the regularization.
Ridge Regression Parameters
      Parameters
Alpha
Values
0.600, 0.69, 0.699, 0.7, 0.705, 0.777, 0.799, 0.8, 0.889, 0.899, 0.9
   Results
Mean Squared Error
0.865
  C. Multi-layer Perceptron Regression
1) Procedure: Multi-layer Perceptron (MLP) is a feed- forward type Artificial Neural Network. It is composed of perceptrons, which are linear classifiers. MLP consists of at minimum an input layer, a hidden layer, and an output layer. In MLP regression, the output layer has no activation function so the output values are continuous. In this project, we use the the Sklearn function MLPRegressor.
Using Grid search, we tune the hyper parameters alpha, hideen layer size, and activation function.
2) Tuned Hyper Parameters:
• Alpha: Penalty parameter
• Activation Function: The function used in the hidden
layer.

 Parameters
Alpha Activation Function
Values
0.001, 0.005, 0.0075, 0.008, 0.009 relu
Multi-layer Perceptron Parameters
[4] H. Zhao and F. Magoules, “Energy consumption prediction of air- conditioning systems in buildings by selecting similar days based on combined weights,” in Renewable and Sustainable Energy Reviews, vol. 16, Issue 6, pp. 3586-3592.
     Results
 Root Mean Squared Error
D. Light Gradient Boosting Model
0.732
 Gradient boosting is machine learning model which pro- duces a prediction in the form of decision trees. It will create an initial decision tree, and then generate subsequent decision trees based on the gradients in the loss function. This model has been successfully used in many other applications.
1) Tuned Hyper Parameters:
• Max Depth: This will limit tree depth
• Number of leaves: Main parameter that determine model
complexity.
• Learning Rate: The learning rate
• Minimum Sample Split: The samples needed before split-
ting an internal node
• Num Iterations: Number of iterations before stopping
Light Gradient Boosting Model Parameters
Parameters Values
Max Depth 8 Number of leaves 77
Learning Rate 0.09 Minimum Sample Split 700 Number of Iterations 3500
         Results
Root Mean Squared Error
IV. CONCLUSION
0.291
  In this paper, we predicted the energy use of buildings based on past data on the building, weather, and energy usage. We predicted the output using 4 models: Linear Regression, Ridge Regression, Multi-layer perceptron Regression, and Light Gra- dient Boosting Model Regression. We used grid search to tune the hyper parameters. The model with the lowest RSME was Light Gradient Boosting Model. However, even though it is the best model that we found, it is not accurate enough to be used in production. Continuing research could help develop better models in the future.
REFERENCES
[1] ASHRAE, 2019. “ASHRAE - Great Energy Predictor III,” https://www.kaggle.com/c/ashrae-energy-prediction. Accessed: 2019- 10-01
[2] H. Nguyen Y. Makino, ”Short-term prediction of energy consumption of air conditioners based on weather forecast,” in 4th NAFOSTED Conference on Information and Computer Science, 2017.
[3] Z. Ma and J. Song, “Energy consumption prediction of air-conditioning systems in buildings by selecting similar days based on combined weights,” in Energy and Buildings, vol. 151, pp. 157-166.
