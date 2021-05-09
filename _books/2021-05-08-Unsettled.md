---
layout: default
title: "Unsettled: What Climate Science Tells Us, What It Doesn’t, and Why It Matters"
date: 2021-05-08
categories: Books
---
## Review
One of the important concepts in building the regression model is a *prediction interval*. Often the prediction interval is confused with a similar concept, a *confidence interval*. 
<!--more-->
<img src="/books/unsettled.jpg">
A confidence interval is typically associated with a coefficient in a regression model. It tells us the uncertainty in a coefficient of a regression model. A confidence interval depends on many factors such as training sample size, and correlation between features. 

Unlike the confidence interval, a prediction interval is associated with a individual data point and the prediction made by the regression model. So let's say we trained a model using a training sample, and we made a prediction on a particular data point. There must be an intrinsic uncertainty associated with this prediction due to the variance in data. However, the instability of the model also contributes to the uncertainty in prediction. i.e. if we repeat fit/predict using the same dataset, we might get a different prediction.

A prediction interval describes the uncertainty in prediction due to this instability of a given model. There are multiple ways to evaluate a prediction interval.
