I don't really get how this is different from the script I used to create the data. There is just one and the comments are embedded in the script.

# load in the data tables, create matrices and merge these data
a <- scan("C:/Users/Jochems/Documents/R/UCI HAR Dataset/train/X_train.txt")
b <- as.numeric(a)
X_train <- matrix(b, nrow = 7352, ncol = 561, byrow = T)
a <- scan("C:/Users/Jochems/Documents/R/UCI HAR Dataset/test/X_test.txt")
b <- as.numeric(a)
X_test <- matrix(b, nrow = 2947, ncol = 561, byrow = T)
data <- rbind(X_train, X_test)

# load in the activity numbers and paste together in same order as data
a <- scan("C:/Users/Jochems/Documents/R/UCI HAR Dataset/train/Y_train.txt")
Y_train <- as.numeric(a)
a <- scan("C:/Users/Jochems/Documents/R/UCI HAR Dataset/test/Y_test.txt")
Y_test <- as.numeric(a)
activity <- c(Y_train, Y_test)

# load in the patient ID numbers and paste together in same order as data
a <- scan("C:/Users/Jochems/Documents/R/UCI HAR Dataset/train/subject_train.txt")
id_train <- as.numeric(a)
a <- scan("C:/Users/Jochems/Documents/R/UCI HAR Dataset/test/subject_test.txt")
id_test <- as.numeric(a)
ID <- c(id_train, id_test)

#name of the labels corresponding to the activity numbers
labels <- c("WALKING", "WALKING_UPSTAIRS", "WALKING_DOWNSTAIRS", "SITTING", "STANDING", "LAYING")

#reads the feature file and matches for the mean and std variables and returns the corresponding column#
a <- scan("C:/Users/Jochems/Documents/R/UCI HAR Dataset/features.txt", what = "")
mean <- grep("mean()", a)
std <- grep("std()", a)
keep <- c(mean/2, std/2)

#deletes the non-mean or std values from the data
data_msd <- data[,keep]

#adds the id and activity
tidy_data <- cbind(ID, activity, data_msd)

#orders thed data by patient id
order <- tidy_data[order(tidy_data[,1]),]

#transforms the activity into a factor with labels and assigns this to the data
factorized <- factor(order[,2], labels = labels)
better <- transform(order, activity = factorized)

#assigns the names from the variables to the columns
total <- c(mean, std)
colnames(better) <- c("ID", "Activity", a[total])

#makes a data set with only the means
clean_data <- better[,1:length(mean)]
write.table(clean_data, "C:/Users/Jochems/Documents/R/UCI HAR Dataset/clean_data", sep="\t")

