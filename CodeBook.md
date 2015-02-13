---
title: "Code Book - Getting and Cleaning Data Project"
author: "mcagney"
date: "Friday, February 13, 2015"
output: html_document
---

#Introduction to Project Data 

One of the most exciting areas in all of data science right now is wearable computing. Companies like Fitbit, Nike, and Jawbone Up are racing to develop the most advanced algorithms to attract new users. The data linked to from the course website represent data collected from the accelerometers from the Samsung Galaxy S smartphone. 

A full description is available at the site where the data was obtained: 

http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones 

Here are the data for the project: 

https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip

#Steps of  Analysis

The attached R file (run_analysis.R) performs the following steps to clean up the original data. 
1. Merges the training and the test data sets to create one data set.

2. Extracts only the measurements on the mean and standard deviation for each measurement. There were a total of 561 measurements, but we need to extract only those for mean and standard deviation, such as: 

```{r}

tbodyacc-mean-x 

tbodyacc-mean-y 

tbodyacc-mean-z 

tbodyacc-std-x 

tbodyacc-std-y 

tbodyacc-std-z 

tgravityacc-mean-x 

tgravityacc-mean-y

```

3. Uses descriptive activity names to name the activities in the data set. Descriptive activity  names were taken from the "activity_labels.txt" file in the original data set: 

```{r}

walking

walkingupstairs

walkingdownstairs

sitting

standing

laying

```

4. Appropriately labels the data set with descriptive variable names. All feature names and activity names are converted to lower case; underscores and brackets are removed. 

5. From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.
