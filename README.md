# Power Outage Prediction

by Jiaqing Yan(j6yan@ucsd.edu), Shiyuan Wang(shw088@ucsd.edu)

Our exploratory data analysis on this dataset can be found [here](https://kayden-yan.github.io/power-outage-discovery/).

The data came from [here](https://engineering.purdue.edu/LASCI/research-data/outages/outagerisks).

---

## Framing the Problem
Our goal is to predict the duration of power outage based on features such as month, cause of the outage, anomaly level and start time, which are all information that could be accessed before the happening of each outage. Note that we can know the cause of the power outage by using many resources, for example, news reports regarding extreme weathers, the date of last time staff have maintained the equipment, or the live footage of people trespassing areas. The anomaly level can be estimated by the local climate conditions reported by reliable resources like weather reports. This is a regression problem. The response variable we choose is the duration of outage in minutes, because predicting outage duration could give people instructional information about approximately how much they need to wait before the power restores so that they could adjust their schedule accordingly to minimize the inconvenience brought by the outage. The metric we are using is rooted mean squared error (RMSE). We choose RMSE because it is a widely accepted metric with mathematical properties suitable for statistical analysis, and it provides direct information about the difference between predicted and actual data. We preferred RMSE over MSE because RMSE is on the same scale as the target variable compared to MSE, which makes interpreting the performance of the model more easy.

---

## Baseline Model

Our baseline model is a linear regression model that uses features of month and outage start time (in hour). Month and outrage start time are one-hot encoded categorical features in our case. For encodings, we converted the unit of start time to hour and then one-hot encoded it along with month. Our baseline model presented a training set RMSE of 4138.77 and testing set RMSE of 4747.35. It is worth noting that the training set RMSE is similar to testing set RMSE, suggesting that there is no over-fitting in our baseline model. We believe its performance is not decent enough, since the RMSE value is pretty high given that the average outage duration in the dataset is 2845.51.

---

## Final Model

For our final model, we engineered and added 2 new features – cause of the outage and anomaly level. Anomaly level represents the oceanic El Niño/La Niña (ONI) index referring to the cold and warm episodes by season, and it could provide information about local climate conditions when the outages happen. Such climate information is useful since it typically takes longer to repair when the weather is extreme. Knowing what causes the outage could also be useful, since some causes of the outage often take longer for it to be repaired, such as severe weather and equipment failure. The modeling algorithm we chose is a random forest regressor, which combines multiple decision trees to perform regression tasks. The best hyperparameters are random_max_depth = 10 and randomforest_n_estimators = 50. The method we used to select the hyperparameters and the overall model is grid search with 5 folds. The final model performs better than the baseline model with an training RMSE of 2495.57, which is 1643.20 less than the baseline model. There might be some over-fitting in our final model because our testing RMSE is 4295.33, we tried many combinations of features and we conclude that the difference is due to the dataset doesn't contain a large enough or diversed enough dataset.

<iframe src="assets/final_model_one.html" width=800 height=600 frameBorder=0></iframe>

<iframe src="assets/final_model_two.html" width=800 height=600 frameBorder=0></iframe>

---

## Fairness Analysis

Our choice of group X and Y are outages in the mornings and outages in the evenings, respectively. Our evaluation metric is mean squared error. Our null hypothesis is that the performance of our model stays the same between morning and evening, while our alternative hypothesis is that the performance of our model is different between morning and evening. Our choice of test statistic is the absolute difference between the RMSE of the duration predictions of outages in the mornings and the RMSE of the ones in the evening. We choose significance level is 0.05. 

<iframe src="assets/fairness.html" width=800 height=600 frameBorder=0></iframe>

The resulting p-value is 0.468, so our conclusion is that we fail to reject the null hypothesis, which means that our model performance is consistent across morning and evening.


