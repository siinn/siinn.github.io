---
layout: default
title: "Unsettled: What Climate Science Tells Us, What It Doesn’t, and Why It Matters"
author: Steven E. Koonin
date: 2021-05-09
categories: ClimateChange
cover: /images/books/unsettled.jpg
---
## Introduction
One of the important concepts in building the regression model is a *prediction interval*. Often the prediction interval is confused with a similar concept, a *confidence interval*. 
<!--more-->
A confidence interval is typically associated with a coefficient in a regression model. It tells us the uncertainty in a coefficient of a regression model. A confidence interval depends on many factors such as training sample size, and correlation between features. 

Unlike the confidence interval, a prediction interval is associated with a individual data point and the prediction made by the regression model. So let's say we trained a model using a training sample, and we made a prediction on a particular data point. There must be an intrinsic uncertainty associated with this prediction due to the variance in data. However, the instability of the model also contributes to the uncertainty in prediction. i.e. if we repeat fit/predict using the same dataset, we might get a different prediction.

A prediction interval describes the uncertainty in prediction due to this instability of a given model. There are multiple ways to evaluate a prediction interval.

## Bootstrapping 
One way to assess a prediction interval is using bootstrapping samples. The idea and procedure is simple.
- Train and make prediction using the original data.
- Produce $$N$$ numbers of bootstrapping samples from training sample.
- For each bootstrapping sample, train a model and make a prediction.
- Calculate a residual between the bootstrapping prediction and the prediction from original data.
- Add the residual to the original prediction and record the result.
- Repeat the steps 2-5, $$N$$ times.
- The distribution of recorded result gives us prediction interval (choose 5% and 95% quantile).

<br>
Remember that a prediction interval is the uncertainty associated with an individual data point. So we are not utilizing the validation sample. This means that a prediction interval does not tell us how good the prediction is. It only tells us how stable the prediction is.

Bootstrapping method can be useful for certain types of regression such as linear regression that does not provide natural way to assess a prediction interval. In the next section, you will see that some regression models such as tree-based model provides an alternative way to estimate a prediction interval without bootstrapping.

## Quantile regression
In tree-based boosting methods such as Random Forest or Gradient Boosting, more than one weak estimator is trained, and these weak learners are combined to make final prediction. This allows us to assess lower and upper bound of a prediction without producing bootstrapping samples.

Scikit-learn provides a simple example of prediction interval estimation using Gradient Boosting here.
[http://scikit-learn.org/stable/auto_examples/ensemble/plot_gradient_boosting_quantile.html](http://scikit-learn.org/stable/auto_examples/ensemble/plot_gradient_boosting_quantile.html)

In this example, a single Gradient Boosting regressor is used to make three predictions: Least squares, quantile regression with alpha = 0.1 and 0.9. This is achieved by the following steps.

- Initialize a GradientBoosting regressor with loss='quantile'. The default value of alpha is 0.9.
- Train the model with training sample, and record the prediction of a particular data point of interest.
- Set alpha of the sample model to 0.1.
- Train the model with new alpha, and record the prediction of the data point.
- Set loss to 'ls' which refers to least squares regression.
- Train the model with 'ls', and make prediction of the data point.

In this procedure, the prediction from least square regression provides the central value for the prediction, and two other predictions from quantile regressions provide upper and lower bound for the prediction interval.


## Conclusion
Building regression models is fun, and sometimes it even feels too easy. However, evaluating and understanding the performance of the trained model should be done more carefully, and confidence intervals and prediction intervals can very useful information about the model and its performance.

