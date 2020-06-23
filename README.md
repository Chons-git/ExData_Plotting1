Peer-graded Assignment: Getting and Cleaning Data Course Project
_________________________________________________________________

Q1: First, I download and unzip all data file into the R working directory. Secondly, I bind the parts of the data together and give the column names.
Finally, I generate the tidy raw data for the assignment.

t2<-read.csv("UCI HAR Dataset_test_X_test.txt",header=FALSE,sep="")
tr2<-read.csv2("UCI HAR Dataset_train_X_train.txt",header=FALSE,sep="")
XY2<-rbind(t2,tr2)
t4<-read.csv2("UCI HAR Dataset_features.txt",header=FALSE,sep="")
colnames(XY2) <- t4[,2]

t1<-read.csv2("UCI HAR Dataset_test_subject_test.txt",header=FALSE)
t3<-read.csv2("UCI HAR Dataset_test_y_test.txt",header=FALSE)
testd<-cbind(t1,t3)

tr1<-read.csv2("UCI HAR Dataset_train_subject_train.txt", header=FALSE)
tr3<-read.csv2("UCI HAR Dataset_train_y_train.txt",header=FALSE)
traind<-cbind(tr1,tr3)

XY1<-rbind(testd,traind)
colnames(XY1) <- c("subject", "activity")

data<-cbind(XY1,XY2)

Q2.By applying grep() command I extract data with measurements on the mean and standard deviation for each measurement (data which variable name contains "mean()" or "std()")

install.packages("dplyr")
library(dplyr)
install.packages("tidyverse")
library(tidyverse)

t5 <- data[grep("mean\\()|std\\()",names(data))]

Q3. I convert activity labels to descriptive activity names to name the activities in the data set.

t6<-cbind(XY1,t5)

t6$activity[t6$activity  == 1]  <-  "WALKING"
t6$activity[t6$activity  == 2]  <-  "WALKING_UPSTAIRS"
t6$activity[t6$activity  == 3]  <-  "WALKING_DOWNSTAIRS"
t6$activity[t6$activity  == 4]  <-  "SITTING"
t6$activity[t6$activity  == 5]  <-  "STANDING"
t6$activity[t6$activity  == 6]  <-  "LAYING"

Q4. I appropriately label the following data set with descriptive variable names.

names(t6) <- gsub("^t", "Time", names(t6)) 
names(t6) <- gsub("^f", "Frequence", names(t6)) 
names(t6) <- gsub("mean", "Mean", names(t6)) 
names(t6) <- gsub("std", "Std", names(t6)) 

Q5. From the data set in step 4, I create a second, independent tidy data set with the average of each variable for each activity and each subject and export the data as a textfile.

t6[3:68] <- sapply(t6[3:68],as.numeric)
t7 <- t6 %>% 
	group_by(subject, activity) %>% 
	summarise_all(funs(mean)) 

write.table(t7,file = "tidydata.txt",row.name=FALSE)