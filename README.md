# Optimizing an ML Pipeline in Azure

## Overview
This project is part of the Udacity Azure ML Nanodegree.
In this project, we build and optimize an Azure ML pipeline using the Python SDK and a provided Scikit-learn model.
This model is then compared to an Azure AutoML run.

## Summary
For this project we are using a dataset with information of customers of a bank, and we seek to predict if the client will subscribe a bank term deposit.

Two approaches were applied to solve this problem in Azure ML platform: a loggistic classification model using a hiperdrive to find the best parameters, and automl. The best performing model was the loggistic classifier.

## Scikit-learn Pipeline
The pipeline architecture consisted of a training script and a hyperdrive pipeline. The training script was used to load and clean the data, and to add the arguments to include int he loggistic model. The hyperdrive pipleine included the setting of the working environment in Azure ML, and a series of steps to tune the parameters, score and evaluate the loggistic model.The steps included the parametrization of a random parameter sampling, the deffinition of an early stopping policy, creating an estimator using the training script, and passing the parameters to the hyperdrive config function.

The sampler parameter used was a random parameter sampler. This sampler allows to test a random set of parameters defined in the search space. In this excescice the parameters used in the search space were C -the nverse of regularization strength-, and max itterations -maximum number of iterations taken for the solvers to converge. 

In machine learning, early stopping is a form of regularization used to avoid overfitting when training a learner with an iterative method. Early stopping rules provide guidance as to how many iterations can be run before the learner begins to over-fit.

## AutoML
The winner AutoMl model was a voting ensemble of the following algoritms: ensembled_algorithms": "['LightGBM', 'XGBoostClassifier', 'XGBoostClassifier', 'XGBoostClassifier', 'RandomForest']" with the following weights: "ensemble_weights": "[0.3333333333333333, 0.13333333333333333, 0.26666666666666666, 0.2, 0.06666666666666667]". We used AUC weighted as the primary metric of the model. 


## Pipeline comparison
Both the hyperdrive pipeline and the AutoMl achieved high leves of accuracy, but he loggistic classifier had an accuracy of 0.9144, versus 0.91397 for the winning architecture resulting from the AutoMl.The difference in architecture between the hyperdrive and the AutoMl may be due to the fact that the primary metric of the hyperdrive was accuracy while for AutoMl it was AUC weighted, so both model optimize by different metrics.   

## Future work
For future experiments the models could be improved using other hyperparameters of the loggistic classifier in the hyperdryve, such as different penalties or different solvers. In the case of AutoMl, increasing the experiment timeout time could result in better models. 
