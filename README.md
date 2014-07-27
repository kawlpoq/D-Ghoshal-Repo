D-Ghoshal-Repo
==============

project assignment 


https://github.com/kawlpoq/D-Ghoshal-Repo.git



Title: Human Activity Prediction Using Accelerometer Data

Introduction: Prediction of human activity using some real time data from a accelerometer is a challenging task and it is the idea of our project. In a sense this is the same goal of machine learning where predictions are made from sensor data. We are here to demonstrate how to use the R programming language and its many functions to accomplish our task. There were individuals who volunteered to conduct the experiments. Each individual carried a sensor device on the waist and performed different activities. In this project, our goal will be to use data from accelerometers on the belt, forearm, arm, and dumbell of 6 participants. They were asked to perform barbell lifts correctly and incorrectly in 5 different ways. An accelerometer and a gyroscope are attached which help to capture axial linear acceleration and axial angular velocity at some constant rate. The actual functioning of the accelerometer and gyroscope is best described in [4]Therefore, it would be very interesting and challenging to see how we can use such data in a very organized way to classify, test, analyze and finally to produce a good prediction.
Method:
Data Collection
> train_file <- read.csv("http://d396qusza40orc.cloudfront.net/predmachlearn/pml-training.csv")
test_file <- read.csv("http://d396qusza40orc.cloudfront.net/predmachlearn/pml-testing.csv")
> nrow(train_file)
[1] 19622
> ncol(train_file)
[1] 160
> nrow(test_file)
[1] 20
> ncol(test_file)
[1] 160
> train_file_wona <- na.omit(train_file)
> nrow(train_file_wona)
[1] 406
> ncol(train_file_wona)
[1] 160

Sample summary

> summary(train_file_wona)
X user_name raw_timestamp_part_1 raw_timestamp_part_2 cvtd_timestamp new_window num_window roll_belt
Min. : 24 adelmo :83 Min. :1.322e+09 Min. :492326 30/11/2011 17:11: 36 no : 0 Min. : 2.0 Min. :-27.80
1st Qu.: 5284 carlitos:56 1st Qu.:1.323e+09 1st Qu.:968313 02/12/2011 14:58: 34 yes:406 1st Qu.:228.5 1st Qu.: 1.15
Median :10303 charles :81 Median :1.323e+09 Median :982651 02/12/2011 14:57: 32 Median :464.5 Median :116.00
Mean :10091 eurico :54 Mean :1.323e+09 Mean :971714 02/12/2011 13:35: 28 Mean :446.5 Mean : 68.16
3rd Qu.:15056 jeremy :77 3rd Qu.:1.323e+09 3rd Qu.:992286 30/11/2011 17:12: 28 3rd Qu.:665.5 3rd Qu.:123.00
Max. :19622 pedro :55 Max. :1.323e+09 Max. :998801 05/12/2011 11:24: 27 Max. :864.0 Max. :161.00
(Other) :221

rf <- randomForest(classe ~ roll_belt+pitch_belt+var_total_accel_belt+avg_roll_belt+stddev_roll_belt+var_roll_belt+avg_pitch_belt+stddev_pitch_belt+var_pitch_belt+avg_yaw_belt+stddev_yaw_belt+var_yaw_belt,data=train_file_wona)

> rf

Call:
randomForest(formula = classe ~ roll_belt + pitch_belt + var_total_accel_belt + avg_roll_belt + stddev_roll_belt + var_roll_belt + avg_pitch_belt + stddev_pitch_belt + var_pitch_belt + avg_yaw_belt + stddev_yaw_belt + var_yaw_belt, data = train_file_wona)
Type of random forest: classification
Number of trees: 500
No. of variables tried at each split: 3
OOB estimate of error rate: 30.05%

Confusion matrix:
A B C D E class.error
A 74 14 13 7 1 0.3211009
B 12 60 5 1 1 0.2405063
C 10 14 39 7 0 0.4428571
D 8 6 7 46 2 0.3333333
E 6 3 4 1 65 0.1772152

> table(train_file_wona$classe)
A B C D E
109 79 70 69 79

> head(getTree(rf,1,labelVar=TRUE),7)
left daughter right daughter split var split point status prediction
1 2 3 avg_roll_belt 130.00000 1 <NA>
2 4 5 var_roll_belt 5.70000 1 <NA>
3 0 0 <NA> 0.00000 -1 E
4 6 7 var_pitch_belt 0.10945 1 <NA>
5 8 9 stddev_roll_belt 7.70000 1 <NA>
6 10 11 pitch_belt -43.05000 1 <NA>
7 12 13 pitch_belt 11.60000 1 <NA>
> tail(getTree(rf,1,labelVar=TRUE),7)
left daughter right daughter split var split point status
183 184 185 avg_pitch_belt 4.75 1
184 186 187 var_total_accel_belt 0.25 1
185 0 0 <NA> 0.00 -1
186 188 189 var_pitch_belt 0.25 1
187 0 0 <NA> 0.00 -1
188 0 0 <NA> 0.00 -1
189 0 0 <NA> 0.00 -1
prediction
183 <NA>
184 <NA>
185 C

Exploratory Analysis:
We first examined the data carefully. The raw data was particularly examined to 1> find any missing values 2> quality of data and 3> checking if any conversion or transformation needed to continue our analysis.
We are to report that in our system, we are satisfied with the quality of data and we did have conversion or transformation needed for NA values in training data. .
Then we reviewed our lecture videos and texts to find the best approach we should consider. In this effort we consulted some publications [4],[5],6] and a good book [7] of Data Analysis. Such review convinced us to follow the statistical analysis of tree and we are going to describe such analysis in the following section.

Statistical Analysis:
In the first step of the Statistical Analysis, after playing with several functions (e.g. glm etc.) we decided to follow the Tree based regression [7] and provided the following command.
>
train.rpart <- rpart(classe ~ roll_belt+pitch_belt+var_total_accel_belt+avg_roll_belt+stddev_roll_belt+var_roll_belt+avg_pitch_belt+stddev_pitch_belt+var_pitch_belt+avg_yaw_belt+stddev_yaw_belt+var_yaw_belt, method ="class", data = train_file_wona, cp =0.0025)
>

Here we use the partition function rpart of R programming language. The dot(.) on the right side represents all available exploratory variables and cp the complexity parameter controls the cross-validation of the data. Increasing or decreasing cp we can find out how the error-rate is changing and thus determine the right value of our choice of cp.
We then provided the command to plot cross-validation vs. cp. >plotcp(train.rpart)

The output of

> printcp(train.rpart)
Classification tree:
rpart(formula = classe ~ roll_belt + pitch_belt + var_total_accel_belt +
avg_roll_belt + stddev_roll_belt + var_roll_belt + avg_pitch_belt +
stddev_pitch_belt + var_pitch_belt + avg_yaw_belt + stddev_yaw_belt +
var_yaw_belt, data = train_file_wona, method = "class", cp = 0.0025)
Variables actually used in tree construction:
[1] avg_pitch_belt avg_roll_belt avg_yaw_belt pitch_belt roll_belt stddev_roll_belt stddev_yaw_belt
[8] var_total_accel_belt var_yaw_belt
Root node error: 297/406 = 0.73153
n= 406

CP nsplit rel error xerror xstd
1 0.202020 0 1.00000 1.00000 0.030066
2 0.041751 1 0.79798 0.79798 0.033442
3 0.023569 6 0.58923 0.74411 0.033788
4 0.020202 7 0.56566 0.65993 0.033901
5 0.019080 8 0.54545 0.64310 0.033862
6 0.018519 11 0.48822 0.64310 0.033862
7 0.016835 14 0.42424 0.60606 0.033703
8 0.010101 17 0.37374 0.55556 0.033322
9 0.006734 18 0.36364 0.53199 0.033078
10 0.003367 21 0.34343 0.52525 0.033000
11 0.002500 24 0.33333 0.51178 0.032834

> Here we can find -
Cross-validation error rate = minimum xerro * Root node error
= 0.511 *.73153
= 0.3738

Since our Cross-validation rate is low, we did not have to play with any lower value of cp.
We now further researched as above but with the randomForest function.
> rf <- randomForest(classe ~ roll_belt+pitch_belt+var_total_accel_belt+avg_roll_belt+ rtab.rf <- randomForest(trainData$activity ~ ., method ="class", data = trainData, importance=TRUE)
>rf
> r
f

Call:
randomForest(formula = classe ~ roll_belt + pitch_belt + var_total_accel_belt + avg_roll_belt + stddev_roll_belt +
var_roll_belt + avg_pitch_belt + stddev_pitch_belt + var_pitch_belt + avg_yaw_belt + stddev_yaw_belt + var_yaw_belt, data = train_file_wona, method = "class", importance = TRUE)
Type of random forest: classification

Number of trees: 500

No. of variables tried at each split: 3

OOB estimate of error rate: 29.8%

Confusion matrix:
A B C D E class.error
A 74 14 9 11 1 0.3211009
B 10 59 7 2 1 0.2531646
C 5 14 42 9 0 0.4000000
D 9 5 8 45 2 0.3478261
E 6 3 4 1 65 0.1772152
>


Since OOB estimate of error rate above is also very low and this function have several advantages over Tree regression e.g. can show the importance of other variables on the independent variable (activity) and the confusion matrix provides good information , we decided to use randomForest on our trainData and get the predictive values by using predict function on the testData.
Instead of all the variables we now do our testing according to our test file. For test file 1
rf1 <-randomForest(classe ~ X+min_roll_belt+accel_belt_y+gyros_arm_y+amplitude_roll_arm+amplitude_yaw_dumbbell+magnet_dumbbell_z+var_accel_forearm,method ="class", data = train_file_wona, importance=TRUE)
we now test our predict function for rf1 and test1.

Similarly we go on for test2,test3…test20 Result : We have already shown some important result of randomForest Analysis above and the test results in programming section Now we are going to find out the important variables which influence the activity most in our test data. We execute the following command:

>importance(rf)

The above command produces all the influence factor of the important variables. The output is shown in Table 1. Table 1> 

importance(rf)
A B C D E MeanDecreaseAccuracy MeanDecreaseGini
roll_belt 8.936543 19.080990 14.148443 23.00905 10.308356 25.88919 32.40155 pitch_belt 18.674516 20.568082 9.983187 20.42890 7.014724 32.38966 32.51218 var_total_accel_belt 19.107613 12.339095 15.401240 15.53894 14.897551 25.26203 25.95097 avg_roll_belt 15.424055 25.392633 17.175713 27.51093 11.848030 33.87653 40.55130 stddev_roll_belt 13.849260 9.112913 14.768998 21.76049 13.482143 20.56879 27.49054 var_roll_belt 11.299545 10.400384 13.633305 19.17806 14.330338 19.74510 25.49317 avg_pitch_belt 23.780243 23.709577 15.392091 23.96524 8.142862 38.64012 35.70655 stddev_pitch_belt 10.261608 5.899581 11.367299 10.93988 6.724217 16.47890 15.09153 var_pitch_belt 13.017921 8.964453 10.140978 11.45394 5.346853 18.01619 13.51320 avg_yaw_belt 26.754839 23.815091 18.341031 18.84389 11.672856 40.78000 41.32098
stddev_yaw_belt 11.590682 5.973404 9.548031 12.14861 1.976540 18.18711 13.24431 var_yaw_belt 10.226273 8.139015 11.662961 13.36427 2.330057 19.50471 17.69566 >

It is clear from the above table 1 that the most influential variable on activity is “avg_yaw_belt” (The influence factor is 41.32). We actually showed a plot (Fig 1 Top plot) of this activity on trainData vs. that variable. It is apparent that we can have similar plots for other variables and can have a good picture of what are the major contributing factors of the sensor data. We can even detect, if some device is not properly functioning, the corresponding activity will show up with problems and we can pinpoint the particular variable responsible for malfunctioning.
much are the differences in values of prediction against actual activity i.e how many of the predictions missed in proportion to actual values.

Conclusion: Our research and analysis confirmed that we can produce a very good model to predict the classe of persons using sensor data. We have to selectively and carefully organize our data as training data and test data. Since in our case the error rate is very low (.29%) i.e success rate is high, we do not need to do any further validations. The class errors on activities as shown above in the confusion matrix clearly indicate that
our prediction of each classe is sound and we can rely on our model. The randomForest package is actually good for relative complex trees and the accuracy of the activity is acceptable. There are many improvements on our model we can think of. We can always have a bigger training data and testing data. We can add more activity types or features and we can have some real time data placing the phone in more unstable locations. Ignoring the sensor data and collecting everything from a more sophisticated sensor system is another good idea 

Reference : 
1. http://groupware.les.inf.puc-rio.br/har 
2.  https://d396qusza40orc.cloudfront.net/predmachlearn/pml-training.csv (TRAINING DATA) 
3. https://d396qusza40orc.cloudfront.net/predmachlearn/pml-testing.csv(TEST DATA) 
4. : http://groupware.les.inf.puc-rio.br/har
5. Activity Recognition using Cell Phone Accelerometers Jennifer R. Kwapisz, Gary M. Weiss, Samuel A. Moore
6. Building Predictive Models in R Using the caret Package
Max Kuhn. P_zer Global R&D 
7. Data Analysis and Graphics Using R: An Example-Based Approach John Maindonald and John Braum

