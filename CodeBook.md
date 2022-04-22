---
title: "Building Statistical Models in R Linear Regression"
author: "Ariel Felices"
date: "1/20/2022"
output: word_document
---

# Building Statistical Models in R: Linear Regression

## Task One: Getting Started
In this task, we will learn change the panes and font size.
Also, we will learn how to set and check your current working directory.

### 1.1: Get the working directory

```{r echo=TRUE}
getwd()
```


## Task Two: Import packages and dataset
In this task, we will import the required packages and data for this project.

### 2.1: Importing required packages
```{r}
library(tidyverse)
library(ggpubr)
library(broom)
library(ggfortify)
```

### 2.2: Import the mpg.csv dataset
```{r}
data <- read.csv("mpg.csv", header = T, sep = ",")
```


### 2.3: View and check the dimension of the dataset
```{r}
View(data)
```

```{r}
dim(data) #This will show the number of rows and columns
```


## Task Three: Explore the dataset
In this task, we will learn how to explore and clean the data


### 3.1: Take a peek using the head and tail functions
```{r}
head(data)
```
```{r}
tail(data)
```


### 3.2: Check the internal structure of the data frame
```{r}
str(data)
```


### 3.3: Count missing values in the variables
```{r}
sum(is.na(data)) #To find how many missing values in the whole dataset.
```

```{r}
sapply(data, function(x) sum(is.na(x))) #To find how many missing values in each of the variable, use this function sapply.
```

### 3.4: Check the column names for the data frame
```{r}
colnames(data)
```
OR
```{r}
names(data)
```


### 3.5: Drop the first column of the data frame
```{r}
data <- data[, -1]
```
```{r}
dim(data)
```
```{r}
colnames(data)
```




## Task Four: Data Visualizations
In this task, we will learn how to visualize the variables we will use to build the statistical model.

### 4.1: Plot a scatter plot for the variables with cty on the x-axis hwy on the y-axis
```{r}
ggplot(data = data) +
  geom_point(aes(x = cty, y = hwy)) +
  stat_smooth(aes(x = cty, y = hwy))
```

But to simplify the above code, do the following:
```{r}
ggplot(data, aes(x = cty, y = hwy)) +
  geom_point() +
  stat_smooth()
```


### 4.2: Find the correlation between the variables
```{r}
cor(data$cty, data$hwy) #the correlation coefficient measures the level of association between two variables X and Y.
```

Its value ranges between minus one, that's a perfect negative correlation (when X increases, Y decreases) or plus one, which is a perfect positive correlation (when X increases Y will also increase). A value close to zero suggests a weak relationship between the variables. A low correlation, say between -0.2 to 0.2 probably suggests that much of the variation of the outcome variable Y is not explained by the predictor variable X. In such a case, we will probably look for a better predictor variable. In our own example here, the correlation coefficient is large enough, so we can continue by building a linear model of y, as a function of x.


## Task Five: Model Building
In this task, we will learn how to build a simple linear regression model.


### 5.1: Create a simple linear regression model using the variables use the lm function to determine the beta coefficients of this linear model.
```{r}
model <- lm(hwy ~ cty, data = data)
```
```{r}
model #call the data
```



### 5.2: Plot the regression line for the model
```{r}
ggplot(data, aes(x = cty, y = hwy)) +
  geom_point() +
  stat_smooth(method = lm)
```


## Task Six: Model Assessment I
In this task, we will learn how to assess and interpret the result of a simple linear regression model.

### 6.1: Assess the summary of the fitted model
```{r}
summary(model)
```

### 6.2: Calculate the confidence interval for the coefficients
```{r}
confint(model)
```


## Task Seven: Model Assessment II
In this task, we will learn how to assess the accuracy of a simple linear regression model.

### 7.1: Assess the summary of the fitted model
```{r}
summary(model)
```


### 7.2: Calculate the prediction error of the fitted model
Let's calculate the percentage error to know how much error we have here.
```{r}
sigma(model)*100/mean(data$hwy)
```



## Task Eight: Model Prediction
In this task, we will learn how to check for metrics from the fitted model and make prediction for new values.

### 8.1: Find the fitted values of the simple regression model
```{r}
fitted <- predict.lm(model)
```
```{r}
head(fitted, 3)
```

### 8.2: Find the fitted values of the simple regression model
```{r}
model_diag_metrics <- augment(model)
```
```{r}
head(model_diag_metrics)
```


### 8.3: Visualize the residuals of the fitted model
```{r}
ggplot(model_diag_metrics, aes(cty, hwy)) +
  geom_point() +
  stat_smooth(method = lm, se = FALSE) +
  geom_segment(aes(xend = cty, yend = .fitted), color = "red", size = 0.3)
```

### 8.4: Predict new values using the model
```{r}
predict(
  object = model,
  newdata = data.frame(cty = c(21, 27, 14))
)
```


## Task Nine: Assumptions Check: Diagnostic Plots
In this task, we will learn how to perform diagnostics check on the fitted model


### 9.1: Plotting the fitted model
```{r}
par(mfrow = c(2, 2))   ## This plots the figures in a 2 x 2
plot(model)
```

Better Version
```{r}
autoplot(model)
```


### 9.2: Return par back to default
```{r}
par(mfrow = c(1, 1))
```


### 9.3: Return the first diagnostic plot for the model
```{r}
plot(model, 1)
```

Build another regression model
```{r}
model1 <- lm(hwy ~ sqrt(cty), data = data)
plot(model, 1)
```

### 9.4: Return the second diagnostic plot for the model
```{r}
plot(model, 2)
```

### 9.5: Return the third diagnostic plot for the model
```{r}
plot(model, 3)
```




## Task Ten: Multiple Regression
In this task, we will learn how to build and interpret the results of a multiple regression model.


### 10.1: Build the multiple regression model with hwy on the y-axis and cty and cyl on the x-axis.
```{r}
mul_reg_model <- lm(hwy ~ cty + cyl, data = data)
```

### 10.2: This prints the result of the model
```{r}
mul_reg_model
```

### 10.3: Check the summary of the multiple regression model
```{r}
summary(mul_reg_model)
```

### 10.4: Plot the fitted multiple regression model
```{r}
autoplot(mul_reg_model)
```

