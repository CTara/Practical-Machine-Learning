---
title: "Practical Machine Learning Course Project"
author: "Chandra Tara Lama"
date: "May 29, 2016"
output:
  html_document:
    
---
###Introduction
Using devices such as Jawbone Up, Nike FuelBand, and Fitbit it is now possible to collect a large amount of data about personal activity relatively inexpensively. These type of devices are part of the quantified self movement – a group of enthusiasts who take measurements about themselves regularly to improve their health, to find patterns in their behavior, or because they are tech geeks.

###Data
The data sets was originated from here:
Ugulino, W.; Cardador, D.; Vega, K.; Velloso, E.; Milidiu, R.; Fuks, H. Wearable Computing: Accelerometers' Data Classification of Body Postures and Movements. Proceedings of 21st Brazilian Symposium on Artificial Intelligence. Advances in Artificial Intelligence - SBIA 2012
Read more: http://groupware.les.inf.puc-rio.br/har#ixzz40iHm7JQz

###Goal
For this project, we are given data from accelerometers on the belt, forearm, arm, and dumbell of 6 research study participants. Our training data consists of accelerometer data and a label identifying the quality of the activity the participant was doing. Our testing data consists of accelerometer data without the identifying label. Our goal is to predict the labels for the test set observations.



```r
library(ggplot2)
library(caret)
```

```
## Warning: package 'caret' was built under R version 3.2.5
```

```
## Loading required package: lattice
```

```
## Error: package or namespace load failed for 'caret'
```

```r
library(randomForest)
```

```
## Warning: package 'randomForest' was built under R version 3.2.5
```

```
## randomForest 4.6-12
```

```
## Type rfNews() to see new features/changes/bug fixes.
```

```
## 
## Attaching package: 'randomForest'
```

```
## The following object is masked from 'package:ggplot2':
## 
##     margin
```

### Load data 
* Loading data.           
* Remove near zero covariates and those with more than 80% missing values since these variables will not provide much power for prediction.       
* Calculate correlations between each remaining feature to the response, `classe`. Use `spearman` rank based correlation because `classe` is a factor.                 
* Plot the two features that have highest correlation with `classe` and color with `classe` to see if we can separate response based on these features.            


```r
# loading data
training <- read.csv("data/pml-training.csv", row.names = 1)
testing <- read.csv("data/pml-testing.csv", row.names = 1)
# remove near zero covariates
nsv <- nearZeroVar(training, saveMetrics = T)
training <- training[, !nsv$nzv]
# remove variables with more than 80% missing values
nav <- sapply(colnames(training), function(x) if(sum(is.na(training[, x])) > 0.8*nrow(training)){return(T)}else{return(F)})
training <- training[, !nav]
# calculate correlations
cor <- abs(sapply(colnames(training[, -ncol(training)]), function(x) cor(as.numeric(training[, x]), as.numeric(training$classe), method = "spearman")))
```

```r
# plot predictors 
summary(cor)
```

```
## Error in object[[i]]: object of type 'closure' is not subsettable
```

```r
plot(training[, names(which.max(cor))], training[, names(which.max(cor[-which.max(cor)]))], col = training$classe, pch = 19, cex = 0.1, xlab = names(which.max(cor)), ylab = names(which.max(cor[-which.max(cor)])))
```

```
## Error in plot(training[, names(which.max(cor))], training[, names(which.max(cor[-which.max(cor)]))], : object 'training' not found
```











