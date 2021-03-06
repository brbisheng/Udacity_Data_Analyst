# Identify Fraud From Enron Financial/Emails Data

This machine learning project is part of the Udacity Data Analyst Nanodegree. Some other projects could be found below:
- [Titanic Data Investigation (EDA)](https://cdn.rawgit.com/brbisheng/ProgrammingFoundations/06081bf9/Final_stage/Titanic_final_Sheng_BI_IPND_1st_modification.html) 
- Prosper Data Investigation (EDA)
  - [Part I](https://cdn.rawgit.com/brbisheng/Udacity_Projects/5ed5e4f7/Sheng_BI_EDA_Prosper_Part1.html)
  - [Part II](https://cdn.rawgit.com/brbisheng/Udacity_Projects/5ed5e4f7/Sheng_BI_EDA_Prosper_Part2.html)
  - [Part III](https://cdn.rawgit.com/brbisheng/Udacity_Projects/5ed5e4f7/Sheng_BI_EDA_Prosper_Part3.html)
- [Data Wrangling](https://cdn.rawgit.com/brbisheng/Udacity_Projects/9d73b4c6/Final_Report.html)

Enron, established in 1985, was one of the largest US energy companies. At the end of 2001, it was discovered that its reported financial condition was actually sustained by creatively planned accounting fraud. The company collapsed into bankruptcy in 2002 due to widespread corporate fraud. During the federal investigation, confidential information was made public, including massive emails and detailed financial data for executives.

We will build a POI (Person of Interest) identifier to spot culpable individuals involved in enron scandal. We will machine learning techniques and the scikit-learn Python library.

## Contents of The Project

The structure of the code is as follows:

- `poi_id.py`: main script
- `myhelpers.py`: contains all the helper funtions used in `poi_id.py`.
- `tester.py`: contains code for testing the algorithm performance

In this README.md document, you could find the answers for all the project questions. This README.md consists of 3 parts:

- Data Exploration: we describe data, identify and remove outliers
- Features Section: we create and select features using appropriate statistical methods.
- Algorithm Tuning: we tune parameters for each classifer and validate our model.

## Data Exploration

> Question 1. Summarize for us the goal of this project and how machine learning is useful in trying to 
accomplish it. As part of your answer, give some background on the dataset and how it 
can be used to answer the project question. Were there any outliers in the data when 
you got it, and how did you handle those? 

The goal of this project is to develop a predictive model that helps identify a POI (person of interest). A POI is a former Enron employee who may have been related to fraud, based on the enron data which comprise both financial and emails information of the employees.

The features extracted from enron data belong to one of the 3 categories:

- financial features (independent variables)
  - `financial_features_list = ['salary', 'deferral_payments', 'total_payments',\
                           'loan_advances', 'bonus', 'restricted_stock_deferred', \
                           'deferred_income', 'total_stock_value', 'expenses', 'exercised_stock_options', \
                           'other', 'long_term_incentive', 'restricted_stock', 'director_fees']`
- email features (independent variables)
  - `email_features_list = ['to_messages', 'email_address', 'from_poi_to_this_person', \
                         'from_messages', 'from_this_person_to_poi', 'shared_receipt_with_poi']`

- `poi` (dependent variable/label)

As a quick summary, the dataset 
- contains a total of 146 data points, of which 18 are POIs and 128 are not .
- contains 14 financial features, 6 email features.

### Looking into NaN data.

We notice significant amount of 'NaN's in data. Below is a table showing the fraction of missing values per feature.

|             | frac_missing_for_non-poi | frac_missing_for_poi	| frac_missing_total |
|-------------|--------------------------|----------------------|--------------------|
|loan_advances|	0.976562	               |0.944444	            |0.972603            |
|director_fees|	0.867188	               |1.000000	            |0.883562            |
|restricted_stock_deferred|	0.859375	   |1.000000	            |0.876712            |
|deferral_payments| 0.734375	| 0.722222	| 0.732877|
|deferred_income|	0.703125	| 0.388889	| 0.664384|
|long_term_incentive|	0.578125	|0.333333	|0.547945|
|bonus|	0.484375	| 0.111111	| 0.438356|
|from_messages|	0.437500	| 0.222222	| 0.410959|
|to_messages|	0.437500	| 0.222222	| 0.410959|
|from_poi_to_this_person|	0.437500|	0.222222|	0.410959|
|shared_receipt_with_poi|	0.437500|	0.222222|	0.410959|
|from_this_person_to_poi|	0.437500|	0.222222|	0.410959|
|other| 0.414062	| 0.000000	| 0.363014|
|expenses|	0.398438	|0.000000	| 0.349315|
|salary|	0.390625	| 0.055556	|0.349315|
|exercised_stock_options|	0.296875	|0.333333	|0.301370|
|restricted_stock|	0.273438	|0.055556|	0.246575|
|email_address|	0.273438	|0.000000|	0.239726|
|total_payments|	0.164062	|0.000000|	0.143836|
|total_stock_value|	0.156250|	0.000000|	0.136986|
|poi|	0.000000|	0.000000|	0.000000|

We can not discard any data at this point, because the 'NaN's may not necessarily represent missing data. Indeed, they may well represent that the variable in question is of value zero. For example, if there are no director fees, the value of this variable should be zero, thus replacing the 'NaN's by zero may help us make influential predictions. This conjecture is supported by the `enron61702insiderpay.pdf` in the `final_project` folder, which gives information on how each variable is formed. We thus will proceed with the NaN's being replaced by zero.

### Outliers

To spot potential outliers, we start by plotting the scatterplot of the two most interesting variabels: `salary` and `bonus`.
![alt text](https://github.com/brbisheng/Udacity_Projects/blob/master/Data_Analyst_Project_5/supporting_materials/salary-bous-scatterplot.png)

There is clearly an outlier with both enormous salary and bonus. It turns out that this outlier is named `TOTAL`, which is the sum of all other obervations. Thus we will remove this observation, and we obtain the following graph:

![salary-bonus-without-Total](https://github.com/brbisheng/Udacity_Projects/blob/master/Data_Analyst_Project_5/supporting_materials/salary-bouns-scatterplot-without-TOTAL.png)

Graphically, there seem to be still 4 outliers, either with unusually high salary or unusually high bonus. It turns out the the two observations with both high salary and bonus are SKILLING JEFFREY and LAY KENNETH, the two biggest bosses and poi's of Enron. We definitely shall keep these two observations. In addition, we have FREVERT MARK with exceptionally high salary and LAVORATO JOHN with exceptionally high bonus. After examination, we find that the values of the other features of these two persons seem to be consistent with similar observations, thus we will also keep them.

### Irrelevant data

Besides, we are going to remove two observations. The first is named 'LOCKHART EUGENE E'. Every feature of this person is NaN, thus it provides no information to help predict. The second is named ''THE TRAVEL AGENCY IN THE PARK''. This is obviously not an Enron employee, thus is irrelevant to our problem. 

In the end, we will have 143 observations to proceed.

## Feature Selection
> Question 2. What features did you end up using in your POI identifier, and what selection process did you use to pick them? 
Did you have to do any scaling? Why or why not?

I used the `SelectKBest` from `scikit-learn` package to select features based on their scores. The key parameter of the `SelectKBest` is `score_function`, by which we can choose different metrics functions to obtain the feature score rankings. Given that this is a classification problem, the most relevant `score_function`s are `f_classif` and `mutual_info_classif`. 
- With `f_classif`, we are doing ANOVA, and calcualte the F-statistic: F = variation between sample means / variation within the samples. The higher the value of the F statistic, the more significant is the relationship between the feature and the label.
- With `Mutual information`, we reply on nonparametric methods based on entropy estimation from k-nearest neighbors distances to calculate the dependency between the variables. The higher the value, the higher the dependency between the feature and the label.

I considered both `f_classif` and `mutual_info_classif` to derive my features. Specifically, for each of these two score functions, I extracted the 15 features with highest scores. I then selected 14 features based on the following rule: every feature should either be among the top 9 highest scores from the estimation result of `f_classif` or `mutual_info_classif`. The features I selected are as follows: 
- `["exercised_stock_options", "total_stock_value", "bonus", "salary", "total_gain", "frac_to_poi", "deferred_income", "long_term_incentive", "restricted_stock", "shared_receipt_with_poi", "expenses", "frac_poi", "other", "director_fees"]`

We notice that 11 out of 14 features are related to financial data, and only 3 features are related to email data. Among these features, 3 are created by me. Below are the 4 features that I created: 
- `frac_to_poi` = `from_this_person_to_poi`/`from_messages`
- `frac_from_poi` = `from_to_poi_this_person`/`to_messages`
- `frac_poi` = (`from_this_person_to_poi` + `from_to_poi_this_person`)/(`to_messages` + `from_messages`)
- `total_gain` = `total_stock_values` + `total_payments`

The first 3 features are from email data. By calculating fractions, we are able to focus on comparison based on the relative values instead of absolute values. (If some employee sent much more emails than the others, then the probability that her/his emails went to the poi is natually higher without further information.) By composing POI ratios, we are also able to verify our conjecture whether the POIs may contact each other more frequently than those non-POI's. The created features bring significant improvement to model performance for most of the algorithms used in later part of our study (e.g Accuracy, precision, recall, F1 and F2 all increase for the AdaBoost algorithm after adding the new features. In particular, the precision & recall metrics increase from 0.25 & 0.20 to 0.67 & 0.40 respectively)

The last simple feature I added is from the financial data. If we were in a linear regression problem, creating additive feature would cause multicollinearity issues. However, we are dealing with non-linear classification problems here, so doing this is problem free and may bring useful insight. We also notice that the feature `total_gain` has highest score based on the `mutual_info_classif` estimation strategy. Because of the highly complex payment structure of Enron, I did not choose to create other financial features. Also, I did not create any polynomial terms. In fact, polynomial possibilites are covered by the neural network models, which is one of the algorithms I considered for prediction. 

I also used `MinMaxScaler()` to verify the result of `SelectKBest`. The result does not change. Naturally, from the way we calculate the F-score and mutual information, we can tell that these two algorithms `f_classif` and `mutual_info_classif` do not depend upon the scale of the features.

Below is a table of the result from `SelectKBest` using `f_classif` and `mutual_info_classif` score functions respectively.

|ranking|features     |f_classif_scores|
|---|-------------|------|
| 1|  exercised_stock_options|  24.815080|
| 2|        total_stock_value|  24.182899|
| 3|                    bonus|  20.792252|
| 4|                   salary|  18.289684|
| 5|               total_gain|  16.993600|
| 6|          fraction_to_poi|  16.409713|
| 7|          deferred_income|  11.458477|
| 8|      long_term_incentive|   9.922186|
| 9|         restricted_stock|   9.212811|
|10|           total_payments|   8.772778|
|11|  shared_receipt_with_poi|   8.589421|
|12|            loan_advances|   7.184056|
|13|                 expenses|   6.094173|
|14|             fraction_poi|   5.399370|
|15|  from_poi_to_this_person|   5.243450|

|ranking|features     |mutual_info_classif_scores|
|---|-------------|------|
| 1|               total_gain|  0.080709|
| 2|  shared_receipt_with_poi|  0.079434|
| 3|                    bonus|  0.072514|
| 4|                 expenses|  0.069550|
| 5|                    other|  0.064712|
| 6|          fraction_to_poi|  0.062894|
| 7|        total_stock_value|  0.042176|
| 8|            director_fees|  0.035012|
| 9|             fraction_poi|  0.034353|
|10|         restricted_stock|  0.032661|
|11|                   salary|  0.026473|
|12|        fraction_from_poi|  0.026184|
|13|            loan_advances|  0.023276|
|14|              to_messages|  0.019640|
|15|  from_this_person_to_poi|  0.017733|


-----------------------------------------------------------

> Question 3. What algorithm did you end up using? What other one(s) did you try? How did model 
performance differ between algorithms? 

I tried the following algorithms: `AdaBoost`, `Support Vector Machine`, `Logistic Regression` (widely used in econometrics), `Gaussian Naive Bayes`, `Multi-layer Perceptron classifier`, and `Random Forest`.

I decide to proceed with all the above algorithms except for `Gaussian Naive Bayes`. They have the potential to be improved for the following reasons:
1. When we improve our feature compositions, either one or all of the metrics (precision, recall) of these algorithms have significant improvement.
2. By default, the tuning parameters have more degrees of freedom. 

Here is a summary of the final model performances **after parameter tuning**:

||Accuracy|	Precision|Recall|F1|F2|
|--------|------|--------|-------|----------|---------|
|AdaBoost|0.90000|0.50000|0.40000|0.44444|0.41667|
|MLPclassifier|0.84000|0.20000|0.20000|0.20000|0.20000|
|LogisticRegression|0.90000|0.50000|0.20000|0.28571|0.22727|
|RandomForest|0.84000|0.20000|0.20000|0.20000|0.20000|
|SVC|0.78000|0.20000|0.40000|0.26667|0.33333|

We notice that AdaBoost has the most balanced performance for accuracy, precision and recall. As a result, the F1 score, the harmonic mean of precision and recall, is also high. The F2 score is a weighted harmonic mean of precision and recall, and turns out to be highest among all algorithms chosen.

LogisticRegression has comparable accuracy and precision but has low recall. Recall measures the probability that the model can correctly spot a POI when the person is actually a POI. A low recall is unacceptable in our situation, where we do not want to miss the potential frauds.

Our candidate for the final submission of the project will be Adaboost, with the 14 features from SelectKBest:

|AdaBoost-Accuracy:|  90%|
|AdaBoost-Precision:| 50%|
|AdaBoost-Recall:|    40%|

## Algorithm Tuning

> Question 4. What does it mean to tune the parameters of an algorithm, and what can happen if you 
don’t do this well?  How did you tune the parameters of your particular algorithm? (Some algorithms do not have parameters that you need to tune, if this is the case for the one you picked, identify and briefly explain how you would have done it for the model that was not your final choice or a different model that does utilize parameter tuning, e.g. a decision tree classifier). 

Machine learning algorithms are developed with many parameters at modelers' disposal to suit model to the situation at hand.

To tune parameters means that we adjust the parameters of the algorithms in order to improve our model prediction precision. Since each dataset has its unique characteristics, it is reasonable to tune the parameters to see whether we are able to come up with a set of parameters which can optimize the model performance.  

However, we may also end up with a combination of parameters which are only perfect in the training set, but have large prediction errors when dealing with new data points. This situation is named overfitting, and this is one biggest downside of parameter tunning. 

We are going to use `Pipeline` and `GridSearchCV` modules from sklearn package to facilitate model comparison and implementaion.

Our final choice is AdaBoost, a boosting algorithm. In theory, this algorithm is prone to overfitting when we have many weak learners which are also deep decision trees. However, it is proved that "as the number of weak learners (rounds of boosting) increases, the bias converges exponentially while the variance increases by geometrically diminishing magnitudes", hence its degree of overfitting is much weaker than most of other methods. Given our data, the parameter grid I choose is as follows:

```python
param_grid = {'adb__n_estimators': range(10,80,10),
                 'adb__random_state':[0],
                 'adb__learning_rate': [0.001, 0.05, 0.1, 0.3, 0.5,0.7,0.9, 1]}
```

The optimal parameter set chosen is ```learning_rate=0.9, n_estimators=50, random_state=0```. We notice that the number of iterations (`n_estimators`) is 50, and the `learning_rate` parameter is set at 0.9. These imply that the optimal algorithm converges fast, and does not have the sign of being stuck in a local extremum. It coincides with the result in research literature that for low-dimensional problems,  early stopping is necessary to prevent from overfitting.

Given our choice, the parameters that yield the best result for AdaBoost is as follows:
```
algorithm='SAMME.R', 
base_estimator=None,
learning_rate=0.9, 
n_estimators=50, 
random_state=0
```

Our current result is derived under 2-fold cross validation.

-----------------------------------------------------------

> Question 5. What is validation, and what’s a classic mistake you can make if you do it wrong? How did you validate your analysis? 

Validation refers to the set of techniques used to make sure that our fitted model can be generalized, i.e., the estimated model is not restricted to a particular part of the data or some particular dataset. 

A classic mistake during model estimation is over-fitting. That is, the model has good performance in the training set, but has poor performance on the testing set.

To address this issue, a common strategy is to dynamically allocate training points and testing points, a method termed cross validation. We conduct cross-validation by firstly fixing  the training set/testing set ratio, then dynamically assign the data points to these sets.

In my study, I considered the following training-to-testing ratio: 6:4, 7:3 and 8:2. I then use the `StratifiedShuffleSplit()` method from `sklearn.model_selection` to shuffle the data points before splitting. Why should we shuffle the data before splitting? In our data, we have only 143 observations, with an imbalanced label: only 14 POI's against 130+ non-POI's. If we split data without shuffling, the test set has lower probability of maintaining a relatively constant fraction of POI's, which will bias the fitting and testing result of certain fold, thus producing both larger bias and variance in the prediction errors. In one word, this method adds randomness (from shuffling) before allocating data points compared to the traditional `train_test_split()` method. This randomness is essential because the size of our dataset is too small and unbalanced.

Using differnt training-to-testing set ratios, our result all yield stable accuracy, precision and recall metrics:

||6:4|7:3|8:2| 
|---|---|---|---|
|Accuracy|0.86|0.88|0.88|
|Precision|0.333|0.4|0.4|
|Recall|0.4|0.4|0.4|

Our result from `StratifiedShuffleSplit()` for AdaBoost is summarized as follows: (with a training-to-testing ratio fixed at 7:3)

```python
Pipeline(memory=None,
     steps=[('scaler', MinMaxScaler(copy=True, feature_range=(0, 1))), ('adb', AdaBoostClassifier(algorithm='SAMME.R', base_estimator=None,
          learning_rate=0.9, n_estimators=20, random_state=0))])
	Accuracy: 0.88000	Precision: 0.40000	Recall: 0.40000	F1: 0.40000	F2: 0.40000
	Total predictions:   50	True positives:    2	False positives:    3	False negatives:    3	True negatives:   42
```

----------------------------------------------------------------

> Question 6. Give at least 2 evaluation metrics and your average performance for each of them. Explain an interpretation of your metrics that says something human-understandable about your algorithm’s performance. 

The evaulation metrics that I choose are Precision and Recall. 

**Why is Accuracy not suitable?**
Accuracy is defined as the ratio of correctly predicted observation to the total observations (`double Accuracy = (TP+TN)/(TP+FP+FN+TN)`). This is a valid measure of model performance when the number of false positives and false negatives do not differ much and the distribution of the label variable is not significantly skewed to one class. However, in our Enron data set, the size of data is small and there are many more non-POI's than POI's. If we only look at accuracy, we may encounter the case where we have high overall accuracy (with low `FP` + `FN` in total), but `FP`>>`FN` or `FP` << `FN`. That's the reason why we should look into other metrics, when we know that our data has some unique characteristics which can not be fully captured by conventional measures.

**Precision**
Precision is defined as `TP/TP+FP`. It is the fraction of correctly labelled POI's among all the labelled POI's. With a value of 0.40, it means that 40% of the POI's would not go unnoticed by our model.

**Recall**
Recall is defined as `TP/TP+FN`. It describes the model's ability of correctly identifying a POI, given that the person is indeed a POI. With a value of 0.40, it means that 40% of the true POI's were successfully identified.

Notice that at a magnitude of 40%, this metric is already very insightful for the Enron case. Our objective is to detect frauds, we do not want to miss any potential guilty person. This may come at a cost of falsely identifying some innocent person as criminal, but it is acceptable in our case.

Let us analyze the case where we have high precision, but low recall. High precision means that whenever our model lables a person as POI, we are very confidentent that this person is not innocent. Now if we have low value of recall, it means that the model is very strict in the edge cases, so that many potential guilty persons may be ignored. This is definitely something undesirable in our enron situation. 

In the case where we have low precision and high recall, it means that we have a lot of persons falsely labelled as criminal. This is OK, because further investigation can help us rule out the culpability of those innocent.

To sum up, I used precision and recal as my evaluation metrics. And I choose AdaBoost as my final model (precision 0.40 & recall 0.40). Due to the nature of our dataset, accuracy could only give us a loose impression of model fitness, so we need to look further into precision and recall values to determine whether our model can yield reliable predictions.

References:
1. why should we shuffle?
  - https://stackoverflow.com/questions/48403239/what-is-the-differene-between-stratify-and-stratifiedkfold-in-python-scikit-lear
  - https://stackoverflow.com/questions/45969390/difference-between-stratifiedkfold-and-stratifiedshufflesplit-in-sklearn
2. When is adaboost overfitting?
  - https://stats.stackexchange.com/questions/163560/how-many-adaboost-iterations
  - https://stats.stackexchange.com/questions/20622/is-adaboost-less-or-more-prone-to-overfitting
