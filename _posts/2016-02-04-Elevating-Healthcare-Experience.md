---
layout: post
title: "Elevating Healthcare Experience: Leveraging Patient Feedback Data"
published: true
tags: 
  - "null"
---







### Problem: What is the optimal number of patients a provider should see in a day?

For my Insight project, I wanted to work on solving a common problem that we often encounter. During outpatient visits, many of us end up waiting for a long time to see the doctor. Ideally, patients should get to spend more time with the doctor in the exam room, which can elevate the overall healthcare experience. 

This leads us to the question of my problem statement. How many patients should providers (doctors) see in a day? This is to make sure patients do not feel rushed, receive adequate time and attentive care from their providers and respond with high rating scores on feedback survey questions.

It may seem intuitive to think that the fewer patients each provider sees a day, the better scores these providers and the healthcare organizations receive. In reality , however, these decisions are not as straightforward as they seem. Healthcare organizations need to maintain healthy cash flow margins to cover huge operating expenses. They need to maintain the right patients per provider ratios to stay in business. More specifically, the question then becomes: what is the optimal ratio that allows healthcare organizations to attend to the needs of all their patients and at the same time allow patients to have more time with their providers?

To help answer this question, I partnered with NYC-based startup that collects patient feedback data for outpatient clinics. 

### How I solved it? - Data and Methods

![Algorithm Stages Pipeline.png]({{site.baseurl}}/images/Algorithm Stages Pipeline.png)

The first step was to segragate data specific to each organization, extract the features of importance and weed out missing values. I derived the following features to drive my analyses:

1. Patients seen by a provider in a day
2. Day of the week
3. Net promoter score

Net promoter score (NPS) is based on raw score provided by patients on feedback surveys. It a percentage that can range from -100 to +100 depending on the percentage of attractor scores (raw score of 9 or 10) and detractor scores (raw scores from 0 to 6). 

I used kernel density estimation with Gaussian kernel to study the underlying distribution of NPS percentages by patients seen per day metric. It helped me visualize that probability density of NPS by number of patients seen in a day. I performed binarization of NPS to classify percentages greater than or equal to 90 as good scores and those below 90 as bad scores.

I plotted the distribution of good scores versus bad scores by patients seen per day. By performing Mann-Whitney U test, I was able to see a statistically significant difference between the distributions of the two groups. At 95% confidence level about the mean, I was able to find the interval for number of patients a provider should see in a day for good scores and also the interval for bad scores. This helped me arrive at a solution for the optimal number of patients a provider should see in a day without seeing a considerable drop in the scores.

![Histogram_volume_nps_2.png]({{site.baseurl}}/images/Histogram_volume_nps_2.png)

While working on the problem, I noticed a difference in the percentage of good scores to bad scores by day of the week. Combining day of the week information with the patients seen per day, I was able to build a logistic regression model to predict the chance of getting good scores with 67% accuracy. With 50% being chance, the model performs reasonably better than chance.

![AUC_ROC_LG.png]({{site.baseurl}}/images/AUC_ROC_LG.png)

In order to maintain the optimal number of patients per provider, it is important to be able to gauge expected patient volumes and have optimal number of providers on site. In order to forecast patient volume, I used time series analysis to study past temporal trends in patient volume. Using auto regressive (AR) as well as auto regressive and moving average (ARMA) models, I was able to perform 1-step forecasting of volume by month and volume by week respectively with a mean absolute percentage error of about 17%.

![ARMonthModified.png]({{site.baseurl}}/images/ARMonthModified.png)

![ARMAWeekModified.png]({{site.baseurl}}/images/ARMAWeekModified.png)


_Please note that some of the numbers have been obfuscated in the figures above to protect sensitive information about clients of the startup._

### Deep Dive: What led me to the choice of models for my analyses?

Scatterplot distribution of the NPS by patients seen per day as shown in the figure below (left) does not indicate a particularly strong correlation. Plotting the kernel density estimation of the distribution helped me visualize the probablity density of good scores between 10 - 15 patients seen per day as shown in darker hue of red in the figure below (right).

![VisualizingData.png]({{site.baseurl}}/images/VisualizingData.png)

Visualizing the probability density led me to study the differences in distribution of the good and bad scores and found statistically significant results (p-value = 0.000). Due to lack of perfect normality in the underlying distribution, I used non-parametric Mann-Whitney U test to compare differences in measure of central tendency (mean) between the two groups.  



### Putting It All Together

The final product (outcome of my analyses) focuses on using the following three models to solve the problem:

1. Mann-Whitney U test to determine the optimal number of patients per provider
2. Forecasting patient volume to have enough providers onsite to maintain 	optimum patients per provider ratios
3. Logistic regression model to determine the probability of receiving good scores by day of the week and patients per provider ratio.

Insights derived thorugh this project should help the startup advise its clients on optimum management of patient per provider ratios leading to better healthcare experience for patients. 

I implemented this project using Python - Pandas for data processing and Scikit-learn for modeling. [Here](https://github.com/anjalibshah/insight-data-science-project.git "Insight Project Github Repository") is the link to my github repository for this project's source code.
