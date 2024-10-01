---
title: "R: Machine Learning Intro"
datePublished: Fri Jan 06 2017 14:13:24 GMT+0000 (Coordinated Universal Time)
cuid: ckruo1gea00eancs11luldtw8
slug: r-machine-learning-intro
tags: machine-learning, r

---

Some basic notes.

Steps: Question -&gt; input data -&gt; features -&gt; algorithm -&gt; parameters -&gt; evaluation

* Large data sample: 60% training, 20% test, 20% validation
    
* Medium data sample: 60%-75% training, 25%-40% test
    
* Small data sample: only training, then cross validation on a sample part
    

Evaluation indexes:

1. Sensitivity: True Positive / (True Positive + False Negative)
    
2. Specificity: True Negative / (False Positive + True Negative)
    
3. Positive Predictive Value: TP / (TP + FP)
    
4. Negative Predictive Value: TN / (FN + TN)
    
5. Accuracy: (TP + TN) / (TP + FP + TN + FN)
    
6. MSE (continuous values): 1/N(Σᵢ Predictionᵢ - Truthᵢ)²
    
7. RMSE (Root Mean Square Error): √MSE
    

Caret package:

```sql
library(caret)
```

### 1\. Pre-process (cleaning)

Search for NA, correlation between variables (if correlated think about PCA)

### 2\. Build Partitions

```sql
library(kernlab)

#Example: see if email is spam
data(spam)
dati <- createDataPartition(y=spam$type, p=0.75, list=FALSE)
training <- spam[dati,]     #train data
testing <- spam[-dati,]     #test data
```

Alternatives to createDataPartition():

* createFolds() to create n partitions for different tests
    
* createResample() to use each record multiple times
    
* createTimeSlices() for historical series (use more recent data for test)
    

### 3\. Build the model and make predictions

General Multiple Linear Regression model:

```sql
modello <- train(type~., data=training, method="glm")
```

See the coefficients:

```sql
modello$finalModel
```

Prediction on test data:

```sql
previsioni <- predict(modello, newdata=testing)
```

Some other models:

* Trees, method="rpart"
    
* Random forests, method="rf"
    

### 4\. Model comparison (confusion matrix)

See how many correct prediction and some evaluation indexes:

```sql
confusionMatrix(previsioni, testing$type)
```