---
title: "Run Analysis"
author: "mcagney"
date: "Friday, February 13, 2015"
output: html_document
---

# Data 
Source of data: https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip 

#Analysis

## Step 1 
Merge the training and the test sets to create one data set training data set is called X_train.txt. Test data set is called X_test.txt. Extract both files from zip folder and placed in my working directory 

```{r}
train <- read.table("X_train.txt")
test <- read.table("X_test.txt")
X <- rbind(train, test)

subect_train <- read.table("UCI HAR Dataset/train/subject_train.txt")
subject_test <- read.table("UCI HAR Dataset/test/subject_test.txt")
S <- rbind(subect_train, subject_test)

ytrain <- read.table("UCI HAR Dataset/train/y_train.txt")
ytext <- read.table("UCI HAR Dataset/test/y_test.txt")
Y <- rbind(ytrain, ytext)
```

## Step 2 
Extract only the measurements on the mean and standard deviation for each measurement

```{r}
features <- read.table("features.txt")
indices_of_good_features <- grep("-mean\\(\\)|-std\\(\\)", features[, 2])
X <- X[, indices_of_good_features]
names(X) <- features[indices_of_good_features, 2]
names(X) <- gsub("\\(|\\)", "", names(X))
names(X) <- tolower(names(X))
```


## Step 3
Use descriptive activity names to name the activities in the data set

```{r}
activities <- read.table("UCI HAR Dataset/activity_labels.txt")
activities[, 2] = gsub("_", "", tolower(as.character(activities[, 2])))
Y[,1] = activities[Y[,1], 2]
names(Y) <- "activity"
```


## Step 4
Appropriately label the data set with descriptive variable names.

```{r}
names(S) <- "subject"
cleaned <- cbind(S, Y, X)
write.table(cleaned, "merged_clean_data.txt")
```


## Step 5 
From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.

```{r}
uniqueSubjects = unique(S)[,1]
numSubjects = length(unique(S)[,1])
numActivities = length(activities[,1])
numCols = dim(cleaned)[2]
result = cleaned[1:(numSubjects*numActivities), ]

row = 1
for (s in 1:numSubjects) {
  for (a in 1:numActivities) {
    result[row, 1] = uniqueSubjects[s]
    result[row, 2] = activities[a, 2]
    tmp <- cleaned[cleaned$subject==s & cleaned$activity==activities[a, 2], ]
    result[row, 3:numCols] <- colMeans(tmp[, 3:numCols])
    row = row+1
  }
}
write.table(result, "data_set_step5.txt")
```

