---
layout: article
title: Data Science Projects
---
# Data Science Projects

Independent Data science projects completed for self-learning purpose. The projects cover important concepts of data science, including data collection, data cleaning, visualization, machine learning, and statistical method.

Python libraries (<b>Pandas, Scikit-learn, Seaborn</b>) are used in these projects.

The proejcts are presented in the form of description page (Markdown) and source codes, available as Jupyter iPython notebook on <a href="https://github.com/siinn/Data-Science-Portfolio">Github</a>.
<br>
<br>


<div class="card" align="center">
    <div style="font-size:25px;font-weight:600;margin:10px 0px 10px 0px">
    .<span style="margin-left:0.8em"></span>.<span style="margin-left:0.8em"></span>.
    </div>
</div>


<div class="card" align="center">
<div class="m1">Warm Welcome</div>
<div class="m2">Improved Recommender System for Medium</div>
Built an improved collaborative filtering for new users on Medium by engineering new user features using <b>LightFM</b> model. User features are extracted from user profiles using <b>tokenizer NLTK</b> and <b>Rake NLTK</b>. Data is collected frmo Medium by web scraping using Python. Web application is built using Python, Flask, Dash and hosted on AWS.
<br><br>

<a href="{{site.url}}/projects/medium.html">
<img src="/images/projects/medium/results.png" width="100%" style="border:1px solid #C0C0C0;border-radius:5px">
</a>
<div style="font-style:italic;font-size:12px;line-height:0;color:#5E5E5E">Precision and Recall at 10 of the collaborative filtering models</div>

<br>
<boxed_big><a href="{{site.url}}/projects/medium.html">DESCRIPTION</a></boxed_big>
<boxed_big><a href="https://github.com/siinn/InsightProject">GITHUB</a></boxed_big>
</div>



<div class="card" align="center">
    <div style="font-size:25px;font-weight:600;margin:10px 0px 10px 0px">
    .<span style="margin-left:0.8em"></span>.<span style="margin-left:0.8em"></span>.
    </div>
</div>


<div class="card" align="center">
<div class="m1">NYC Rent Prediction</div>
<div class="m2">Decision Tree Regression</div>

Regression models are built for apartment rent in NYC using <b>RandomForest</b> and <b>GradientBoosting</b> regression. Two regression models are compared using statistical method. The dataset is collected by <b>web scraping</b> and cleaned using Pandas. Exploratory analysis is presented using Seaborn.
<br><br>

<a href="{{site.url}}/projects/nyc.html">
<img src="/images/projects/nyc_heatmap.png" width="100%" style="border:1px solid #C0C0C0;border-radius:5px">
</a>
<div style="font-style:italic;font-size:12px;line-height:0;color:#5E5E5E">Heat map generated by apartment distribution in NYC</div>

<br>
<boxed_big><a href="{{site.url}}/projects/nyc.html">DESCRIPTION</a></boxed_big>
<boxed_big><a href="https://github.com/siinn/Data-Science-Portfolio/blob/master/NYC-Rental-Prediction">GITHUB</a></boxed_big>
</div>




<br>
<div class="card" align="center">
    <div style="font-size:25px;font-weight:600;margin:10px 0px 10px 0px">
    .<span style="margin-left:0.8em"></span>.<span style="margin-left:0.8em"></span>.
    </div>
</div>


<div class="card" align="center">
<div class="m1">Review Classification</div>
<div class="m2">NLP and Classification</div>

Yelp reviews are classified into star ratings based on the text contents of reviews. A <b>Naive Bayes</b> classification model is built using <b>CountVectorizer</b>, <b>TF-IDF transformer</b>, and sklearn <b>Pipeline</b>, and the performance of the model is evaluated using confusion matrix and classification report.
<br><br>

<a href="https://github.com/siinn/Data-Science-Portfolio/blob/master/NaturalLanguageProcessing/notebook/Natural%20Language%20Processing.ipynb">
<img src="/images/projects/nlp/texts.png" width="80%" style="border:0px solid #C0C0C0;border-radius:5px">
</a>
<div style="font-style:italic;font-size:12px;line-height:0;color:#5E5E5E">Text contents of reviews and average vote for each star rating.</div>

<br>
<boxed_big><a href="https://github.com/siinn/Data-Science-Portfolio/blob/master/NaturalLanguageProcessing/notebook/Natural%20Language%20Processing.ipynb">GITHUB</a></boxed_big>
</div>







<br>
<div class="card" align="center">
    <div style="font-size:25px;font-weight:600;margin:10px 0px 10px 0px">
    .<span style="margin-left:0.8em"></span>.<span style="margin-left:0.8em"></span>.
    </div>
</div>


<div class="card" align="center">
<div class="m1">Recognizing Hand-written Digits</div>
<div class="m2">Unsupervised Clustering</div>

Images of hand-written digits are analyzed and classified. <b>Principal component analysis (PCA)</b> is performed on the images using Scikit-learn, as a feature reduction method. An unsupervised learning is performed using <b>K-Mean clustering</b> algorithm to classify the images into 10 categories, representing digits 0-9.
<br><br>

<a href="https://github.com/siinn/Data-Science-Portfolio/blob/master/DigitRecognizer/notebook/Principal%20Component%20Analysis.ipynb">
<img src="/images/projects/digit_recognizer/pca.png" width="80%" style="border:0px solid #C0C0C0;border-radius:5px">
</a>
<div style="font-style:italic;font-size:12px;line-height:0;color:#5E5E5E">Hand-written digits and principal components extracted from training dataset</div>

<br>
<boxed_big><a href="https://github.com/siinn/Data-Science-Portfolio/blob/master/DigitRecognizer/notebook/Principal%20Component%20Analysis.ipynb">GITHUB</a></boxed_big>
</div>

