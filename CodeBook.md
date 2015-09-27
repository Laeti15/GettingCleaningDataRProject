# GettingCleaningDataRProject
CodeBook.md
Laeti15
Getting & Cleaning Data Project in R

Week 3 Project Assignment
The purpose of this project is to demonstrate your ability to collect, work with, and clean a data set. The goal is to prepare tidy data that can be used for later analysis. You will be graded by your peers on a series of yes/no questions related to the project. You will be required to submit: 1) a tidy data set as described below, 2) a link to a Github repository with your script for performing the analysis, and 3) a code book that describes the variables, the data, and any transformations or work that you performed to clean up the data called CodeBook.md. You should also include a README.md in the repo with your scripts. This repo explains how all of the scripts work and how they are connected.  

One of the most exciting areas in all of data science right now is wearable computing - see for example this article . Companies like Fitbit, Nike, and Jawbone Up are racing to develop the most advanced algorithms to attract new users. The data linked to from the course website represent data collected from the accelerometers from the Samsung Galaxy S smartphone. A full description is available at the site where the data was obtained: 

http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones 

Here are the data for the project: 

https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip 

 You should create one R script called run_analysis.R that does the following. 
Merges the training and the test sets to create one data set.
Extracts only the measurements on the mean and standard deviation for each measurement. 
Uses descriptive activity names to name the activities in the data set
Appropriately labels the data set with descriptive variable names. 
From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.

Data Set Information
The experiments have been carried out with a group of 30 volunteers within an age bracket of 19-48 years. Each person performed six activities (WALKING, WALKING_UPSTAIRS, WALKING_DOWNSTAIRS, SITTING, STANDING, LAYING) wearing a smartphone (Samsung Galaxy S II) on the waist. Using its embedded accelerometer and gyroscope, we captured 3-axial linear acceleration and 3-axial angular velocity at a constant rate of 50Hz. The experiments have been video-recorded to label the data manually. The obtained dataset has been randomly partitioned into two sets, where 70% of the volunteers was selected for generating the training data and 30% the test data. 

The sensor signals (accelerometer and gyroscope) were pre-processed by applying noise filters and then sampled in fixed-width sliding windows of 2.56 sec and 50% overlap (128 readings/window). The sensor acceleration signal, which has gravitational and body motion components, was separated using a Butterworth low-pass filter into body acceleration and gravity. The gravitational force is assumed to have only low frequency components, therefore a filter with 0.3 Hz cutoff frequency was used. From each window, a vector of features was obtained by calculating variables from the time and frequency domain. See 'features_info.txt' for more details. 

For each record it is provided:
======================================

- Triaxial acceleration from the accelerometer (total acceleration) and the estimated body acceleration.
- Triaxial Angular velocity from the gyroscope. 
- A 561-feature vector with time and frequency domain variables. 
- Its activity label. 
- An identifier of the subject who carried out the experiment.

The dataset includes the following files:
=========================================

- 'README.txt'
- 'features_info.txt': Shows information about the variables used on the feature vector.
- 'features.txt': List of all features.
- 'activity_labels.txt': Links the class labels with their activity name.
- 'train/X_train.txt': Training set.
- 'train/y_train.txt': Training labels.
- 'test/X_test.txt': Test set.
- 'test/y_test.txt': Test labels.

The following files are available for the train and test data. Their descriptions are equivalent. 

- 'train/subject_train.txt': Each row identifies the subject who performed the activity for each window sample. Its range is from 1 to 30. 
- 'train/Inertial Signals/total_acc_x_train.txt': The acceleration signal from the smartphone accelerometer X axis in standard gravity units 'g'. Every row shows a 128 element vector. The same description applies for the 'total_acc_x_train.txt' and 'total_acc_z_train.txt' files for the Y and Z axis. 
- 'train/Inertial Signals/body_acc_x_train.txt': The body acceleration signal obtained by subtracting the gravity from the total acceleration. 
- 'train/Inertial Signals/body_gyro_x_train.txt': The angular velocity vector measured by the gyroscope for each window sample. The units are radians/second. 

Notes: 
======
- Features are normalized and bounded within [-1,1].
- Each feature vector is a row on the text file.
- The units used for the accelerations (total and body) are 'g's (gravity of earth -> 9.80665 m/seg2).
- The gyroscope units are rad/seg.
- A video of the experiment including an example of the 6 recorded activities with one of the participants can be seen in the following link: http://www.youtube.com/watch?v=XOEN9W05_4A


Analysis Steps
## Script for Getting and Cleaning Data in R Project
## Check the work directory
getwd()
## install and bring into library all necessary packages for script execution if not already done
install.packages("data.table")
install.packages("dplyr")
library(data.table)
library(dplyr)
## Double check if a project directory already created before creating one to store all necessary files for the analysis
if(!file.exists("projectdir")){dir.create("projectdir")}
## Download, unzip and read all data files
zipurl<-"https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip"
download.file(zipurl, dest="./projectdir/projectdata.zip")
unzip ("./projectdir/projectdata.zip", exdir = "./projectdir")
list.files("./projectdir")
## It appears that the data files and more information about the data under the README.txt file are all under UCI HAR Dataset
list.files("./projectdir/UCI HAR Dataset")
## Get familiar with the content by reading README
list.files("./projectdir/UCI HAR Dataset/test")
list.files("./projectdir/UCI HAR Dataset/train")
## Reading and getting familiar with data for subjects
subject_test_dt<-read.table("./projectdir/UCI HAR Dataset/test/subject_test.txt")
head(subject_test_dt)
str(subject_test_dt)
table(subject_test_dt)
subject_train_dt<-read.table("./projectdir/UCI HAR Dataset/train/subject_train.txt")
head(subject_train_dt)
str(subject_train_dt)
table(subject_train_dt)
## Reading and getting familiar with data sets (x) and the corresponding labels (integers and activity name) to reappend for both test and training groups
X_test_dt<-read.table("./projectdir/UCI HAR Dataset/test/X_test.txt")
head(X_test_dt)
str(X_test_dt)
X_train_dt<-read.table("./projectdir/UCI HAR Dataset/train/X_train.txt")
head(X_train_dt)
str(X_train_dt)
Y_test_dt<-read.table("./projectdir/UCI HAR Dataset/test/Y_test.txt")
head(Y_test_dt)
str(Y_test_dt)
table(Y_test_dt)
Y_train_dt<-read.table("./projectdir/UCI HAR Dataset/train/Y_train.txt")
head(Y_train_dt)
str(Y_train_dt)
table(Y_train_dt)
activity_labels_dt<-read.table("./projectdir/UCI HAR Dataset/activity_labels.txt")
head(activity_labels_dt)

## Part 1. Merge the training and the test sets to create one data set.
### Merge test subject, label integers and feature measures
test_dt <- cbind(as.data.table(subject_test_dt),Y_test_dt, X_test_dt)
### Merge training subject, label integers and feature measures
training_dt <- cbind(as.data.table(subject_train_dt),Y_train_dt, X_train_dt)
### Merge test and training data frames together using row bind
testntraining_dt<-rbind(test_dt,training_dt)
## Part 2. Extract only the measurements on the mean and standard deviation for each measurement.
### Modify header vector to now include "subject", "activity" and the feature names.
### then append the new vector of header names onto the merged data set
### Create a subset of data by only keeping subject, activity columns along with mean and std columns using the data manipulation verb 'select' from the dplyr package since it is simpler to do than using straight R coding
features_dt<-read.table("./projectdir/UCI HAR Dataset/features.txt")
features_dt ## exclude the column with the observation number
features_dt[,2]
featurenames<-as.character(features_dt[,2])
header<-c("subject","activity",featurenames)
colnames(testntraining_dt)<-c("subject","activity",featurenames)
df<-data.frame(testntraining_dt)
xtr<- select(df,contains("subject"), contains("activity"), contains(".mean."), contains(".std."))
str(xtr) 
## Part 3. Append descriptive activity names provided in the activity_labels file to name the activities ( in integer form) in the data set
xtr2<-merge(activity_labels_dt,xtr,by.x="V1",by.y="activity",all=TRUE)

## Part 4. Appropriately label the data set with descriptive variable name such as displaying activity_id, activity_name, subject_id, and spelling out t as time and f as frequency in the feature variables.
names(xtr2)<-gsub("V1", "activity_id", names(xtr2))
names(xtr2)<-gsub("V2", "activity_name", names(xtr2))
names(xtr2)<-gsub("subject", "subject_id", names(xtr2))
names(xtr2)<-gsub("^t", "time", names(xtr2))
names(xtr2)<-gsub("^f", "frequency", names(xtr2))
### Double check the renaming steps worked
names(xtr2)


## Part 5. From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.
### Install the reshape 2 package that will allow user to reshape the data set my melting it and casting it into the right groupings for the calculation of the mean
install.packages("reshape2")
library(reshape2)
xtr2Melt<-melt(xtr2,id=c("subject_id","activity_name"),measure.vars=colnames(xtr2[,4:69]))
head(xtr2Melt)
tidydataq5<-dcast(xtr2Melt,subject_id+activity_name~variable,mean)
write.table(tidydataq5,file="tidy_dataset.txt")

Viewing first 12 rows of output tidy data set showing average values for extracted features by subject (here subject with id 1 and 2) and by activity name.


> head(tidydataq5,12)
   subject_id      activity_name timeBodyAcc.mean...X timeBodyAcc.mean...Y
1           1             LAYING            0.2215982         -0.040513953
2           1            SITTING            0.2612376         -0.001308288
3           1           STANDING            0.2789176         -0.016137590
4           1            WALKING            0.2773308         -0.017383819
5           1 WALKING_DOWNSTAIRS            0.2891883         -0.009918505
6           1   WALKING_UPSTAIRS            0.2554617         -0.023953149
7           2             LAYING            0.2813734         -0.018158740
8           2            SITTING            0.2770874         -0.015687994
9           2           STANDING            0.2779115         -0.018420827
10          2            WALKING            0.2764266         -0.018594920
11          2 WALKING_DOWNSTAIRS            0.2776153         -0.022661416
12          2   WALKING_UPSTAIRS            0.2471648         -0.021412113
   timeBodyAcc.mean...Z timeGravityAcc.mean...X timeGravityAcc.mean...Y
1            -0.1132036              -0.2488818               0.7055498
2            -0.1045442               0.8315099               0.2044116
3            -0.1106018               0.9429520              -0.2729838
4            -0.1111481               0.9352232              -0.2821650
5            -0.1075662               0.9318744              -0.2666103
6            -0.0973020               0.8933511              -0.3621534
7            -0.1072456              -0.5097542               0.7525366
8            -0.1092183               0.9404773              -0.1056300
9            -0.1059085               0.8969286              -0.3700627
10           -0.1055004               0.9130173              -0.3466071
11           -0.1168129               0.8618313              -0.3257801
12           -0.1525139               0.7907174              -0.4162149
   timeGravityAcc.mean...Z timeBodyAccJerk.mean...X timeBodyAccJerk.mean...Y
1               0.44581772               0.08108653             0.0038382040
2               0.33204370               0.07748252            -0.0006191028
3               0.01349058               0.07537665             0.0079757309
4              -0.06810286               0.07404163             0.0282721096
5              -0.06211996               0.05415532             0.0296504490
6              -0.07540294               0.10137273             0.0194863076
7               0.64683488               0.08259725             0.0122547885
8               0.19872677               0.07225644             0.0116954511
9               0.12974716               0.07475886             0.0103291775
10              0.08472709               0.06180807             0.0182492679
11             -0.04388902               0.11004062            -0.0032795908
12             -0.19588824               0.07445078            -0.0097098551
   timeBodyAccJerk.mean...Z timeBodyGyro.mean...X timeBodyGyro.mean...Y
1               0.010834236           -0.01655309          -0.064486124
2              -0.003367792           -0.04535006          -0.091924155
3              -0.003685250           -0.02398773          -0.059397221
4              -0.004168406           -0.04183096          -0.069530046
5              -0.010971973           -0.03507819          -0.090937129
6              -0.045562545            0.05054938          -0.166170015
7              -0.001802649           -0.01847661          -0.111800825
8               0.007605469           -0.04547066          -0.059928680
9              -0.008371588           -0.02386239          -0.082039658
10              0.007895337           -0.05302582          -0.048238232
11             -0.020935168           -0.11594735          -0.004823292
12              0.019481439           -0.05769126          -0.032088310
   timeBodyGyro.mean...Z timeBodyGyroJerk.mean...X timeBodyGyroJerk.mean...Y
1             0.14868944               -0.10727095               -0.04151729
2             0.06293138               -0.09367938               -0.04021181
3             0.07480075               -0.09960921               -0.04406279
4             0.08494482               -0.08999754               -0.03984287
5             0.09008501               -0.07395920               -0.04399028
6             0.05835955               -0.12223277               -0.04214859
7             0.14488285               -0.10197413               -0.03585902
8             0.04122775               -0.09363284               -0.04156020
9             0.08783517               -0.10556216               -0.04224195
10            0.08283366               -0.08188334               -0.05382994
11            0.09717381               -0.05810385               -0.04214703
12            0.06883740               -0.08288580               -0.04240537
   timeBodyGyroJerk.mean...Z timeBodyAccMag.mean.. timeGravityAccMag.mean..
1                -0.07405012           -0.84192915              -0.84192915
2                -0.04670263           -0.94853679              -0.94853679
3                -0.04895055           -0.98427821              -0.98427821
4                -0.04613093           -0.13697118              -0.13697118
5                -0.02704611            0.02718829               0.02718829
6                -0.04071255           -0.12992763              -0.12992763
7                -0.07017830           -0.97743549              -0.97743549
8                -0.04358510           -0.96789362              -0.96789362
9                -0.05465395           -0.96587518              -0.96587518
10               -0.05149392           -0.29040759              -0.29040759
11               -0.07102298            0.08995112               0.08995112
12               -0.04451575           -0.10732268              -0.10732268
   timeBodyAccJerkMag.mean.. timeBodyGyroMag.mean.. timeBodyGyroJerkMag.mean..
1               -0.954396265            -0.87475955                 -0.9634610
2               -0.987364196            -0.93089249                 -0.9919763
3               -0.992367791            -0.97649379                 -0.9949668
4               -0.141428809            -0.16097955                 -0.2987037
5               -0.089447481            -0.07574125                 -0.2954638
6               -0.466503446            -0.12673559                 -0.5948829
7               -0.987741696            -0.95001157                 -0.9917671
8               -0.986774713            -0.94603509                 -0.9910815
9               -0.980489077            -0.96346634                 -0.9839519
10              -0.281424154            -0.44654909                 -0.5479120
11               0.005655163            -0.16218859                 -0.4108727
12              -0.321268911            -0.21971347                 -0.5728164
   frequencyBodyAcc.mean...X frequencyBodyAcc.mean...Y frequencyBodyAcc.mean...Z
1                -0.93909905              -0.867065205                -0.8826669
2                -0.97964124              -0.944084550                -0.9591849
3                -0.99524993              -0.977070848                -0.9852971
4                -0.20279431               0.089712726                -0.3315601
5                 0.03822918               0.001549908                -0.2255745
6                -0.40432178              -0.190976721                -0.4333497
7                -0.97672506              -0.979800878                -0.9843810
8                -0.98580384              -0.957343498                -0.9701622
9                -0.98394674              -0.959871697                -0.9624712
10               -0.34604816              -0.021904810                -0.4538064
11                0.11284116               0.278345042                -0.1312908
12               -0.26672093               0.009924459                -0.2810020
   frequencyBodyAccJerk.mean...X frequencyBodyAccJerk.mean...Y
1                    -0.95707388                   -0.92246261
2                    -0.98659702                   -0.98157947
3                    -0.99463080                   -0.98541870
4                    -0.17054696                   -0.03522552
5                    -0.02766387                   -0.12866716
6                    -0.47987525                   -0.41344459
7                    -0.98581363                   -0.98276825
8                    -0.98784879                   -0.97713970
9                    -0.98097324                   -0.97085134
10                   -0.30461532                   -0.07876408
11                    0.13812068                    0.09620916
12                   -0.25863944                   -0.18784213
   frequencyBodyAccJerk.mean...Z frequencyBodyGyro.mean...X frequencyBodyGyro.mean...Y
1                     -0.9480609                 -0.8502492                -0.95219149
2                     -0.9860531                 -0.9761615                -0.97583859
3                     -0.9907522                 -0.9863868                -0.98898446
4                     -0.4689992                 -0.3390322                -0.10305942
5                     -0.2883347                 -0.3524496                -0.05570225
6                     -0.6854744                 -0.4926117                -0.31947461
7                     -0.9861971                 -0.9864311                -0.98332164
8                     -0.9851291                 -0.9826214                -0.98210092
9                     -0.9797752                 -0.9670371                -0.97257615
10                    -0.5549567                 -0.4297135                -0.55477211
11                    -0.2714987                 -0.1457760                -0.36191382
12                    -0.5227281                 -0.3316436                -0.48808612
   frequencyBodyGyro.mean...Z frequencyBodyAccMag.mean..
1                 -0.90930272                -0.86176765
2                 -0.95131554                -0.94778292
3                 -0.98077312                -0.98535636
4                 -0.25594094                -0.12862345
5                 -0.03186943                 0.09658453
6                 -0.45359721                -0.35239594
7                 -0.96267189                -0.97511020
8                 -0.95981482                -0.96127375
9                 -0.96062770                -0.96405217
10                -0.39665991                -0.32428943
11                -0.08749447                 0.29342483
12                -0.24860112                -0.14531854
   frequencyBodyBodyAccJerkMag.mean.. frequencyBodyBodyGyroMag.mean..
1                         -0.93330036                      -0.8621902
2                         -0.98526213                      -0.9584356
3                         -0.99254248                      -0.9846176
4                         -0.05711940                      -0.1992526
5                          0.02621849                      -0.1857203
6                         -0.44265216                      -0.3259615
7                         -0.98537411                      -0.9721130
8                         -0.98387470                      -0.9718406
9                         -0.97706530                      -0.9617759
10                        -0.16906435                      -0.5307048
11                         0.22224741                      -0.3208385
12                        -0.18951114                      -0.4506122
   frequencyBodyBodyGyroJerkMag.mean.. timeBodyAcc.std...X timeBodyAcc.std...Y
1                           -0.9423669         -0.92805647        -0.836827406
2                           -0.9897975         -0.97722901        -0.922618642
3                           -0.9948154         -0.99575990        -0.973190056
4                           -0.3193086         -0.28374026         0.114461337
5                           -0.2819634          0.03003534        -0.031935943
6                           -0.6346651         -0.35470803        -0.002320265
7                           -0.9902487         -0.97405946        -0.980277399
8                           -0.9898620         -0.98682228        -0.950704499
9                           -0.9778498         -0.98727189        -0.957304989
10                          -0.5832493         -0.42364284        -0.078091253
11                          -0.3801753          0.04636668         0.262881789
12                          -0.6007985         -0.30437641         0.108027280
   timeBodyAcc.std...Z timeGravityAcc.std...X timeGravityAcc.std...Y
1          -0.82606140             -0.8968300             -0.9077200
2          -0.93958629             -0.9684571             -0.9355171
3          -0.97977588             -0.9937630             -0.9812260
4          -0.26002790             -0.9766096             -0.9713060
5          -0.23043421             -0.9505598             -0.9370187
6          -0.01947924             -0.9563670             -0.9528492
7          -0.98423330             -0.9590144             -0.9882119
8          -0.95982817             -0.9799888             -0.9567503
9          -0.94974185             -0.9866858             -0.9741944
10         -0.42525752             -0.9726932             -0.9721169
11         -0.10283791             -0.9403618             -0.9400685
12         -0.11212102             -0.9344077             -0.9237675
   timeGravityAcc.std...Z timeBodyAccJerk.std...X timeBodyAccJerk.std...Y
1              -0.8523663             -0.95848211             -0.92414927
2              -0.9490409             -0.98643071             -0.98137197
3              -0.9763241             -0.99460454             -0.98564873
4              -0.9477172             -0.11361560              0.06700250
5              -0.8959397             -0.01228386             -0.10160139
6              -0.9123794             -0.44684389             -0.37827443
7              -0.9842304             -0.98587217             -0.98317254
8              -0.9544159             -0.98805585             -0.97798396
9              -0.9459271             -0.98108572             -0.97105944
10             -0.9720728             -0.27753046             -0.01660224
11             -0.9314383              0.14724914              0.12682801
12             -0.8780041             -0.27612189             -0.18564895
   timeBodyAccJerk.std...Z timeBodyGyro.std...X timeBodyGyro.std...Y timeBodyGyro.std...Z
1               -0.9548551           -0.8735439         -0.951090440           -0.9082847
2               -0.9879108           -0.9772113         -0.966473895           -0.9414259
3               -0.9922512           -0.9871919         -0.987734440           -0.9806456
4               -0.5026998           -0.4735355         -0.054607769           -0.3442666
5               -0.3457350           -0.4580305         -0.126349195           -0.1247025
6               -0.7065935           -0.5448711          0.004105184           -0.5071687
7               -0.9884420           -0.9882752         -0.982291609           -0.9603066
8               -0.9875182           -0.9857420         -0.978919527           -0.9598037
9               -0.9828414           -0.9729986         -0.971441996           -0.9648567
10              -0.5860904           -0.5615503         -0.538453668           -0.4810855
11              -0.3401220           -0.3207892         -0.415739145           -0.2794184
12              -0.5737464           -0.4392531         -0.466298337           -0.1639958
   timeBodyGyroJerk.std...X timeBodyGyroJerk.std...Y timeBodyGyroJerk.std...Z
1                -0.9186085               -0.9679072               -0.9577902
2                -0.9917316               -0.9895181               -0.9879358
3                -0.9929451               -0.9951379               -0.9921085
4                -0.2074219               -0.3044685               -0.4042555
5                -0.4870273               -0.2388248               -0.2687615
6                -0.6147865               -0.6016967               -0.6063320
7                -0.9932358               -0.9895675               -0.9880358
8                -0.9897090               -0.9908896               -0.9855423
9                -0.9793240               -0.9834473               -0.9736101
10               -0.3895498               -0.6341404               -0.4354927
11               -0.2439406               -0.4693967               -0.2182663
12               -0.4648544               -0.6454913               -0.4675960
   timeBodyAccMag.std.. timeGravityAccMag.std.. timeBodyAccJerkMag.std..
1           -0.79514486             -0.79514486              -0.92824563
2           -0.92707842             -0.92707842              -0.98412002
3           -0.98194293             -0.98194293              -0.99309621
4           -0.21968865             -0.21968865              -0.07447175
5            0.01988435              0.01988435              -0.02578772
6           -0.32497093             -0.32497093              -0.47899162
7           -0.97287391             -0.97287391              -0.98551808
8           -0.95308144             -0.95308144              -0.98447587
9           -0.95787497             -0.95787497              -0.97667528
10          -0.42254417             -0.42254417              -0.16415099
11           0.21558633              0.21558633               0.22961719
12          -0.20597705             -0.20597705              -0.21738939
   timeBodyGyroMag.std.. timeBodyGyroJerkMag.std.. frequencyBodyAcc.std...X
1             -0.8190102                -0.9358410              -0.92443743
2             -0.9345318                -0.9883087              -0.97641231
3             -0.9786900                -0.9947332              -0.99602835
4             -0.1869784                -0.3253249              -0.31913472
5             -0.2257244                -0.3065106               0.02433084
6             -0.1486193                -0.6485530              -0.33742819
7             -0.9611641                -0.9897181              -0.97324648
8             -0.9613136                -0.9895949              -0.98736209
9             -0.9539434                -0.9772044              -0.98905647
10            -0.5530199                -0.5577982              -0.45765138
11            -0.2748441                -0.3431879               0.01610462
12            -0.3775322                -0.5972917              -0.32058241
   frequencyBodyAcc.std...Y frequencyBodyAcc.std...Z frequencyBodyAccJerk.std...X
1               -0.83362556              -0.81289156                  -0.96416071
2               -0.91727501              -0.93446956                  -0.98749299
3               -0.97229310              -0.97793726                  -0.99507376
4                0.05604001              -0.27968675                  -0.13358661
5               -0.11296374              -0.29792789                  -0.08632790
6                0.02176951               0.08595655                  -0.46190703
7               -0.98102511              -0.98479218                  -0.98725026
8               -0.95007375              -0.95686286                  -0.98945911
9               -0.95790884              -0.94643358                  -0.98300792
10              -0.16921969              -0.45522215                  -0.31431306
11               0.17197397              -0.16203289                   0.04995906
12               0.08488028              -0.09454498                  -0.36541544
   frequencyBodyAccJerk.std...Y frequencyBodyAccJerk.std...Z frequencyBodyGyro.std...X
1                   -0.93221787                   -0.9605870                -0.8822965
2                   -0.98251391                   -0.9883392                -0.9779042
3                   -0.98701823                   -0.9923498                -0.9874971
4                    0.10673986                   -0.5347134                -0.5166919
5                   -0.13458001                   -0.4017215                -0.4954225
6                   -0.38177707                   -0.7260402                -0.5658925
7                   -0.98498739                   -0.9893454                -0.9888607
8                   -0.98080423                   -0.9885708                -0.9868085
9                   -0.97352024                   -0.9845999                -0.9749881
10                  -0.01533295                   -0.6158982                -0.6040530
11                   0.08083335                   -0.4082274                -0.3794367
12                  -0.24355415                   -0.6250910                -0.4763588
   frequencyBodyGyro.std...Y frequencyBodyGyro.std...Z frequencyBodyAccMag.std..
1                -0.95123205                -0.9165825               -0.79830094
2                -0.96234504                -0.9439178               -0.92844480
3                -0.98710773                -0.9823453               -0.98231380
4                -0.03350816                -0.4365622               -0.39803259
5                -0.18141473                -0.2384436               -0.18653030
6                 0.15153891                -0.5717078               -0.41626010
7                -0.98191062                -0.9631742               -0.97512139
8                -0.97735619                -0.9635227               -0.95557560
9                -0.97103605                -0.9697543               -0.96051938
10               -0.53304695                -0.5598566               -0.57710521
11               -0.45873275                -0.4229877               -0.02147879
12               -0.45975849                -0.2180725               -0.36672824
   frequencyBodyBodyAccJerkMag.std.. frequencyBodyBodyGyroMag.std..
1                         -0.9218040                     -0.8243194
2                         -0.9816062                     -0.9321984
3                         -0.9925360                     -0.9784661
4                         -0.1034924                     -0.3210180
5                         -0.1040523                     -0.3983504
6                         -0.5330599                     -0.1829855
7                         -0.9845685                     -0.9610984
8                         -0.9841242                     -0.9613857
9                         -0.9751605                     -0.9567887
10                        -0.1640920                     -0.6517928
11                         0.2274807                     -0.3725768
12                        -0.2604238                     -0.4386204
   frequencyBodyBodyGyroJerkMag.std..
1                          -0.9326607
2                          -0.9870496
3                          -0.9946711
4                          -0.3816019
5                          -0.3919199
6                          -0.6939305
7                          -0.9894927
8                          -0.9896329
9                          -0.9777543
10                         -0.5581046
11                         -0.3436990
12                         -0.6218202
