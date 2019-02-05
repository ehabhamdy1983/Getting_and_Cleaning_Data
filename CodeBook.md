## Set Working directory to the location of assignment files
setwd("C:/Users/Ehab/Desktop/Data Science/Cleaning/assignment")
## Show available folders
dir()

## Read column names
cnames<-read.table("features.txt")

## Read test files
setwd("test")
dir()
subtest<-read.table("subject_test.txt")
xtest<-read.table("X_test.txt")
ytest<-read.table("y_test.txt")

## Set column names - objective 3
colnames(xtest)<-cnames[,2]
colnames(ytest)<-"activity_label"
colnames(subtest)<-"subject"

## Megre test files
alltest<-cbind(ytest,subtest,xtest)

## Read train files
setwd("..")
setwd("train")
dir()
subtrain<-read.table("subject_train.txt")
xtrain<-read.table("X_train.txt")
ytrain<-read.table("y_train.txt")

## Set column names - - objective 3
colnames(xtrain)<-cnames[,2]
colnames(ytrain)<-"activity_label"
colnames(subtrain)<-"subject"

## Mearge train files
alltrain<-cbind(ytrain,subtrain,xtrain)

## Megre all data - Objective 1
alldata<-rbind(alltest,alltrain)

## Define a logical array to determine which columns to grab
cnames<-colnames(alldata)
grabinfo<-(grepl("activity_label",cnames)|grepl("subject",cnames)|grepl("mean..",cnames)
           |grepl("std..",cnames))
## Extract the measurements on mean and standard deviation for each row- objective 2
mean_and_std<-alldata[,grabinfo==TRUE]

## Read activity labels
activitylabels<-read.table("activity_labels.txt")
colnames(activitylabels)=c("activity_label","act_label")

## Assign appropriate activity labels to mean_and_std dataset - objective 4
mean_and_std<-merge(mean_and_std,activitylabels,by='activity_label',all.x=TRUE)

## Write the file with all merged data
write.table(alldata,"all_data.txt",row.name=FALSE)
