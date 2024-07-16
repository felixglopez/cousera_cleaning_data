# cousera_cleaning_data
module getting and cleaning data - coursera


#Define the URL and destination file

setwd("~/Documents/GitHub/coursera_cleaning_data_project")

fileUrl <-"https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip" destfile <- "~/Documents/GitHub/coursera_cleaning_data_project/file.zip"

#Download the file
download(fileUrl, destfile)

#The file is in the directory. Now, beginning the task

library(dplyr)

#Now I will assign all data frames necessary to do the task
#FEATURES 
features <- read.table("~/Documents/GitHub/coursera_cleaning_data_project/UCI HAR Dataset/features.txt", col.names = c("n","functions"))

#ACTIVITIES 
activities <- read.table("~/Documents/GitHub/coursera_cleaning_data_project/UCI HAR Dataset/activity_labels.txt", col.names = c("code", "activity"))

#SUBJECT TEST, X_TEST AND Y_TEST 
subject_test <- read.table("/Documents/GitHub/coursera_cleaning_data_project/UCI HAR Dataset/test/subject_test.txt", col.names = "subject") x_test <- read.table("/Documents/GitHub/coursera_cleaning_data_project/UCI HAR Dataset/test/X_test.txt", col.names = features$functions) y_test <- read.table("/Documents/GitHub/coursera_cleaning_data_project/UCI HAR Dataset/test/y_test.txt", col.names = "code") subject_train <- read.table("/Documents/GitHub/coursera_cleaning_data_project/UCI HAR Dataset/train/subject_train.txt", col.names = "subject")

TRAIN DATASETS (X_TRAIN AND Y_TRAIN)
x_train <- read.table("/Documents/GitHub/coursera_cleaning_data_project/UCI HAR Dataset/train/X_train.txt", col.names = features$functions) y_train <- read.table("/Documents/GitHub/coursera_cleaning_data_project/UCI HAR Dataset/train/y_train.txt", col.names = "code")

#STAGE 1 - I will merge the training and testing datasets into one dataset (step 1)

row bind xtrain and xtest
X <- rbind(x_train, x_test) Y <- rbind(y_train, y_test) Subject <- rbind(subject_train, subject_test)

#NOW I'M MERGING ALL DATA INTRO ONE DATASET Merged_Data <- cbind(Subject, Y, X)

Step 2 is to get the mean and SD for each measurement. I do this by creating the TidyData object

TidyData <- Merged_Data %>% select(subject, code, contains("mean"), contains("std"))

View(TidyData)

#The next step is to use descriptive activity names to name the activities in the existing dataset

TidyData$code <- activities[TidyData$code, 2]

head(TidyData$code) str(TidyData$code)

I now label the previous dataset with descriptive variable names creating subgroups
#testing names(TidyData)[2] = "activity" names(TidyData)<-gsub("Acc", "Accelerometer", names(TidyData)) names(TidyData)<-gsub("Gyro", "Gyroscope", names(TidyData)) names(TidyData)<-gsub("BodyBody", "Body", names(TidyData)) names(TidyData)<-gsub("Mag", "Magnitude", names(TidyData)) names(TidyData)<-gsub("^t", "Time", names(TidyData)) names(TidyData)<-gsub("^f", "Frequency", names(TidyData)) names(TidyData)<-gsub("tBody", "TimeBody", names(TidyData)) names(TidyData)<-gsub("-mean()", "Mean", names(TidyData), ignore.case = TRUE) names(TidyData)<-gsub("-std()", "STD", names(TidyData), ignore.case = TRUE) names(TidyData)<-gsub("-freq()", "Frequency", names(TidyData), ignore.case = TRUE) names(TidyData)<-gsub("angle", "Angle", names(TidyData)) names(TidyData)<-gsub("gravity", "Gravity", names(TidyData))

Stage 5 - From the data set in step 4, create a second, independent tidy data set with the average of each variable for each activity and each subject. This is the final.txt file available in my repo. 

Final <- TidyData %>% group_by(subject, activity) %>% summarise_all(funs(mean)) write.table(Final, "~/Documents/GitHub/coursera_cleaning_data_project/Final.txt", row.name=FALSE)

cousera_cleaning_data/codebook.md at main · felixglopez/cousera
_cleaning_data
