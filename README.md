##README

Refer to run_analysis.R script that contains the R script to:

### Merges the training and the test sets to create one data set.
* Uses rbind to merge the two sets as:
  * dataSet1 <- rbind(testSet, trainSet)


### Extracts only the measurements on the mean and standard deviation for each measurement. 
* Uses the features data to grep for variables having mean() or std() as part of the variable name and then passes to dataSet1 to select only the required variables 
  
  * mean.std <- grep("mean()|std()", as.character(features[,2]))
  * dataSet2 <- dataSet1[,mean.std]



### Uses descriptive activity names to name the activities in the data set
* Following code adds the activity labels to the dataSet2:

  * colnames(activityLabels) <- c("ID","ACTIVITY")
  * colnames(dataSetActivity1) <- c("ID")
  * dataSetActivity2 <- merge(activityLabels, dataSetActivity1)
  * dataSet2$activity <-dataSetActivity2[,2]


### Appropriately labels the data set with descriptive variable names. 
* Following code provides valies names to variables:

  * colnames(dataSet1) <- gsub("\\.$","",gsub("\\.+",".",make.names(as.character(features[,2]))))

### From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.

* the script uses dplyr package to find the variable averages by activity and subject and then outputs the table to file. Refer to run_analysis.R script

