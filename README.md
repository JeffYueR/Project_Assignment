Project_Assignment
==================
The zip file was first downloaded manually to the working directory.  The student is not familiar with using the unzip
function to download the zip files.  Any advice on this is welcome.
The R comments are listed in bullets below.

* Set working directory

setwd("E:/Users/JeffPat390039/Documents/Machine_Learning/R/Data_Science/3_Getting_and_Cleaning_Data/Project_Assignment")

library(dplyr)

* Read activity labels. This is a 6 by 2 table

activity_labels <- read.table("./UCI HAR Dataset/activity_labels.txt")

* Read in the features vector. This is 561 by 2 table

features <- read.table("./UCI HAR Dataset/features.txt")

* Read in the training set data
* Read subject_train file, which contains the number of subjects in the training set. This is 7352 by 1 table  

subject_train <- read.table("./UCI HAR Dataset/train/subject_train.txt")

* Read y_train file which contains the activity labels. This is a 7352 by 1 table

y_train <- read.table("./UCI HAR Dataset/train/y_train.txt")

* Read X_train dataset.  This is a 7352 by 561 table

X_train <- read.table("./UCI HAR Dataset/train/X_train.txt")

* Read in the test set data
* Read subject_test file, which contains the number of subjects in the test set. This is 2947 by 1 table  

subject_test <- read.table("./UCI HAR Dataset/test/subject_test.txt")

* Read y_test file which contains the activity labels. This is a 2947 by 1 table

y_test <- read.table("./UCI HAR Dataset/test/y_test.txt")

* Read X_test dataset.  This is a 2947 by 561 table

X_test <- read.table("./UCI HAR Dataset/test/X_test.txt")


* Compile the training data set
* Assign feature names to the columns of X_train

names(X_train) <- features[,2]

* Column bind subject_train and y_train together

train <- cbind(subject_train,y_train)

* Set a temporary vector with length = nrow(train) to assign the activity labels to this vector
* Then column bind the temp vector to train and label the columns of train

temp1 <- vector("character", length = nrow(train))

temp2 <- vector("character", length = nrow(train))

for (i in 1:nrow(train)) {temp1[i] <- as.character(activity_labels[which(train[i,2] == activity_labels),2])
                          temp2[i] <- "Training"}

train <- cbind(train, temp1, temp2)

names(train) <- c("Subject", "Activity_label", "Activity", "Dataset_Type")

* Combine train dataset with X_train dataset. This dataset has a dimension of 7352 by 564

train <- cbind(train, X_train)


* Compile the test data set
* Assign feature names to the columns of X_test

names(X_test) <- features[,2]

* Column bind subject_test and y_test together

test <- cbind(subject_test,y_test)

* Set a temporary vector with length = nrow(test) to assign the activity labels to this vector
* Then column bind the temp vector to test and label the columns of test

temp3 <- vector("character", length = nrow(test))

temp4 <- vector("character", length = nrow(test))

for (i in 1:nrow(test)) {temp3[i] <- as.character(activity_labels[which(test[i,2] == activity_labels),2])
                         temp4[i] <- "Test"}

test <- cbind(test, temp3, temp4)

names(test) <- c("Subject", "Activity_label", "Activity", "Dataset_Type")

* Combine train dataset with X_train dataset. This dataset has a dimension of 2947 by 564

test <- cbind(test, X_test)

* Merge or Combine train and test dataset

merged <- rbind(train, test)

* Select values that contain mean and standard deviation from merged and assign to a variable, mean_std

mean_std <- merged %>% select(Subject, Activity_label, Activity, Dataset_Type, contains("mean"), contains("std"))

* Subset the mean_std dataset to with only the mean variables and arrange by each subject and each activity
* in ascending order

mean_arranged <- mean_std %>% select(Subject, Activity_label, Activity, Dataset_Type, contains("mean")) %>%
                              arrange(Subject, Activity_label)

* Write the data frame mean_arranged into a text file names mean_arranged.txt.  Columns are delimited by space

write.table(mean_arranged, file = "mean_arranged.txt", row.names = FALSE)
