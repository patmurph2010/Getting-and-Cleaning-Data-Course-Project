Getting-and-Cleaning-Data-Course-Project
========================================

Course Project for Getting and Cleaning Data, Dec. 2014
run_analysis <- function() {
  
  extractor <- function(name, subject){         ## this functino will be used to fill tidy_matrix with data relating to the subjects, activity names and the relevant averages.
  keep_rows <- y_bind == name & subject_test_train == subject
  new_array <- test_train[keep_rows,]
  colSums(new_array)/nrow(new_array)
  }
  
  test_vector <- read.table("UCI HAR Dataset/test/X_test.txt") ## lines 13-22 read in to R the necessary tables and bind them where needed.
  train_vector <- read.table("UCI HAR Dataset/train/X_train.txt")
  test_train <- rbind(test_vector, train_vector) ## test_train is a matrix with all data from the two tables combined. 
  activity_labels <- read.table("UCI HAR Dataset/activity_labels.txt")
  features <- read.table("UCI HAR Dataset/features.txt")
  y_train <- read.table("UCI HAR Dataset/train/y_train.txt")
  y_test <- read.table("UCI HAR Dataset/test/y_test.txt")
  y_bind <- rbind(y_test, y_train) ## y_bind is a table that lists all activities in order for the tables.
  subject_train <- read.table("UCI HAR Dataset/train/subject_train.txt")
  subject_test <- read.table("UCI HAR Dataset/test/subject_test.txt")
  subject_test_train <- rbind(subject_test, subject_train) ##subject_test_train is a table that lists all subjects participating in order.
  colnames(test_train) <- features[, 2] ## this line makes the column names of test_train the features that they are measuring.
  mean_or_std <- grepl("mean(", features[, 2], fixed = TRUE) | grepl("std(", features[, 2], fixed = TRUE) ##mean_or_std creates a logical vector describing whether the measurements are on the mean or the standard deviation, which will be used on line 26 to extract only the ones that are. 
  test_train <- test_train[, mean_or_std]
  activity_names <- unique(y_bind[,1]) ## lines 27-33 uniquely order the subjects and activities  so that we can create a matrix for tidy data with organized, labeled data in line 36
  ordered_activity_names <- activity_names[order(activity_names)]
  y_names <- activity_labels[y_bind[,1], 2]
  unique_subject <- unique(subject_test_train[,1])
  ordered_subject <- unique_subject[order(unique_subject)]
  unique_names <- unique(y_names)
  ordered_names <- unique_names[order(unique_names)]
  subject <- rep(ordered_subject, each = length(ordered_names)) ## lines 34 and 35 create vectors with each subject repeated for however many activities there are and the names of the activities repeated for however many subjects there are. 
  names <- rep(ordered_activity_names, length(ordered_subject))
  tidy_matrix <- matrix(data = NA, nrow = length(subject), ncol = ncol(test_train)) ## here we create an empty matrix with dimensions for how many observations and variables we are left with that can be filled out by the following for loop.
  for(i in 1:length(subject)) {       ## this for loop, using the function extractor() created above, fills out the matrix tidy_matrix with the appropriate data.
    tidy_matrix[i, ] <- extractor(names[i], subject[i])
  }
  colnames(tidy_matrix) <- colnames(test_train) ## lines 40-43 assign the column names and row names for our tidy_matrix
  rownames_vector <- vector(length = length(subject))
  for(i in 1:length(subject)) rownames_vector[i] <- paste("Subject", subject[i], "Activity", activity_labels[names[i], 2], sep = " ")
  rownames(tidy_matrix) <- rownames_vector
  write.table(tidy_matrix, file = "tidy.txt", row.names = FALSE) ## and finally we write the table that we have created into a .txt file 
}
