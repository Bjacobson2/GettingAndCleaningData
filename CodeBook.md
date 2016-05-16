#code book that describes the variables, the data, and any transformations or work that you performed to clean up the data 

Original Data Source: http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones
Data Dictionary: contained in above link
Downloaded: 5/15/2016

Getting/Cleaning the Data
-data was imported using read.table function
-column names and activites (as described below) were added to both the test/train datasets
-train and test data was merged 
-subsetting for standard deviation (std) and mean (mean) variables was used through dplyr
-finally, the data was summarized by the mean, grouped by activity (using dplyr)
*no analytical transformations apart from the mean were needed. Variable names were maintained where possible.

Experimental infomation available at original data source: 

The experiments have been carried out with a group of 30 volunteers 

within an age bracket of 19-48 years. Each person performed six activities 
(WALKING, WALKING_UPSTAIRS, WALKING_DOWNSTAIRS, SITTING, STANDING, LAYING) 
wearing a smartphone (Samsung Galaxy S II) on the waist. Using its embedded accelerometer
 and gyroscope, we captured 3-axial linear acceleration and 3-axial angular velocity at a constant
 rate of 50Hz. The experiments have been video-recorded to label the data manually. The obtained dataset
 has been randomly partitioned into two sets, where 70% of the volunteers was selected 
for generating the training data and 30% the test data. 

The sensor signals (accelerometer and gyroscope) were pre-processed by applying noise filters and then sampled in fixed-width sliding windows of 2.56 sec and 50% overlap (128 readings/window). The sensor acceleration signal, which has gravitational and body motion components, was separated using a Butterworth low-pass filter into body acceleration and gravity. The gravitational force is assumed to have only low frequency components, therefore a filter with 0.3 Hz cutoff frequency was used. From each window, a vector of features was obtained by calculating variables from the time and frequency domain. See 'features_info.txt' for more details. 

For each record it is provided:
======================================

- Triaxial acceleration from the accelerometer (total acceleration) and the estimated body acceleration.
- Triaxial Angular velocity from the gyroscope. 
- A 561-feature vector with time and frequency domain variables. 
- Its activity label. 
- An identifier of the subject who carried out the experiment.

The dataset includes the following files:
=========================================
- 'features_info.txt': Shows information about the variables used on the feature vector.
- 'features.txt': List of all features.
- 'activity_labels.txt': Links the class labels with their activity name.
- 'train/X_train.txt': Training set.
- 'train/y_train.txt': Training labels.
- 'test/X_test.txt': Test set.
- 'test/y_test.txt': Test labels.
======
- Features are normalized and bounded within [-1,1].
- Each feature vector is a row on the text file.
