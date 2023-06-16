# Power Outage Prediction

by Jiaqing Yan(j6yan@ucsd.edu), Shiyuan Wang(shw088@ucsd.edu)

Our exploratory data analysis on this dataset can be found [here](https://kayden-yan.github.io/power-outage-discovery/).

---

## Framing the Problem
Our goal is to predict the duration of power outage based on features such as month, outage category, anomaly level and start time. This is a regression problem. The response variable we choose is the duration of outage in minutes, because predicting outage duration could give people instructional information about approximately how much they need to wait before the power restores so that they could adjust their schedule accordingly to minimize the inconvenience brought by the outage. The metric we are using is rooted mean squared error (RMSE). We choose RMSE because it is a widely accepted metric with mathematical properties suitable for statistical analysis, and it provides direct information about the difference between predicted and actual data. We preferred RMSE over MSE because RMSE is on the same scale as the target variable compared to MSE, which makes interpreting the performance of the model more easy.

---

## Baseline Model

Our baseline model is a linear regression model that uses features of month, category, anomaly level and outage start time (in hour). Month, category, and outage start time are one-hot encoded categorical features in our case, while the anomaly level is quantitative. For encodings, we converted the unit of start time to hour and then one-hot encoded it along with month and category. Our baseline model on training set presented a RMSE of 5522 and testing set presented a RMSE of 1221. We believe its performance is not decent enough, since the RMSE value is reasonably low so it should give an relatively reliable prediction. It is also worth noting that our training set RMSE and testing set RMSE are similar, suggesting that there seems no over-fitting to the training data in our baseline model. 

---

## Final Model

From initial observation, we noticed that the OUTAGE.DURATION is either significantly shorter or longer when the CUSTOMERS.AFFECTED is missing. So we speculate that the extreme cases of OUTAGE.DURATION can leads to the missingness of CUSTOMERS.AFFECTED, such that CUSTOMERS.AFFECTED is NMAR.

We conduct permutation test to see if the missingness of CUSTOMERS.AFFECTED depends on OUTAGE.DURATION.

Null Hypothesis: The mean outage duration of missing CUSTOMERS.AFFECTED equals the mean outage duration of not missing CUSTOMERS.AFFECTED.

Alternative Hypothesis: The mean outage duration of missing CUSTOMERS.AFFECTED is smaller than the mean outage duration of not missing CUSTOMERS.AFFECTED.

We use mean difference between missing and not missing as our test statistic.

<iframe src="assets/final_model_one.html" width=800 height=600 frameBorder=0></iframe>

<iframe src="assets/final_model_two.html" width=800 height=600 frameBorder=0></iframe>

---

## Fairness Analysis

Our choice of group X and Y are outages in the mornings and outages in the evenings, respectively. Our evaluation metric is mean squared error. Our null hypothesis is that the performance of our model stays the same between morning and evening, while our alternative hypothesis is that the performance of our model is different between morning and evening. Our choice of test statistic is the absolute difference between the RMSE of the duration predictions of outages in the mornings and the RMSE of the ones in the evening. We choose significance level is 0.05. 

<iframe src="assets/fairness.html" width=800 height=600 frameBorder=0></iframe>

The resulting p-value is 0.468, so our conclusion is that we fail to reject the null hypothesis, which means that our model performance is consistent across morning and evening.


