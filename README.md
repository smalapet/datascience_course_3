# datascience_course_3
Project
# GettingandCleaningData
#Comment

  ##1. Read features and activityLabels vector
  
  features <- read.csv("features.txt", sep = "", header = FALSE)[2]
  activities <- read.csv("activity_labels.txt", sep = "", header = FALSE)
  
  ##2. Read data sets
  
  test_set <- read.csv("test/X_test.txt", sep = "", header = FALSE)
  train_set <- read.csv("train/X_train.txt", sep = "", header = FALSE)
  merged_set <- rbind(test_set,train_set)        
  
  ##3. Read movement
  
  test_moves <- read.csv("test/y_test.txt", sep = "", header = FALSE)
  train_moves <- read.csv("train/y_train.txt", sep = "", header = FALSE)
  merged_moves <- rbind(test_moves, train_moves)
  
  ##4. Read personID
  
  test_person <- read.csv("test/subject_test.txt", sep = "", header = FALSE)
  train_person <- read.csv("train/subject_train.txt", sep = "", header = FALSE)
  merged_person <- rbind(test_person, train_person)
  
  ##5. Extracting columns which includes measurements
  
  names(merged_set) <- features[ ,1]
  merged_set <- merged_set[ grepl("std|mean", names(merged_set), ignore.case = TRUE) ] 
  
  #6. Descripe activityName
  
  merged_moves <- merge(merged_moves, activities, by.x = "V1", by.y = "V1")[2]
  merged_set <- cbind(merged_person, merged_moves, merged_set)
  names(merged_set)[1:2] <- c("PersonID", "Activities")
  
  
  ##7. Tidy merged_set
  
  group_by(merged_set, PersonID, Activities) %>%
    summarise_each(funs(mean))
