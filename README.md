# GettingAndCleaningData Course Project Week 4

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
