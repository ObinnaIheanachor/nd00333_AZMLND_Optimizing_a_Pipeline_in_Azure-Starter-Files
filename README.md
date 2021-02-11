# Optimizing an ML Pipeline in Azure

## Overview
This project is part of the Udacity Azure ML Nanodegree.
In this project, we build and optimize an Azure ML pipeline using the Python SDK and a provided Scikit-learn model.
This model is then compared to an Azure AutoML run.


## Problem Statement
I worked on a dataset of Bank Marketing with the primary task of carrying out binary classification. I had to clean and transform the dataset 
and employ both Python SDK and AutoML in completing the project. The dataset provided had class imbalance as well and no missing values. 
Further details about the dataset can be found <a href='https://archive.ics.uci.edu/ml/datasets/Bank+Marketing'>here</a>

## Solution

![](https://github.com/ObinnaIheanachor/nd00333_AZMLND_Optimizing_a_Pipeline_in_Azure-Starter-Files/blob/main/Archietecture.jpg)

Using the workflow depicted in the image above, the solution adopted a two(2) way approach to solve the classfication task. I used both Microsoft Azure Hyperdrive and AutoML to train the dataset. A Logistic Regression model was used for the Hyperdrive run and I had the flexibility of comparing several models for the AutoML run. Both runs were compared to check for the best performing model.
Logistic Regression had an accuracy of 0.9142 on Python SDK via HyperDrive while the best performing model was VotingEnsemble with accuracy of 0.9167 on Azure AutoMl run.

## Scikit-learn Pipeline

For the Scikit-learn pipeline, Logistic Regression which is a Linear model was used. The pipeline also consists of compute target/cluster created in Microsoft Azure, a train.py script where I did most of the data preparation and a Jupyter notebook to programmatically control the experiment.

### Data Loading
The bankmarketing dataset was loaded into the datastore and workspace using the `TabularDatasetFactory`

### Data Processing and Modelling

The data was then cleaned using the `cleandata` function in the train.py script and subsequently split into `train` and `test` . A `Logistic Regression` was used and the two(2) hyperparameters used to train the model were `C` & `max_iter`. 

`RandomParameterSampling` was used and this defines random sampling over a hyperparameter search space. The parameter values are choosen from a set of discrete values or a distribution over a continuous range. So this makes the computation less expensive.

The early stopping policy used was `BanditPolicy` and this policy terminates runs early where the primary metric is not within the pre-defined slack factor with respect to the best performing training run.

## AutoML

About 24 models were trained with 5 cross validations and 4 of the 24 models perform better than the Logistic Regression model from Scikit-learn pipeline. The best performing model was `VotingEnsemble` with an accuracy of 0.9167. VotingEnsemble works by combining the predictions from several models. This often leads to a better performance than if a single model was used.
Hyperparameters generated were 'min_samples_leaf' ,  'min_samples_leaf' and 'n_estimators'. Due to imbalance in the dataset, we can use a different metric than accuracy like AUC and ROC_AUC_Score, or f1 score.

## Pipeline comparison

The model accuracy for the hyperdrive model is *91.42%* and the accuracy score for the AutoML is *91.67%*, the difference is a bit significant and may be due to difference in both architectures. For the hyperdrive model, it requires lots of manual fine-tuning and less flexibility in choosing the right hyperparameters to help increase the performance of the model which may be daunting, hence a model may not necessarily perform well for a use case but there won't be a way to determine unless it is tested, however, the AutoML give the flexibility of exploring other algorithm and easy hyperparameter tuning.
On the other hand, the Python SDK pipeline provides more customization but at the expense of longer time in orchestrating the pipeline.
One approach would be to use AutoML to compare several models, and then customize and optimize the best performing model using the Python SDK pipeline.

## Future work

1. Dealing with class imbalance will help improve the experiment as this will eliminate bias towards the unbalanced class.
2. Extensive EDA and engineering new features may lead to improvements in the results of the experiment
3. Model Interpretation on the best performing model(s) to help understand which features are most important in the predictions.
