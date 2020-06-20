Q1:

Downloading data and creating one tidy dataset

> t1<-read.csv2("UCI HAR Dataset_test_subject_test.txt",header=FALSE)
> str(t1)
'data.frame':   2947 obs. of  1 variable:
 $ V1: int  2 2 2 2 2 2 2 2 2 2 ...

> t2<-read.csv("UCI HAR Dataset_test_X_test.txt",header=FALSE,sep="")
> dim(t2)
[1] 2947  561

> t3<-read.csv2("UCI HAR Dataset_test_y_test.txt",header=FALSE)
> str(t3)
'data.frame':   2947 obs. of  1 variable:
 $ V1: int  5 5 5 5 5 5 5 5 5 5 ...

> testd<-cbind(t1,t2,t3)
> str(testd)
'data.frame':   2947 obs. of  3 variables:
 $ V1: int  2 2 2 2 2 2 2 2 2 2 ...
 $ V1: chr  "  2.5717778e-001 -2.3285230e-002 -1.4653762e-002 -9.3840400e-001 -9.2009078e-001 -6.6768331e-001 -9.5250112e-00"| __truncated__ "  2.8602671e-001 -1.3163359e-002 -1.1908252e-001 -9.7541469e-001 -9.6745790e-001 -9.4495817e-001 -9.8679880e-00"| __truncated__ "  2.7548482e-001 -2.6050420e-002 -1.1815167e-001 -9.9381904e-001 -9.6992551e-001 -9.6274798e-001 -9.9440345e-00"| __truncated__ "  2.7029822e-001 -3.2613869e-002 -1.1752018e-001 -9.9474279e-001 -9.7326761e-001 -9.6709068e-001 -9.9527433e-00"| __truncated__ ...
 $ V1: int  5 5 5 5 5 5 5 5 5 5 ...
> dim(testd)
[1] 2947  563

> tr1<-read.csv2("UCI HAR Dataset_train_subject_train.txt", header=FALSE)
> str(tr1)
'data.frame':   7352 obs. of  1 variable:
 $ V1: int  1 1 1 1 1 1 1 1 1 1 ...

> tr2<-read.csv2("UCI HAR Dataset_train_X_train.txt",header=FALSE,sep="")
> str(tr2)
'data.frame':   7352 obs. of  1 variable:
 $ V1: chr  "  2.8858451e-001 -2.0294171e-002 

> tr3<-read.csv2("UCI HAR Dataset_train_y_train.txt",header=FALSE)
> str(tr3)
'data.frame':   7352 obs. of  1 variable:
 $ V1: int  5 5 5 5 5 5 5 5 5 5 ...

> traind<-cbind(tr1,tr2,tr3)
> str(traind)
'data.frame':   7352 obs. of  3 variables:
 $ V1: int  1 1 1 1 1 1 1 1 1 1 ...
 $ V1: chr  "  2.8858451e-001 -2.0294171e-002 -1.3290514e-001 -9.9527860e-001 -9.8311061e-001 -9.1352645e-001 -9.9511208e-00"| __truncated__ "  2.7841883e-001 -1.6410568e-002 -1.2352019e-001 -9.9824528e-001 -9.7530022e-001 -9.6032199e-001 -9.9880719e-00"| __truncated__ "  2.7965306e-001 -1.9467156e-002 -1.1346169e-001 -9.9537956e-001 -9.6718701e-001 -9.7894396e-001 -9.9651994e-00"| __truncated__ "  2.7917394e-001 -2.6200646e-002 -1.2328257e-001 -9.9609149e-001 -9.8340270e-001 -9.9067510e-001 -9.9709947e-00"| __truncated__ ...
 $ V1: int  5 5 5 5 5 5 5 5 5 5 ...
> dim(traind)
[1] 7352  563

> data<-rbind(testd,traind) ## One almost tidy dataset with 563 columns and 10299 rows is created. Almost tidy because variables does not have names yet.
> dim(data)
[1] 10299   563

> summary(data)

## added "subject" in the first row and "activity" in the last row of the features file

> tdata<-t(data)
> dim(tdata)
[1]   563 10299

Q3,Q4:

> t4<-read.csv2("UCI HAR Dataset_features.txt",header=FALSE,sep="") ## downloads names as a column
> dim(t4)
[1] 563   2

> t5<-cbind(t4,tdata)
> t5$V1<-NULL
> t6<-t(t5)
> colnames(t6) <- t6[1,] ## All 563 columns have now names in line with the features file.
> t7<-t6[-1,] ## We are deleting the first row which contains names. The dataset is now tidy.
> dim(t7)
[1] 10299   563

Q2
> install.packages("dplyr")
> library(dplyr)
> install.packages("tidyverse")
> library(tidyverse)
> t8 <- as_tibble(t7)
> t9<-select(t8,contains(c("mean","std"))) ## Selects all columns containing "mean" and "std" and creates a separate dataset t9.

Q5
> by_s_a <- t8 %>% group_by(subject, activity)
> head(by_s_a)
> t11<-summarize(by_s_a,mean(c(2:562)))

> write.table(data,file = "nonamedata.txt",row.name=FALSE)
> write.table(t7,file = "tidydata.txt",row.name=FALSE)
> write.table(t11,file = "groupaveragesdata.txt",row.name=FALSE)
