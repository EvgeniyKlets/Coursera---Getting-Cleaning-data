# Coursera---Getting-Cleaning-data
## Getting data file from the web
> fileurl<-"https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip"
> setwd("C:/Users/Evgeniy/Documents/R course - coursera/3. Getting & Cleanning data/5. FINAL Asignment")
> download.file(fileurl,"Assignment.zip")

##Unzipping downloaded file in Working Directory
> unzip("Assignment.zip",exdir = ".")

## Reading file with activity labels
>Set1<-read.table("./UCI HAR Dataset/activity_labels.txt", header=FALSE,sep="")

##Downloading "Features" file
>Set2<-read.table("./UCI HAR Dataset/features.txt",header=FALSE,sep="")

##Reading Subject identifier file of each test
>Set3<-read.csv("./UCI HAR Dataset/test/subject_test.txt", header=FALSE)

##Assigning correct name to Activity code
>names(Set3)<-"Subject Name"

#Reading Test labels file
>Set4<-read.csv("./UCI HAR Dataset/test/y_test.txt", header=FALSE)

##Assigning correct name to Activity code
>Set4a<-merge(Set4, Set1, by="V1", all=TRUE, sort=FALSE)

##Renaming columns in Set4a
>names(Set4a)<-c("Activity Code","Activity Name")

##Reading Test KPIs dataset
>Set5<-read.table("./UCI HAR Dataset/test/X_test.txt", header=FALSE)
>names(Set5)<-Set2$V2

##Creating table with all TEST results
>TEST<-cbind(Set3,Set4a,Set5,"TEST")

##Creating TRAIN dataset
>Set6<-read.csv("./UCI HAR Dataset/train/subject_train.txt", header=FALSE)
>names(Set6)<-"Subject Code"
>Set7<-read.csv("./UCI HAR Dataset/train/y_train.txt", header=FALSE)
>Set7a<-merge(Set7, Set1, by="V1", all=TRUE, sort=FALSE)
>names(Set7a)<-c("Activity Code","Activity Name")
>Set8<-read.table("./UCI HAR Dataset/train/X_train.txt", header=FALSE)
>names(Set8)<-Set2$V2
>TRAIN<-cbind(Set6,Set7a,Set8,"TRAIN")

##CREATING FINAL DATASET
>Final_Database<-merge(TEST,TRAIN, all=TRUE)

##Reworking dataframe to have correct names
>Part1<-Final_Database[,1:3]
>Part2<-Final_Database[,4:564]
>names(Part2)<-Set2$V2
>Part3<-Final_Database[,565:566]
>Final<-cbind(Part1,Part2,Part3)

##Selecting Only mean & STD columns
>Check1<-grep("mean",names(Final))
>Check2<-grep("std",names(Final))
>Check<-c(Check1,Check2)

##Subsetting only Mean & STD columns
>Subset<-Final[,c(1:3,Check)]

##Final subset
##Joining DPLYR package
>library(plyr)
>Subset2<-split(Subset,paste(Subset$`Subject Code`,Subset$`Activity Name`))
>Subset3<-sapply(Subset2,function(x) colMeans(Subset[c(4:82)]))

##Writing file
>write.table(Subset3,file="Final File.txt")
