---
layout: post
title: "Correlation between features"
date: 2017-11-16
categories: MachineLearning
---
**A linear regression may fail if there are features with strong correlations.**
<!--more-->

Suppose we are building a regression model. A linear regression may fail if there are strong correlation between features. An example of such features would be the size of house and the number of bedrooms in a house. So what happens when you include two highly correlated features, calling feature A and B, in a model. The "significance" of a feature is determined by many factors such as the number of features, the data size, the effect of given features, and correlation with other features. Even if we include both A and B in the model, one would not simply become insignificant because of the others. 


<br>
An important concept in dealing with correlations between features is the _variance inflation factor_ (VIF). The VIF quantifies the collinearity between features in an ordinary least squares regression models. The VIF for a feature $$j$$ is defined as $$\frac{1}{1-R_{j}^{2}}$$ where $$R_{j}^{2}$$ is the coefficient of multiple correlation which ranges from 0 to 1. It represents the unexplained variance ($$R^{2}$$) when we regress the feature $$j$$ using the remaining features. 

So let's think about what VIF represent. If the feature $$j$$ is uncorrelated to the other features, $$R^{2}$$ will be greater than 0, leading the VIF to be greater than 1. If the feature $$j$$ has no correlation to other features, $$R^{2}$$ will be 0, and the VIF will be 1. In simple word, the VIF tells us how much the variance of the sampling distribution of the regression coefficient of the feature $$j$$ would increase if $$j$$ is uncorrelated to other features.

Therefore, if we have two correlated features in a linear regression model, we can estimate how much variance of the sampling distribution of the regression coefficient would increase by estimating the VIF for each features.

And because of the variance inflation, the variance of the correlated features would increase, and the linear regression model would have higher p-value, making it less significant.
