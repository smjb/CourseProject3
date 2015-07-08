# CodeBook for Getting and Cleaning Data Course Project


----------


## Background
The *run_analysis.R* script will run on the data from collected from accelerometer in Samsung Galaxy S as described in 
	http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones
	
The dataset is downloadable from 
     https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip

**This document must be read together with the original data set description linked above to understand the data.**


----------


## Preprocessing
The dataset contains multiple files and two sub folders, namely _train_ and _test_.
In each subfolders, there are 3 data files (*subject_test*, *X_[file]* and *y_[file]*) and another subfolder called _Inertial Signal_. 
We assume that the three files are joined  by its row number (It is supported based on the identitical number of rows in these files)

The script will ignore the data in _Inertial_Signal_.

### X_file
This dataset contains the some estimations of the filtered measurements (in time and frequency for each Cartesian axis)from the embedded accelerometer and gyroscope. 
Each row represent the estimations of each Subject doing a Specific Activity during an interval. 
There are 561 different estimations in each observation

### y_file
This dataset contains the activity code during the measurement made in X_file

### subject_test
This dataset contains the Subject ID during the measurement made in X_file

The data from these 3 files are merged together (wide format) using **cbind**
The wide test and train data is then combined using **rbind**

### Naming the rows
#### Estimation names
The script will load features.txt to get the estimation name and then uses it to define the column in the full wide dataset.
However, the estimation names in features.txt contains characters difficult to manipulate in R. The script will strip all these characters and replace some of them with "_". 
The replacement is purely arbitrary as it depends to the script writer self naming convention.
In this script, all parenthesis characters ( "(" and ")" )are stripped while comma and hyphen (",", "-") are replaced with "_".
This is achieved by 
> features$V2 <- gsub("[)(]", "", features$V2)
> features$V2 <- gsub("[,-]", "_", features$V2)

The full column names after manipulation can be found at the end of this document 

####Activity description
The file *activity_labels.txt* contains the Activity Code and its description. The script will load *activity_labels.txt* and use merge function to replace the activity code column in the full dataset (Column 2) with its description.

----------


## Selecting rows related to mean and standard deviation
The assignment requires subsetting of mean and standard deviation columns. 
As per the original data set description, the script will seach for the keywords "mean" and "std" within the estimation names. 
However there are few estimation names that contains the same search words but does not infer to average of measurement. 
The estimations with names like "meanFreq" and "angle" must not be selected for further analysis.
The intersect between [ *column with **mean** or **std** in name* ] and [ *column without **freq** and **angle** in name* ] will select the columns required for the assignment

## Final Output
The final output is a table of 35 observations and 68 measurements, where the average value of each mean.and.std subset measurement are generated and 
they are grouped by the Activity made and the Subject ID


----------
 
## Data 


# R-friendly Estimation names 

*Please refer to the main documentation in http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones*

```

ActivityDesc
Subject
tBodyAcc_mean_X
tBodyAcc_mean_Y
tBodyAcc_mean_Z
tBodyAcc_std_X
tBodyAcc_std_Y
tBodyAcc_std_Z
tGravityAcc_mean_X
tGravityAcc_mean_Y
tGravityAcc_mean_Z
tGravityAcc_std_X
tGravityAcc_std_Y
tGravityAcc_std_Z
tBodyAccJerk_mean_X
tBodyAccJerk_mean_Y
tBodyAccJerk_mean_Z
tBodyAccJerk_std_X
tBodyAccJerk_std_Y
tBodyAccJerk_std_Z
tBodyGyro_mean_X
tBodyGyro_mean_Y
tBodyGyro_mean_Z
tBodyGyro_std_X
tBodyGyro_std_Y
tBodyGyro_std_Z
tBodyGyroJerk_mean_X
tBodyGyroJerk_mean_Y
tBodyGyroJerk_mean_Z
tBodyGyroJerk_std_X
tBodyGyroJerk_std_Y
tBodyGyroJerk_std_Z
tBodyAccMag_mean
tBodyAccMag_std
tGravityAccMag_mean
tGravityAccMag_std
tBodyAccJerkMag_mean
tBodyAccJerkMag_std
tBodyGyroMag_mean
tBodyGyroMag_std
tBodyGyroJerkMag_mean
tBodyGyroJerkMag_std
fBodyAcc_mean_X
fBodyAcc_mean_Y
fBodyAcc_mean_Z
fBodyAcc_std_X
fBodyAcc_std_Y
fBodyAcc_std_Z
fBodyAccJerk_mean_X
fBodyAccJerk_mean_Y
fBodyAccJerk_mean_Z
fBodyAccJerk_std_X
fBodyAccJerk_std_Y
fBodyAccJerk_std_Z
fBodyGyro_mean_X
fBodyGyro_mean_Y
fBodyGyro_mean_Z
fBodyGyro_std_X
fBodyGyro_std_Y
fBodyGyro_std_Z
fBodyAccMag_mean
fBodyAccMag_std
fBodyBodyAccJerkMag_mean
fBodyBodyAccJerkMag_std
fBodyBodyGyroMag_mean
fBodyBodyGyroMag_std
fBodyBodyGyroJerkMag_mean
fBodyBodyGyroJerkMag_std

```
