#CodeBook

This file contains the variables used for this project and a list of the manipulations done to the raw data.
The manipulation of the raw data assumes the package dlyr is installed in the host. The first step is to create a directory
that will hold the raw data and the results. This directory is called "Project" and the script does check if it exists, if it
does not, it will create one, otherwise it will use the existing folder.
The script then calls the URL of the data set repository and downloads it to disk
#the following chunk of code downloads the original data set
if(!file.exists("./Project")){dir.create("./Project")}
fileUrl <-"https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip"
download.file(fileUrl,destfile="./Project/getdata-projectfiles-UCI HAR Dataset.zip")
unzip("./Project/getdata-projectfiles-UCI HAR Dataset.zip",exdir = "./Project")

#the next step is to read the files into R.loading the train data
X.train <-read.table("./Project/UCI HAR Dataset/train/X_train.txt")
y.train <-read.table("./Project/UCI HAR Dataset/train/y_train.txt")
subject.train <-read.table("./Project/UCI HAR Dataset/train/subject_train.txt")

#loading the test data
X.test <-read.table("./Project/UCI HAR Dataset/test/X_test.txt")
y.test <-read.table("./Project/UCI HAR Dataset/test/y_test.txt")
subject.test <-read.table("./Project/UCI HAR Dataset/test/subject_test.txt")

#loading activity and variable names
variables<-read.table("./Project/UCI HAR Dataset/features.txt")
activities<-read.table("./Project/UCI HAR Dataset/activity_labels.txt")

#joining the train and test data and then the whole set
train_df<-cbind(subject.train, y.train, X.train)
test_df<-cbind(subject.test, y.test, X.test)
whole_set_df<-rbind(train_df,test_df)

#setting the right names for the variables.
colnames(whole_set_df)<-c("subject","activity",as.character(variables$V2))

#extracting ID, activity, mean and std columns and these to create the data frame with relevant variables only
means <- grep("[Mm]ean",names(whole_set_df),value = FALSE)
stds<-grep("[Ss]td",names(whole_set_df))
colms<-sort(c(1,2,means,stds))
filt_set_df<-whole_set_df[, colms]

#replacing activity names in dataset
names(activities)<-c("ID","activity")
clean<-merge(activities,filt_set_df,by.x = "ID",by.y = "activity")
clean<-clean[,-1]

#changing to descriptive names
names(clean)<-gsub("f[B]*","frequencyB",names(clean))
names(clean)<-gsub("^t[B]","timeB",names(clean))
names(clean)<-gsub("^t[G]","timeG",names(clean))
names(clean)<-gsub("-","",names(clean))
names(clean)<-gsub("[()]","",names(clean))
names(clean)<-gsub(",","",names(clean))
names(clean)<-gsub("Acc","Acceleration",names(clean))

#at this point the data is tidy and now clean all unused variables
rm(y.train,y.test,X.test,X.train,whole_set_df,variables,train_df,test_df,
   subject.test,subject.train,stds,means,filt_set_df,fileUrl,colms,activities)

#create summary of data and save to disk
action<-group_by(clean,activity,subject)
final_data<-summarize_each(action,funs(mean))
write.csv(final_data,"./Project/final_data.csv")