# GettingCleaningDataRProject
##### The purpose of this project is to demonstrate your ability to collect, work with, and clean a data set. The goal is to 
##### prepare tidy data that can be used for later analysis. You will be graded by your peers on a series of yes/no questions 
##### related to the project. You will be required to submit: 1) a tidy data set as described below, 2) a link to a Github 
##### repository with your script for performing the analysis, and 3) a code book that describes the variables, the data, 
##### and any transformations or work that you performed to clean up the data called CodeBook.md. 
##### You should also include a README.md in the repo with your scripts. 
##### This repo explains how all of the scripts work and how they are connected.
## Script for Getting and Cleaning Data in R Project
## Check the work directory
getwd()
## install and bring into library all necessary packages for script execution if not already done
install.packages("data.table")
install.packages("dplyr")
library(data.table)
library(dplyr)
## Double check if a project directory already created before creating one to store all necessary files for the analysis
if(!file.exists("projectdir")){dir.create("projectdir")}
## Download, unzip and read all data files
zipurl<-"https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip"
download.file(zipurl, dest="./projectdir/projectdata.zip")
unzip ("./projectdir/projectdata.zip", exdir = "./projectdir")
list.files("./projectdir")
## It appears that the data files and more information about the data under the README.txt file are all under UCI HAR Dataset
list.files("./projectdir/UCI HAR Dataset")
## Get familiar with the content by reading README
list.files("./projectdir/UCI HAR Dataset/test")
list.files("./projectdir/UCI HAR Dataset/train")
## Reading and getting familiar with data for subjects
subject_test_dt<-read.table("./projectdir/UCI HAR Dataset/test/subject_test.txt")
head(subject_test_dt)
str(subject_test_dt)
table(subject_test_dt)
subject_train_dt<-read.table("./projectdir/UCI HAR Dataset/train/subject_train.txt")
head(subject_train_dt)
str(subject_train_dt)
table(subject_train_dt)
## Reading and getting familiar with data sets (x) and the corresponding labels (integers and activity name) to reappend for both test and training groups
X_test_dt<-read.table("./projectdir/UCI HAR Dataset/test/X_test.txt")
head(X_test_dt)
str(X_test_dt)
X_train_dt<-read.table("./projectdir/UCI HAR Dataset/train/X_train.txt")
head(X_train_dt)
str(X_train_dt)
Y_test_dt<-read.table("./projectdir/UCI HAR Dataset/test/Y_test.txt")
head(Y_test_dt)
str(Y_test_dt)
table(Y_test_dt)
Y_train_dt<-read.table("./projectdir/UCI HAR Dataset/train/Y_train.txt")
head(Y_train_dt)
str(Y_train_dt)
table(Y_train_dt)
activity_labels_dt<-read.table("./projectdir/UCI HAR Dataset/activity_labels.txt")
head(activity_labels_dt)
## Part 1. Merge the training and the test sets to create one data set.
### Merge test subject, label integers and feature measures
test_dt <- cbind(as.data.table(subject_test_dt),Y_test_dt, X_test_dt)
### Merge training subject, label integers and feature measures
training_dt <- cbind(as.data.table(subject_train_dt),Y_train_dt, X_train_dt)
### Merge test and training data frames together using row bind
testntraining_dt<-rbind(test_dt,training_dt)
## Part 2. Extract only the measurements on the mean and standard deviation for each measurement.
### Modify header vector to now include "subject", "activity" and the feature names.
### then append the new vector of header names onto the merged data set
### Create a subset of data by only keeping subject, activity columns along with mean and std columns using the data manipulation verb 'select' from the dplyr package since it is simpler to do than using straight R coding
features_dt<-read.table("./projectdir/UCI HAR Dataset/features.txt")
features_dt ## exclude the column with the observation number
features_dt[,2]
featurenames<-as.character(features_dt[,2])
header<-c("subject","activity",featurenames)
colnames(testntraining_dt)<-c("subject","activity",featurenames)
df<-data.frame(testntraining_dt)
xtr<- select(df,contains("subject"), contains("activity"), contains(".mean."), contains(".std."))
str(xtr) 
## Part 3. Append descriptive activity names provided in the activity_labels file to name the activities ( in integer form) in the data set
xtr2<-merge(activity_labels_dt,xtr,by.x="V1",by.y="activity",all=TRUE)
## Part 4. Appropriately label the data set with descriptive variable name such as displaying activity_id, activity_name, subject_id, and spelling out t as time and f as frequency in the feature variables.
names(xtr2)<-gsub("V1", "activity_id", names(xtr2))
names(xtr2)<-gsub("V2", "activity_name", names(xtr2))
names(xtr2)<-gsub("subject", "subject_id", names(xtr2))
names(xtr2)<-gsub("^t", "time", names(xtr2))
names(xtr2)<-gsub("^f", "frequency", names(xtr2))
### Double check the renaming steps worked
names(xtr2)
## Part 5. From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.
### Install the reshape 2 package that will allow user to reshape the data set my melting it and casting it into the right groupings for the calculation of the mean
install.packages("reshape2")
library(reshape2)
xtr2Melt<-melt(xtr2,id=c("subject_id","activity_name"),measure.vars=colnames(xtr2[,4:69]))
head(xtr2Melt)
tidydataq5<-dcast(xtr2Melt,subject_id+activity_name~variable,mean)
write.table(tidydataq5,file="tidy_dataset.txt")
