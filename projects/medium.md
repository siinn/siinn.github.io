---
layout: article
title: Improved recommender system for new users on Medium
---
# Improved Recommender System for Medium
> Improved collaborative filtering for new users on Medium by engineering new user features using <b>LightFM</b> model. User features are extracted from user profiles using <b>tokenizer NLTK</b> and <b>Rake NLTK</b>. Data is collected frmo Medium by web scraping using Python. Web application is built using Python, Flask, Dash and hosted on AWS.

**Table of Contents:**
- Table of Contents
{:toc}

## Introduction

Medium is a popular online publishing platform with more than 60 million monthly readers. One of the reasons for their success is their recommender system. The recommender system helped many businesses to be extremely successful, including Netflix, Youtube, or Amazon.

However, the recommender system isn't perfect. Modern recommender systems, i.e. __collaborative filtering__, are based on user interactions. On Medium, they use readers' views, comments, and likes (claps) to determine the best recommendation for the user. However, naturally this becomes a problem for new user as the system wouldn't have those records, and it won't be able to make customized recommendations. This is known as __cold start problem__.

In this project, I introduced new user features extracted from user profiles to help the recommender system to make better recommendation in the early interactions for new users. As a deliverable, a __web application__ is built using Python, Flask, Dash and hosted on the AWS.




## Data Collection
Jupyter notebook for this section is available in <boxed><a href="https://github.com/siinn/InsightProject/blob/master/preprocessing.py">Data collection</a></boxed>.

Data is collected by web scraping from [Medium.com](https://medium.com), and Python <verbatim>requests</verbatim> and <verbatim>BeautifulSoup</verbatim> libraries are used for web scraping and parsing the data. The web scraping jobs are parallelized so that the data collection can be done in short time. In order to handle failed request, it checks the status codes of request and attempt to retry after 3 seconds if it failed.

The following information is collected with time range between 2012 and present. Due to the scope of this project, topics are limited to "Data Science" and "Machine Learning".

- Articles (15k)
- Users (32k)
- Comments (44k)

<br>


## Exploratory anlaysis
In this section, a quick exploratory analysis is performed in order to understand different features collected in the previous section.

### Number of articles
First, the number of articles related to data science and machine learning on medium is plotted as a function of time. The plot shows that the number of articles increased over time with large fluctuation. 
<br>
<div align="center"><img src="/images/projects/medium/article_time.png" width="400px"></div>

### Comments and likes (claps)
Next, the distributions of number of comments and likes for each article are plotted. The plots show that most articles have less than 20 responses and a small number of claps. 
<div align="center"><img src="/images/projects/medium/article_claps_response.png" width="800px"></div>




## Feature Engineering (NLP)
One of the most important part in this project is the engineering user features. The user features are extracted from user profiles on Medium and used as input for the recommender system. For an example, given a user profile as below,

    Software developer with a passion for natural language processing, machine learning, data science, tech buzzwords, and Oxford commas.

<br>
the profile is tokenized into words using **tokenizer, NLTK**. Then this use would have tokens such as 

    "data science", "developer", "machine learning", "buzzwords", "oxford".

<br>
Some of the tokens are relevant to the user, but there are also other tokens that aren't very descriptive such as "buzzword" or "oxford". Therefore, only the keywords are extracted from these tokens using __Rake, NLTK__. In order to extract the keywords, all user profiles are merged into a single document and used as corpus for the keyword extraction. This step was necessary because keyword extraction works similar to TF-IDF, and it doesn't work well with a short text.

The most common keywords from the corpus is shown below.

<div align="center"><img src="/images/projects/medium/keywords.png" width="500px"></div>





## Machine Learning (Collaborative Filtering)
Jupyter notebook for this section is available in <boxed><a href="https://github.com/siinn/InsightProject/blob/master/LFM_validation.py">MACHINE LEARNING</a></boxed>.

The recommender system is built using <a href="https://github.com/lyst/lightfm">LightFM</a> model, an industrial standard collaborative filtering model. In this model, user comments are used as positive signals, i.e. based on the comments user left, the system would determine the best articles to recommend. LightFM model is used for the following reasons. More description of the model can be found in this <a href="https://arxiv.org/abs/1507.08439">paper</a>.

- The model can work well with user features and item features
- The model can handle implicit binary rating.

<br>

The user features obtained from the feature engineering are supplied to the model as sklearn sparse matrix.

    # convert dictionary to sparse matrix of user features
    from sklearn.feature_extraction import DictVectorizer
    dv = DictVectorizer()
    
    user_features  = dv.fit_transform(df["keyword"])


<br>
In this sparse matrix, users are represented as rows and user features are represented as columns. Two models are built using the LightFM.
- Cold start model where the model only uses user comments are positive signal
- Warm start model where the model uses both user comments and user features

<br>
As a hyperparameter search, precision and recall of two models are plotted below.
<div align="center"><img src="/images/projects/medium/epoch.png" width="500px"></div>
The plot shows that both models reach plateau after ~5 epochs. 


## Validation

The performance of two models are compared using precision, recall at 10, and the corresponding F1 score. In addition to the cold start and warm start model, the performance of "most-popular" model, in which the articles with most comments are recommended, and "random" model are compared. i.e. we compare the performance of the following four models:

- Cold start 
- Warm start
- Most popular
- Random

<br>
The precision and recall at 10 scores are repeatedly calculated for these models 100 times using __hold out__ and __random sampling__ methods. The distributions of precision and recall at 10 for these models are shown below. 
<div align="center"><img src="/images/projects/medium/precision_recall.png" width="800px"></div>
The plots show that the warm start model performs better than the cold start model. The most-popular and random models show relatively poor performance as expected.

Similarly, the distribution of F1 score of these models are shown below after repeating the calculation for 100 times.
<div align="center"><img src="/images/projects/medium/f1_at_k_df.png" width="500px"></div>
The plot shows that the warm start model performs better than the cold start model by 22%.



## Conclusion
In this projects, the recommender system is built for Medium using user comments are positive signals. In attempt to solve cold start problem, the user features are engineered from the user profiles on Medium. The results shows this method has a potential to help solving the cold start problem for Medium. This means that it can help Medium to
- retain new users
- improve customer satisfactions


<br>
The method is also applicable to other businesses that use recommender system as their business model. However, due to the scope of this project, the method is limited to a single topic, and generalizing the method into more topics would be an interesting future project. 


<br><br>
