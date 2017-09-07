---
layout: post
title: "What is probability"
date: 2017-08-07
categories: statistics
---


Unlike 'Subjective probability', or Bayesian statistics, Bayes' theorem itself has uncontroversial and trivial definition. Consider a conditional probability P(a|b).

```
P(a|b) = probability of (a) given (b)
```

Then the probability of having (a) and (b), denoted as P(a and b), is

```
P(a and b) = P(a|b) * p(b)
```

However, it is also true that the probability of having (a) and (b) can be written as

```
P(a and b) = P(b|a) * p(a)
```

Combining two equations, we get Bayes theorem.

```
P(a|b) = P(b|a)*P(a) / P(b)
```
