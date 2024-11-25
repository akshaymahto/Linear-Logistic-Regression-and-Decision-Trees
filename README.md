

# Assignment 3: Hands-on with Linear and Logistic Regression and Decision Trees
8 points
October 18, 2024
Abstract
ATTENTION: this assignment should be completed individually. And I will use
tools to check your codes against your peers’ submissions! This assignment consists of
two problems. For each problem, the instructions are designed to modify the data sequentially. In
other words, each step should be performed on the resulting data frame from the previous step.
# Problem 1: Predicting Income using Logistic Regression and
Decision Trees (5 points)
For this problem, you will be using the Adult dataset from UCI. The goal is to use a logistic regression
and a decision tree model to predict the binary variable income (> 50K or <= 50K) based on the
other attributes in the dataset. Read the attribute information from the website, then click on the
“DOWNLOAD” on the website and obtain the dataset of “adult.data”.
# 1. (0.3 points) Load “adult.data” into a dataframe in R. Note that “adult.data” does not have
column names in the first line, so you need to set header=FALSE when you read the data and
then manually set the column names. Inspect the dataset using str and summary functions.
For each variable in the dataset, is it numeric or categorical? And for each categorical variable
explain whether it is nominal or ordinal.
# 2. (0.1 points) There are some missing values in this dataset represented as “ ?” (Note: there
is a space before ?). Make sure that all “ ?” are converted to NAs. You can do so by setting
“na.strings” parameters in read.csv to “ ?”.
# 3. (0.1 points) Set the random seed, and split the data to train/test. Use 80% of samples for
training and the remaining 20% for testing. You can use sample (see what we did in week 6
decision tree code demo train sample < −sample(1000, 800), but you need to adjust 1000 and
800 according to the size of your dataset). Alternatively, you can use createDataPartition
method from caret package.
# 4. (0.5 points) Read the section on “Handling Missing Data” in chapter 13 of the
textbook “Machine Learning with R”. Find which columns/variables in the train and test
set have missing values. Then decide about how you want to impute the missing values in these
columns. Explain why you chose this imputation approach (Pay attention to data leakage!).
# 5. (0.5 points) The variable “native-country” is sparse, meaning it has too many levels, with some
levels occurring infrequently. Most machine learning algorithms do not work well with sparse
data. One-hot-encoding or dummy coding of these variables will increase feature dimensions
significantly and typically some preprocessing is required to reduce the number of levels. One
approach based on frequency is to group together the levels which occur infrequently. For
instance, one could combine together countries with less than 0.1% occurrence in the data to an
“other” category. Another possibility is to use domain knowledge; for instance, combine
countries based on their geographic location ( “Middle East”, “East-Europe”, “West-Europe”,
etc. Please read the section on Making use of sparse data (remapping sparse cate-
gorical data) in chapter 13 of the textbook “Machine learning with R”. Then combine
1
some of the infrequent levels of the native-country. You can decide whether you want to combine
the levels based on frequency or domain knowledge. Either one is fine for this assignment but
preference will be with a choice that would increase the cross validation performance of the ML
models you will train subsequently. (You do not need to choose the better one, this last sentence
is just for your knowledge on how to determine the better strategy here!)
# 6. (0.5 points) Use appropriate plots and statistic tests to find which variables in the dataset are
associated with “income”. Remove the variable(s) that are not associated with income. You can
check the Bivariate analysis table at the end of this assignment.
# 7. (0.5 points) Train a logistic regression model on the train data (preprocessed and transformed
using above steps) using the glm package and use it to predict “income” for the test data. Note:
As explained in the lectures, “predict” method will return predicted probabilities of being positive
(Note: you can check which level is positive by using levels(your response variable) in R, the
positive class is typically the second level in your response factor. For example, if the output
is Output: [1] “Down” “Up”, then “Up” is positive). To convert them to labels, you need to
use some threshold (typically set as 50%) and if the predicted probability is greater than 50%
you predict the positive class; otherwise predict the negative class (please review the example in
week 8 code demo).
# 8. (0.5 points) Get the cross table between the predicted labels and true labels in the test data
and compute the total error.
# 9. (0.5 points) The target variable “income” is imbalanced; the number of adults who make
<=50K is three times more than the number of adults who make >50K. Most classification mod-
els trained on imbalanced data are biased towards predicting the majority class (income<=50K
in this case) and yield a higher classification error on the minority class (income >50K).
One way to deal with class imbalance problem is to down-sample the majority class; meaning
randomly sample the observations in the majority class to make it the same size as the minority
class. The downside of this approach is that for smaller datasets, removing data will result in
significant loss of information and lower performance. Later, we will learn about other techniques
to deal with data imbalance without removing information, but for this assignment, we use down-
sampling in an attempt to address data imbalance.
Note: Down-sampling should only be done on the training data and the test data
should have the original imbalance distribution. You can downsample as follows:
– Divide your training data into two sets, adults who make <=50K and the ones who make
>50K.
– Suppose that the >50K set has m elements. Take a sample of size m from the <=50K set.
You can use “sample” from the base package to sample the rows. Alternatively, you can
use the method “sample n” from “dplyr” package to directly sample the dataframe.
– Combine the above sample with the >50K set. You can use “rbind” function to combine
the rows in two or more dataframes. This will give you a balanced training data with the
same observations in >50K and <=50K classes.
– Re-train the logistic regression model on the balanced training data and evaluate it on the
test data. Compare the total error. Which model does better in terms of total error?
# 10. (1.5 points) Repeat steps 7-9 above but this time, use a C5.0 decision tree model to predict
“income” instead of the logistic regression model (use trials=30 for boosting multiple decision
trees (see an example in week 6 decision tree code demo). Compare the logistic regression model
with the boosted C5.0 model in terms of total errors.
# Problem 2: Predicting Student Performance (3 points)
For this problem, we are going to use UCI’s student performance dataset. The dataset is a recording
of student grades in math and language and includes attributes related to student demographics and
school related features. Click on the above link, then go to “DOWNLOAD” and download and unzip
2
“student.zip”. You will be using “student-mat.csv” file. The goal is to create a regression model to
forecast student final grade in math “G3” based on the other attributes.
# 1. (0.1 points) Read the dataset into a dataframe. Ensure that you are using a correct delimiter
to read the data correctly and set the “sep” option in read.csv accordingly.
# 2. (0.3 points) Explore the dataset. More specifically, answer the following questions:
– a. Is there any missing values in the dataset?
– b. Draw a histogram of the target variable “G3” and interpret it.
# 3. (0.1 points) Split the data into train and test. Use 80% of samples for training and 20% of
samples for testing. You can use sample (see what we did in week 6 decision tree code demo
train sample < −sample(1000, 800), but you need to adjust 1000 and 800 according to the size
of your dataset). Alternatively, you can use createDataPartition method from caret package.
# 4. (0 point) Set the random seed: set.seed(2024)
# 5. (1 point) Use caret package (“trainControl”) to run 10-fold cross validation using linear
regression method on the train data to predict the “G3” variable. Print the resulting model
to see the cross validation RMSE. In addition, take a summary of the model and interpret the
coefficients. Which coefficients are statistically different from zero? What does this mean? (Hint:
see week 7 code demo, summary(ins model))
# 6. (0 point) Set the random seed again. We need to do this before each training to ensure we
get the same folds in cross validation. Set.seed(2024) so we can compare the models using their
cross validation RMSE.
# 7. (1 point) Use caret and leap packages to run a 10-fold cross validation using step wise linear
regression method with backward selection on the train data. The train method by default uses
maximum of 4 predictors (features) and reports the best models with 1∼4 predictors. We need
to change this parameter to consider all predictors. So inside your train function, add the
following parameter “tuneGrid = data.frame(nvmax = 1:n)”, where n is the number of variables
you use to predict “G3”. Which model (with how many variables or nvmax) has the lowest
cross validation RMSE? Take the summary of the final model, which variables are selected in
the model with the lowest RMSE? (Hint: see “leaps” code section in week 7 code demo)
# 8. (0.5 points) Compare the model you obtained from Q5 and the best model you obtained from
Q7, which model does better at predicting “G3” based on the cross validation RMSE? Get the
predictions of this model for the test data and report RMSE on the test data.
What to turn in
You need to create an R notebook consisting of your answers/analysis to the questions outlined
above for problems one and two together with your R code you used to answer each question.
The submission must be in two formats
• A html file; You run all the code cells, get all the intermediate results and formalize your
answers/analysis, then you click ”preview” and this will create a html file in the same directory as
your notebook. You must submit this html file or your submission will not be graded.
Please note that if you knit your R notebook once, then you will lose the ”preview”
button! So do not knit!
• An Rmd file which contains your R notebook.
3
Numeric Ordinal Nominal
Numeric
Pearson;
Scatter Plots
Kendall or Spearman;
Scatter Plots
If nominal variable has
two groups:
- Two sample t-test
If nominal variable has
more than two groups:
- ANOVA
Side by side boxplots
Ordinal
Kendall or Spearman;
Scatter Plots
Kendall or Spearman;
Scatter Plots
Kruskal-Wallis test;
Side by side boxplots
Nominal
If nominal variable has
two groups:
- Two sample t-test
If nominal variable has
more than two groups:
- ANOVA
Side by side boxplots
Kruskal-Wallis test;
Side by side boxplots
Chi-Square test;
Mosaic Plots
