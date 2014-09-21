run_analysis.R script contains the R script to merge the training and test set.

...{r}
testSet <- read.table("./UCI HAR Dataset/test/X_test.txt",skip=0,header =FALSE)
trainSet <- read.table("./UCI HAR Dataset/train/X_train.txt",skip=0,header =FALSE)
dataSet1 <- rbind(testSet, trainSet)

...