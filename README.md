# GettingAndCleaningData Course Project Week 4

This is the code along with descriptions for the data in the final project for Getting and Cleaning Data. 

All variables are listed as follows in the final data when the run_analysis.R is run with the appropriate wd() set up. 

 [1] "Activity"                        "tBodyAcc-std()-X"                "tBodyAcc-std()-Y"               
 [4] "tBodyAcc-std()-Z"                "tGravityAcc-std()-X"             "tGravityAcc-std()-Y"            
 [7] "tGravityAcc-std()-Z"             "tBodyAccJerk-std()-X"            "tBodyAccJerk-std()-Y"           
[10] "tBodyAccJerk-std()-Z"            "tBodyGyro-std()-X"               "tBodyGyro-std()-Y"              
[13] "tBodyGyro-std()-Z"               "tBodyGyroJerk-std()-X"           "tBodyGyroJerk-std()-Y"          
[16] "tBodyGyroJerk-std()-Z"           "tBodyAccMag-std()"               "tGravityAccMag-std()"           
[19] "tBodyAccJerkMag-std()"           "tBodyGyroMag-std()"              "tBodyGyroJerkMag-std()"         
[22] "fBodyAcc-std()-X"                "fBodyAcc-std()-Y"                "fBodyAcc-std()-Z"               
[25] "fBodyAccJerk-std()-X"            "fBodyAccJerk-std()-Y"            "fBodyAccJerk-std()-Z"           
[28] "fBodyGyro-std()-X"               "fBodyGyro-std()-Y"               "fBodyGyro-std()-Z"              
[31] "fBodyAccMag-std()"               "fBodyBodyAccJerkMag-std()"       "fBodyBodyGyroMag-std()"         
[34] "fBodyBodyGyroJerkMag-std()"      "tBodyAcc-mean()-X"               "tBodyAcc-mean()-Y"              
[37] "tBodyAcc-mean()-Z"               "tGravityAcc-mean()-X"            "tGravityAcc-mean()-Y"           
[40] "tGravityAcc-mean()-Z"            "tBodyAccJerk-mean()-X"           "tBodyAccJerk-mean()-Y"          
[43] "tBodyAccJerk-mean()-Z"           "tBodyGyro-mean()-X"              "tBodyGyro-mean()-Y"             
[46] "tBodyGyro-mean()-Z"              "tBodyGyroJerk-mean()-X"          "tBodyGyroJerk-mean()-Y"         
[49] "tBodyGyroJerk-mean()-Z"          "tBodyAccMag-mean()"              "tGravityAccMag-mean()"          
[52] "tBodyAccJerkMag-mean()"          "tBodyGyroMag-mean()"             "tBodyGyroJerkMag-mean()"        
[55] "fBodyAcc-mean()-X"               "fBodyAcc-mean()-Y"               "fBodyAcc-mean()-Z"              
[58] "fBodyAcc-meanFreq()-X"           "fBodyAcc-meanFreq()-Y"           "fBodyAcc-meanFreq()-Z"          
[61] "fBodyAccJerk-mean()-X"           "fBodyAccJerk-mean()-Y"           "fBodyAccJerk-mean()-Z"          
[64] "fBodyAccJerk-meanFreq()-X"       "fBodyAccJerk-meanFreq()-Y"       "fBodyAccJerk-meanFreq()-Z"      
[67] "fBodyGyro-mean()-X"              "fBodyGyro-mean()-Y"              "fBodyGyro-mean()-Z"             
[70] "fBodyGyro-meanFreq()-X"          "fBodyGyro-meanFreq()-Y"          "fBodyGyro-meanFreq()-Z"         
[73] "fBodyAccMag-mean()"              "fBodyAccMag-meanFreq()"          "fBodyBodyAccJerkMag-mean()"     
[76] "fBodyBodyAccJerkMag-meanFreq()"  "fBodyBodyGyroMag-mean()"         "fBodyBodyGyroMag-meanFreq()"    
[79] "fBodyBodyGyroJerkMag-mean()"     "fBodyBodyGyroJerkMag-meanFreq()"

#run_analysis.R
#installing packages for data manipulation
library(dplyr)
library(lubridate)
library(stringr)
library(tidyr)
library(regexr)
getwd() 
setwd("C:/Users/Bjaco/Documents/R/Practice") #setting working directory

#importing flatfiles 
dataTest         <- read.table("../Practice/UCI HAR Dataset/test/X_test.txt", header=F)
dataTrain        <- read.table("../Practice/UCI HAR Dataset/train/X_train.txt", header=F)
subjectTest      <- read.table("../Practice/UCI HAR Dataset/test/subject_test.txt", header=F) #contains test subject information, merge with test
subjectTrain     <- read.table("../Practice/UCI HAR Dataset/train/subject_train.txt", header=F) # container train subject information, merge with train
namesTest        <- read.table("../Practice/UCI HAR Dataset/test/y_test.txt", header=F) #contains test labels
namesTrain       <- read.table("../Practice/UCI HAR Dataset/train/y_train.txt", header=F) #contains train labels
features         <- read.table("../Practice/UCI HAR Dataset/features.txt", header=F) #basically columnames for test/train

#adding correct colnames from feature.txt
names(dataTest) <-features[,2] 
names(dataTrain)<-features[,2] 

#combining test and respective test/train data
dataTest <-cbind(namesTest, dataTest)
dataTrain <-cbind(namesTrain, dataTrain)
colnames(dataTest)[1]<-"Activity"
colnames(dataTrain)[1]<-"Activity"

#reading in activities
activityLabels   <- read.table("../Practice/UCI HAR Dataset/activity_labels.txt", header=F) #activies

#adding activity strings to data -this could be done programmatically better with if/indices, but running low on time
dataTest$Activity[dataTest$Activity == 6] <- "laying" 
dataTest$Activity[dataTest$Activity == 5] <- "standing"
dataTest$Activity[dataTest$Activity == 4] <- "sitting"
dataTest$Activity[dataTest$Activity == 3] <- "walking downstairs"
dataTest$Activity[dataTest$Activity == 2] <- "walking upstairs"
dataTest$Activity[dataTest$Activity == 1] <- "walking"

dataTrain$Activity[dataTrain$Activity == 6] <- "laying"
dataTrain$Activity[dataTrain$Activity == 5] <- "standing"
dataTrain$Activity[dataTrain$Activity == 4] <- "sitting"
dataTrain$Activity[dataTrain$Activity == 3] <- "walking downstairs"
dataTrain$Activity[dataTrain$Activity == 2] <- "walking upstairs"
dataTrain$Activity[dataTrain$Activity == 1] <- "walking"
View(dataTest) #viewing data

#mergin the two datasets
mergedData<-rbind(dataTest,dataTrain)
View(mergedData) #dims also checked in Global Environment at 10299X562, 2947+7352=10299. Looks good

#subsetting for str and mean (including Freqmean), joining, and checking
sub<-mergedData[,grepl("std", colnames(mergedData))]
sub2<-mergedData[,grepl("mean", colnames(mergedData))]
finalData <- cbind(mergedData[1],sub,sub2) #merging data for part including activity
names(finalData); dim(finalData) #quick dim check

#part 5 subsetting for averages and grouped by each variable to get summary
averageData <-finalData %>% group_by(Activity) %>% summarise_each(funs(mean))
View(averageData)
