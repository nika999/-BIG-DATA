Лабораторна робота № 5. Отримання та очистка даних
==================================================

Дані, з якими ви будете працювати, представляють дані, зібрані з
акселерометрів зі смартфона Samsung Galaxy S. Повний опис доступний на
сайті, де були отримані дані:
<a href="http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones" class="uri">http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones</a>

Дані для лабораторної роботи містяться за посиланням:
<a href="https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip" class="uri">https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip</a>

Ви повинні створити один R-скрипт, який називається run\_analysis.R,
який виконує наступні дії.

1.  Об’єднує навчальний та тестовий набори, щоб створити один набір
    даних.

2.  Витягує лише вимірювання середнього значення та стандартного
    відхилення (mean and standard deviation) для кожного вимірювання.

3.  Використовує описові назви діяльностей (activity) для найменування
    діяльностей у наборі даних.

4.  Відповідно присвоює змінним у наборі даних описові імена.

5.  З набору даних з кроку 4 створити другий незалежний акуратний набір
    даних (tidy dataset) із середнім значенням для кожної змінної для
    кожної діяльності та кожного суб’єкту (subject).

Завантажуємо назви діяльностей (activity) та назви стовбців, а також
залишаємо лише середнє значення та стандартне відхилення (mean and
standard deviation)

``` r
if (!require("data.table")) {
  install.packages("data.table")
}
```

    ## Loading required package: data.table

    ## Warning: package 'data.table' was built under R version 4.0.3

``` r
if (!require("reshape2")) {
  install.packages("reshape2")
}
```

    ## Loading required package: reshape2

    ## Warning: package 'reshape2' was built under R version 4.0.3

    ## 
    ## Attaching package: 'reshape2'

    ## The following objects are masked from 'package:data.table':
    ## 
    ##     dcast, melt

``` r
require("data.table")
require("reshape2")

# Load: activity labels
activity_labels <- read.table("./UCI HAR Dataset/activity_labels.txt")[,2]
activity_labels
```

    ## [1] "WALKING"            "WALKING_UPSTAIRS"   "WALKING_DOWNSTAIRS"
    ## [4] "SITTING"            "STANDING"           "LAYING"

``` r
# Load: data column names
features <- read.table("./UCI HAR Dataset/features.txt")[,2]

# Extract only the measurements on the mean and standard deviation for each measurement.
extract_features <- grepl("mean|std", features)
```

Завантажуємо тестові дані

``` r
# Load and process X_test & y_test data.
X_test <- read.table("./UCI HAR Dataset/test/X_test.txt")
y_test <- read.table("./UCI HAR Dataset/test/y_test.txt")
subject_test <- read.table("./UCI HAR Dataset/test/subject_test.txt")

names(X_test) = features

# Extract only the measurements on the mean and standard deviation for each measurement.
X_test = X_test[,extract_features]

# Load activity labels
y_test[,2] = activity_labels[y_test[,1]]
names(y_test) = c("Activity_ID", "Activity_Label")
names(subject_test) = "subject"

# Bind data
test_data <- cbind(as.data.table(subject_test), y_test, X_test)
```

Завантажуємо тренувальні дані

``` r
# Load and process X_train & y_train data.
X_train <- read.table("./UCI HAR Dataset/train/X_train.txt")
y_train <- read.table("./UCI HAR Dataset/train/y_train.txt")

subject_train <- read.table("./UCI HAR Dataset/train/subject_train.txt")

names(X_train) = features

# Extract only the measurements on the mean and standard deviation for each measurement.
X_train = X_train[,extract_features]

# Load activity data
y_train[,2] = activity_labels[y_train[,1]]
names(y_train) = c("Activity_ID", "Activity_Label")
names(subject_train) = "subject"

# Bind data
train_data <- cbind(as.data.table(subject_train), y_train, X_train)
```

Об’єднуємо дані:

``` r
# Merge test and train data
data = rbind(test_data, train_data)

id_labels   = c("subject", "Activity_ID", "Activity_Label")
data_labels = setdiff(colnames(data), id_labels)
melt_data      = melt(data, id = id_labels, measure.vars = data_labels)

# Apply mean function to dataset using dcast function
tidy_data   = dcast(melt_data, subject + Activity_Label ~ variable, mean)

write.table(tidy_data, file = "./tidy_data.txt")
```
