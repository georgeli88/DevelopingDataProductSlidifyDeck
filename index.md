---
title       : Developing Data Products Course Project
subtitle    : Shiny App Presentation with Slidify
author      : George Li
job         : 
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
url:
  lib: ./libraries
  assets: ./assets
widgets     : [mathjax]     # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---

## Shiny App Introduction

This simple shiny app uses two regression models to predict the duration of eruption time for the famous Old Faithful geyser in Yellowstone National Park.   

- simple linear regression model (lm)  
- random forest  

The regression model uses the dataset **faithful** from  **caret** library with 272 observations. The dataset is partitioned with 70% as train dataset and 30% as test dataset.


```r
library(caret); library(randomForest); set.seed(12345)
inTrain <- createDataPartition(y=faithful$waiting, p=0.70, list=FALSE)
trainFaith <- faithful[inTrain, ]
testFaith <- faithful[-inTrain, ]
```

---  

## Model's Test Set Error Comparison

- Root mean squared error (RMSE) on test set of Linear Regression Model (lm)


```r
model1 <- lm(eruptions ~ waiting, data=trainFaith)
prediction1 <- predict(model1, newdata=testFaith)
sqrt(sum((prediction1 - testFaith$eruptions)^2))
```

```
## [1] 4.571707
```

- Root mean squared error (RMSE) on test set of randomForest Model

```r
model2 <- randomForest(eruptions ~ waiting, data=trainFaith)
prediction2 <- predict(model2, newdata=testFaith)
sqrt(sum((prediction2 - testFaith$eruptions)^2))
```

```
## [1] 3.700957
```

---  

## Develop and Deploy the Shiny App
- The following packages and libraries are needed to develop and run the app 
    - library(shiny)  
    - library(devtools)  
    - library(shinyapps)  
    - library(caret)  
    - library(randomForest)  
- Ceate two R files `ui.R` and `server.R` in the same directory  
- Run command `runApp()` in the directory to debug the app
- Run command `deployApp()` to deploy the Shiny app to Shinyapps.io site at [http://georgeli88.shinyapps.io/oldfaithful](http://georgeli88.shinyapps.io/oldfaithful)

---  

## How to Use the App

<img class=center src=./assets/img/Oldfaithful_ShinyApp.png height=300>

- Enter a positive number for the waiting time on [http://georgeli88.shinyapps.io/oldfaithful](http://georgeli88.shinyapps.io/oldfaithful)
- Select the regression model, "lm" or "randomForest"  
- Click "Submit" button  
- The predicted eruption time is displayed in the main panel   
- If an non-positive number is entered, a warning message is displayed in the main panel 



