##README

Refer to run_analysis.R script that contains the R script to:

### Merges the training and the test sets to create one data set.
* Uses rbind to merge the two sets as:
  dataSet1 <- rbind(testSet, trainSet)


### Extracts only the measurements on the mean and standard deviation for each measurement. 
* Uses the features data to grep for variables having mean() or std() as part of the variable name and then passes to dataSet1 to select only the required variables 
  
  mean.std <- grep("mean()|std()", as.character(features[,2]))
  dataSet2 <- dataSet1[,mean.std]



### Uses descriptive activity names to name the activities in the data set
* Following code adds the activity labels to the dataSet2:

  colnames(activityLabels) <- c("ID","ACTIVITY")
  colnames(dataSetActivity1) <- c("ID")
  dataSetActivity2 <- merge(activityLabels, dataSetActivity1)
  dataSet2$activity <-dataSetActivity2[,2]




### Appropriately labels the data set with descriptive variable names. 
* Following code provides valies names to variables:

  colnames(dataSet1) <- gsub("\\.$","",gsub("\\.+",".",make.names(as.character(features[,2]))))

### From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.

* Following codes using the dplyr package to find the variable averages by activity and subject and then outputs the table to file

library(dplyr)
dataSet3 <- tbl_df(dataSet2)
dataSet3_group <- group_by(dataSet3, activity, subject)
sumDataSet <- summarize(
   dataSet3_group, 
   mean(tBodyAcc.mean.X),
   mean(tBodyAcc.mean.Y),
   mean(tBodyAcc.mean.Z),
   mean(tBodyAcc.std.X),
   mean(tBodyAcc.std.Y),
   mean(tBodyAcc.std.Z),
   mean(tGravityAcc.mean.X),
   mean(tGravityAcc.mean.Y),
   mean(tGravityAcc.mean.Z),
   mean(tGravityAcc.std.X),
   mean(tGravityAcc.std.Y),
   mean(tGravityAcc.std.Z),
   mean(tBodyAccJerk.mean.X),
   mean(tBodyAccJerk.mean.Y),
   mean(tBodyAccJerk.mean.Z),
   mean(tBodyAccJerk.std.X),
   mean(tBodyAccJerk.std.Y),
   mean(tBodyAccJerk.std.Z),
   mean(tBodyGyro.mean.X),
   mean(tBodyGyro.mean.Y),
   mean(tBodyGyro.mean.Z),
   mean(tBodyGyro.std.X),
   mean(tBodyGyro.std.Y),
   mean(tBodyGyro.std.Z),
   mean(tBodyGyroJerk.mean.X),
   mean(tBodyGyroJerk.mean.Y),
   mean(tBodyGyroJerk.mean.Z),
   mean(tBodyGyroJerk.std.X),
   mean(tBodyGyroJerk.std.Y),
   mean(tBodyGyroJerk.std.Z),
   mean(tBodyAccMag.mean),
   mean(tBodyAccMag.std),
   mean(tGravityAccMag.mean),
   mean(tGravityAccMag.std),
   mean(tBodyAccJerkMag.mean),
   mean(tBodyAccJerkMag.std),
   mean(tBodyGyroMag.mean),
   mean(tBodyGyroMag.std),
   mean(tBodyGyroJerkMag.mean),
   mean(tBodyGyroJerkMag.std),
   mean(fBodyAcc.mean.X),
   mean(fBodyAcc.mean.Y),
   mean(fBodyAcc.mean.Z),
   mean(fBodyAcc.std.X),
   mean(fBodyAcc.std.Y),
   mean(fBodyAcc.std.Z),
   mean(fBodyAcc.meanFreq.X),
   mean(fBodyAcc.meanFreq.Y),
   mean(fBodyAcc.meanFreq.Z),
   mean(fBodyAccJerk.mean.X),
   mean(fBodyAccJerk.mean.Y),
   mean(fBodyAccJerk.mean.Z),
   mean(fBodyAccJerk.std.X),
   mean(fBodyAccJerk.std.Y),
   mean(fBodyAccJerk.std.Z),
   mean(fBodyAccJerk.meanFreq.X),
   mean(fBodyAccJerk.meanFreq.Y),
   mean(fBodyAccJerk.meanFreq.Z),
   mean(fBodyGyro.mean.X),
   mean(fBodyGyro.mean.Y),
   mean(fBodyGyro.mean.Z),
   mean(fBodyGyro.std.X),
   mean(fBodyGyro.std.Y),
   mean(fBodyGyro.std.Z),
   mean(fBodyGyro.meanFreq.X),
   mean(fBodyGyro.meanFreq.Y),
   mean(fBodyGyro.meanFreq.Z),
   mean(fBodyAccMag.mean),
   mean(fBodyAccMag.std),
   mean(fBodyAccMag.meanFreq),
   mean(fBodyBodyAccJerkMag.mean),
   mean(fBodyBodyAccJerkMag.std),
   mean(fBodyBodyAccJerkMag.meanFreq),
   mean(fBodyBodyGyroMag.mean),
   mean(fBodyBodyGyroMag.std),
   mean(fBodyBodyGyroMag.meanFreq),
   mean(fBodyBodyGyroJerkMag.mean),
   mean(fBodyBodyGyroJerkMag.std),
   mean(fBodyBodyGyroJerkMag.meanFreq))
colnames(sumDataSet)<-gsub("subject.act.sub","subject",gsub("activity.act.sub","activity",gsub("$",".act.sub",gsub(")","",gsub("mean(","",colnames(sumDataSet),fixed=TRUE),fixed=TRUE)),fixed=TRUE),fixed=TRUE)

write.table(sumDataSet,file = "tidy-data.txt",row.name=FALSE)




