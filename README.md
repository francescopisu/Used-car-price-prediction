# Regression Analysis for Used Car Price Prediction
[Francesco Pisu](http://github.com/francescopisu), Hicham Lafhouli

- [Regression Analysis for Used Car Price Prediction](#regression-analysis-for-used-car-price-prediction)
  * [1.1 Introduction](#11-introduction)
  
  * [1.2 Tools](#12-tools)
  * [1.3 Used car price prediction problem](#13-used-car-price-prediction-problem)
- [2. Notebook structure](#2-notebook-structure)
- [3. Methodology](#3-methodology)
  * [3.1 ELK Stack Analysis](#31-elk-stack-analysis)
    - [Logstash](#logstash)
    - [Elasticsearch](#elasticsearch)
    - [Kibana](#kibana)
  * [3.2 Regression Analysis](#32-regression-analysis)
    - [Data Analysis](#data-analysis)
    - [Data preprocessing](#data-preprocessing)
    - [Removing Outliers](#removing-outliers)
    - [Managing categorical features](#managing-categorical-features)
- [4. Comparing regression models](#4-comparing-regression-models)
  * [4.1 Parallelizing Hyperparameter Tuning with Apache Spark](#41-parallelizing-hyperparameter-tuning-with-apache-spark)
  * [4.2 Linear Regression](#42-linear-regression)
  * [4.3 Decision Tree Regression](#43-decision-tree-regression)
  * [4.4 Random Forest Regression](#44-random-forest-regression)
- [5. Experimental Results](#5-experimental-results)
- [6. Conclusions](#6-conclusion)
- [References](#references)

<!-- FIRST CHAPTER -->
## 1.1 Introduction
This project aims to solve the problem of predicting the price of a used car, using Sklearn's supervised machine learning techniques integrated with Spark-Sklearn library. It is clearly a regression problem and predictions are carried out on dataset of used car sales in the american car market. Several regression tecniques have been studied, including Linear Regression, Decision Trees and Random forests of decision trees. Their performances were compared in order to determine which one works best with out dataset.

This project has been developed for the [Big Data](http://people.unica.it/diegoreforgiato/didattica/insegnamenti/?mu=Guide/PaginaADErogata.do;jsessionid=3BAC62552963C7EB2AD77FCA1D703ACF.jvm1?ad_er_id=2018*N0*N0*S1*29056*20383&ANNO_ACCADEMICO=2018&mostra_percorsi=S&step=1&jsid=3BAC62552963C7EB2AD77FCA1D703ACF.jvm1&nsc=ffffffff0909189545525d5f4f58455e445a4a42378b) course at [University of Cagliari](http://corsi.unica.it/informatica/), supervised by professor [Diego Reforgiato](http://people.unica.it/diegoreforgiato/).

## 1.2 Tools
Most of the project has been developed using Python as the programming language of choice and the following libraries:
- [Scikit-Learn](https://scikit-learn.org/stable/), regression models and cross validation techniques.
- [Spark-Sklearn](https://github.com/databricks/spark-sklearn), parallelization of the hyperparameter tuning process.
- [Pandas](https://pandas.pydata.org), data analysis purposes.
- [ELK Stack](https://www.elastic.co/elk-stack), data analysis too.
- [Rfpimp](https://github.com/parrt/random-forest-importances), feature importances in random forests.

Last but not least, [Google Colaboratory](https://colab.research.google.com/) was the computing platform of choice for training and testing our models.

## 1.3 Used car price prediction problem
Used car price prediction problem has a certain value because different studies show that the market of used cars is destined to a continuous growth in the short term. In fact, leasing cars is now a common practice through which it is possible to get get hold of a car by paying a fixed sum for an agreed number of months rather than buying it in its entirety. Once leasing period is over, it is possible to buy the car by paying the residual value, i.e. at the **expected** resale price. It is therefore in the interest of vendors to be able to predict this value with a certain degree of accuracy, since if this value is initially underestimated, the installment will be higher for the customer which will most likely opt for another dealership. It is therefore clear that the price prediction of used cars has a high commercial value, especially in developed countries where the economy of *leasing* has a certain volume.  
This problem, however, is not easy to solve as the car's value depends on many factor including year of registration, manufacturer, model, mileage, horsepower, origin and several other specific informations such as type of fuel and braking sysrem, condition of bodywork and interiors, interior materials (plastics of leather), safery index, type of change (manual, assisted, automatic, semi-automatic), number of doors, number of previous owners, if it was previously owned by a private individual or by a company and the prestige of the manufacturer.  
Unfortunately, only a small part of this information is available and therefore it is very important to relate the results obtained in terms of accuracy to the features available for the analysis. Moreover, not all the previously listed features have the same importance, some are more so than others and therefore is essential to identify the most important ones, on which to perform the analysis.  
Since some attributes of the dataset aren't relevant to our analysis, they have been discarded; so, as mentioned above, this fact must be taken into account when conclusions on the accuracy are drawn.

<!-- FIRST CHAPTER -->

<!-- SECOND CHAPTER -->
# 2 Notebook structure
The python notebook is structured as follows:

```
Used_car_price_prediction:
│   installing libraries 
│   imports
│   read csv file 
│
└─── Price attribute analysis
│     relationship with numerical features
│     relationship with categorical features
│     feature importance related to price
│     correlation matrices (price and general)
│     scatterplots
|
└─── Preprocessing
│     outliers management
│         │     bivariate analysis
│         │     removing outliers by model
│         │     read new csv file (cleaned_cars)
│         │     final outliers removal
│         --------------------------------------
│     towards normal distribution of prices
│     label encoding
│     train/test split
|     
└─── Training and evaluating models
│     function definitions
│     linear regressor
│     decision tree regressor
│         |     complexity curve
│         ----------------------
│     random forest regressor
│         |     complexity curve
│         ----------------------
|
└─── Cross validation
│     linear regression
│     decision tree regression
│     random forest regression
|
└─── Predictions on final model and conclusions
|     feature importance

```
<!-- SECOND CHAPTER -->

<!-- THIRD CHAPTER -->
# 3 Methodology
This chapter provides an in-depth description of the followed methodology for solving the problem discussed above, with particular emphasis on the first phase concerning the dataset analysis carried out with ELK stack and the consequent preprocessing of data, followed by the definition, training and evaluation of the chosen regression models, highlighting the importance of integrating these techniques with Spark to parallelize the process of hyperparameter tuning of decision models.  
The [dataset](https://www.kaggle.com/jpayne/852k-used-car-listings) on which the regression analysis was performed consists of approximately 1.2 milion records of used car sales in the american market from 1997 to 2018, acquired via scraping on [TrueCar.com](http://truecar.com) car sales portal.  
Each record has the following features: Id, price, year, mileage, city, state, license plate, manufacturer and model.


 

<!-- THIRD CHAPTER -->
































