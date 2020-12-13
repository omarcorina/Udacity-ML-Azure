# Optimizing an ML Pipeline in Azure

## Overview
This project is part of the Udacity Azure ML Nanodegree.
In this project, we build and optimize an Azure ML pipeline using the Python SDK and a provided Scikit-learn model.
This model is then compared to an Azure AutoMl run.

## Summary
For this project we are using a dataset with information of customers of a bank, and we seek to predict if the client will subscribe a bank term deposit.

Two approaches were applied to solve this problem in Azure ML platform: a logistic classification model using a hyperdrive to find the best parameters, and AutoMl. The best performing model was the logistic classifier.

## Scikit-learn Pipeline
The pipeline architecture consisted of a training script and a hyperdrive pipeline. The training script was used to load and clean the data, and to add the arguments to include int he logistic model. The hyperdrive pipeline included the setting of the working environment in Azure ML, and a series of steps to tune the parameters, score and evaluate the logistic model. The steps included the parametrization of a random parameter sampling, the definition of an early stopping policy, creating an estimator using the training script, and passing the parameters to the hyperdrive config function.

The sampler parameter used was a random parameter sampler. This sampler allows to test a random set of parameters defined in the search space. In this exercise the parameters used in the search space were C -the inverse of regularization strength-, and max iterations -maximum number of iterations taken for the solvers to converge. 

In machine learning, early stopping is a form of regularization used to avoid overfitting when training a learner with an iterative method. Early stopping rules provide guidance as to how many iterations can be run before the learner begins to over-fit.

## AutoML
The winner AutoMl model was a voting ensemble of the following algoritms: ensembled_algorithms": "['LightGBM', 'XGBoostClassifier', 'XGBoostClassifier', 'XGBoostClassifier', 'RandomForest']" with the following weights: "ensemble_weights": "[0.3333333333333333, 0.13333333333333333, 0.26666666666666666, 0.2, 0.06666666666666667]". We used AUC weighted as the primary metric of the model. 

In this winning pipeline the AutoMl reached a precision score of 0.914 and an AUC weighted of 0.948. Below a list of the main parameters of the winning AutoMl pipeline.

"ensembled_iterations": "[19, 0, 1, 23, 22]",
"ensembled_algorithms": "['XGBoostClassifier', 'LightGBM', 'XGBoostClassifier', 'XGBoostClassifier', 'LightGBM']",
"ensemble_weights": "[0.26666666666666666, 0.3333333333333333, 0.13333333333333333, 0.2, 0.06666666666666667]",
"best_individual_pipeline_score": "0.9470874417221132",
"best_individual_iteration": "19",
"score": "0.9479907145030658"
"training_type": "MeanCrossValidation",
"num_classes": "2",
"framework": "sklearn",
"fit_time": "30",
"goal": "AUC_weighted_max",
"primary_metric": "AUC_weighted",

## Pipeline comparison
Both the hyperdrive pipeline and the AutoMl achieved high levels of accuracy, but he logistic classifier had an accuracy of 0.9144, versus 0.91397 for the winning architecture resulting from the AutoMl. The difference in architecture between the hyperdrive and the AutoMl may be due to the fact that the primary metric of the hyperdrive was accuracy while for AutoMl it was AUC weighted, so both model optimize by different metrics.   

## Future work
For future experiments the models could be improved using other hyperparameters of the logistic classifier in the hyperdrive, such as different penalties or different solvers. In the case of AutoMl, increasing the experiment timeout time could result in better models.
