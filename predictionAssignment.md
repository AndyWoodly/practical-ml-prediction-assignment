# Coursera: Practical Machine Learning - Prediction Assignment (https://www.coursera.org/learn/practical-machine-learning)
Andreas Hoelzl [GitHub](https://github.com/AndyWoodly/practical-ml-prediction-assignment)  

**Background**

Using devices such as Jawbone Up, Nike FuelBand, and Fitbit it is now possible to collect a large amount of data about personal activity relatively inexpensively. These type of devices are part of the quantified self movement - a group of enthusiasts who take measurements about themselves regularly to improve their health, to find patterns in their behavior, or because they are tech geeks. One thing that people regularly do is quantify how much of a particular activity they do, but they rarely quantify how well they do it. In this project, your goal will be to use data from accelerometers on the belt, forearm, arm, and dumbell of 6 participants. They were asked to perform barbell lifts correctly and incorrectly in 5 different ways. More information is available from the website here: http://groupware.les.inf.puc-rio.br/har (see the section on the Weight Lifting Exercise Dataset). 


**Data**

Training data:

https://d396qusza40orc.cloudfront.net/predmachlearn/pml-training.csv

Test data:

https://d396qusza40orc.cloudfront.net/predmachlearn/pml-testing.csv

Reference:

http://groupware.les.inf.puc-rio.br/har.

Velloso, E.; Bulling, A.; Gellersen, H.; Ugulino, W.; Fuks, H. Qualitative Activity Recognition of Weight Lifting Exercises. Proceedings of 4th International Conference in Cooperation with SIGCHI (Augmented Human '13) . Stuttgart, Germany: ACM SIGCHI, 2013.


# Execution Environment

Load required libraries. Prepare environment.


```
## R version 3.0.2 (2013-09-25)
```

```
## Loading required package: lattice
```

```
## Loading required package: ggplot2
```

# Prepare the datasets

The training and testing data set are obtained from the following urls


```r
trainingUrl <- "http://d396qusza40orc.cloudfront.net/predmachlearn/pml-training.csv"
testingUrl <- "http://d396qusza40orc.cloudfront.net/predmachlearn/pml-testing.csv"
```

Downloading and peeking into the data shows a lot of NA values for some measurements. We need to consider that when loading the data.


```r
trainingRaw <- read.csv(url(trainingUrl), na.strings=c("NA",""))
dim(trainingRaw)
```

```
## [1] 19622   160
```

```r
testingRaw <- read.csv(url(testingUrl), na.strings=c("NA",""))
dim(testingRaw)
```

```
## [1]  20 160
```

There are 160 columns. 19622 training and 20 testing rows.


```r
names(trainingRaw)
```

```
##   [1] "X"                        "user_name"               
##   [3] "raw_timestamp_part_1"     "raw_timestamp_part_2"    
##   [5] "cvtd_timestamp"           "new_window"              
##   [7] "num_window"               "roll_belt"               
##   [9] "pitch_belt"               "yaw_belt"                
##  [11] "total_accel_belt"         "kurtosis_roll_belt"      
##  [13] "kurtosis_picth_belt"      "kurtosis_yaw_belt"       
##  [15] "skewness_roll_belt"       "skewness_roll_belt.1"    
##  [17] "skewness_yaw_belt"        "max_roll_belt"           
##  [19] "max_picth_belt"           "max_yaw_belt"            
##  [21] "min_roll_belt"            "min_pitch_belt"          
##  [23] "min_yaw_belt"             "amplitude_roll_belt"     
##  [25] "amplitude_pitch_belt"     "amplitude_yaw_belt"      
##  [27] "var_total_accel_belt"     "avg_roll_belt"           
##  [29] "stddev_roll_belt"         "var_roll_belt"           
##  [31] "avg_pitch_belt"           "stddev_pitch_belt"       
##  [33] "var_pitch_belt"           "avg_yaw_belt"            
##  [35] "stddev_yaw_belt"          "var_yaw_belt"            
##  [37] "gyros_belt_x"             "gyros_belt_y"            
##  [39] "gyros_belt_z"             "accel_belt_x"            
##  [41] "accel_belt_y"             "accel_belt_z"            
##  [43] "magnet_belt_x"            "magnet_belt_y"           
##  [45] "magnet_belt_z"            "roll_arm"                
##  [47] "pitch_arm"                "yaw_arm"                 
##  [49] "total_accel_arm"          "var_accel_arm"           
##  [51] "avg_roll_arm"             "stddev_roll_arm"         
##  [53] "var_roll_arm"             "avg_pitch_arm"           
##  [55] "stddev_pitch_arm"         "var_pitch_arm"           
##  [57] "avg_yaw_arm"              "stddev_yaw_arm"          
##  [59] "var_yaw_arm"              "gyros_arm_x"             
##  [61] "gyros_arm_y"              "gyros_arm_z"             
##  [63] "accel_arm_x"              "accel_arm_y"             
##  [65] "accel_arm_z"              "magnet_arm_x"            
##  [67] "magnet_arm_y"             "magnet_arm_z"            
##  [69] "kurtosis_roll_arm"        "kurtosis_picth_arm"      
##  [71] "kurtosis_yaw_arm"         "skewness_roll_arm"       
##  [73] "skewness_pitch_arm"       "skewness_yaw_arm"        
##  [75] "max_roll_arm"             "max_picth_arm"           
##  [77] "max_yaw_arm"              "min_roll_arm"            
##  [79] "min_pitch_arm"            "min_yaw_arm"             
##  [81] "amplitude_roll_arm"       "amplitude_pitch_arm"     
##  [83] "amplitude_yaw_arm"        "roll_dumbbell"           
##  [85] "pitch_dumbbell"           "yaw_dumbbell"            
##  [87] "kurtosis_roll_dumbbell"   "kurtosis_picth_dumbbell" 
##  [89] "kurtosis_yaw_dumbbell"    "skewness_roll_dumbbell"  
##  [91] "skewness_pitch_dumbbell"  "skewness_yaw_dumbbell"   
##  [93] "max_roll_dumbbell"        "max_picth_dumbbell"      
##  [95] "max_yaw_dumbbell"         "min_roll_dumbbell"       
##  [97] "min_pitch_dumbbell"       "min_yaw_dumbbell"        
##  [99] "amplitude_roll_dumbbell"  "amplitude_pitch_dumbbell"
## [101] "amplitude_yaw_dumbbell"   "total_accel_dumbbell"    
## [103] "var_accel_dumbbell"       "avg_roll_dumbbell"       
## [105] "stddev_roll_dumbbell"     "var_roll_dumbbell"       
## [107] "avg_pitch_dumbbell"       "stddev_pitch_dumbbell"   
## [109] "var_pitch_dumbbell"       "avg_yaw_dumbbell"        
## [111] "stddev_yaw_dumbbell"      "var_yaw_dumbbell"        
## [113] "gyros_dumbbell_x"         "gyros_dumbbell_y"        
## [115] "gyros_dumbbell_z"         "accel_dumbbell_x"        
## [117] "accel_dumbbell_y"         "accel_dumbbell_z"        
## [119] "magnet_dumbbell_x"        "magnet_dumbbell_y"       
## [121] "magnet_dumbbell_z"        "roll_forearm"            
## [123] "pitch_forearm"            "yaw_forearm"             
## [125] "kurtosis_roll_forearm"    "kurtosis_picth_forearm"  
## [127] "kurtosis_yaw_forearm"     "skewness_roll_forearm"   
## [129] "skewness_pitch_forearm"   "skewness_yaw_forearm"    
## [131] "max_roll_forearm"         "max_picth_forearm"       
## [133] "max_yaw_forearm"          "min_roll_forearm"        
## [135] "min_pitch_forearm"        "min_yaw_forearm"         
## [137] "amplitude_roll_forearm"   "amplitude_pitch_forearm" 
## [139] "amplitude_yaw_forearm"    "total_accel_forearm"     
## [141] "var_accel_forearm"        "avg_roll_forearm"        
## [143] "stddev_roll_forearm"      "var_roll_forearm"        
## [145] "avg_pitch_forearm"        "stddev_pitch_forearm"    
## [147] "var_pitch_forearm"        "avg_yaw_forearm"         
## [149] "stddev_yaw_forearm"       "var_yaw_forearm"         
## [151] "gyros_forearm_x"          "gyros_forearm_y"         
## [153] "gyros_forearm_z"          "accel_forearm_x"         
## [155] "accel_forearm_y"          "accel_forearm_z"         
## [157] "magnet_forearm_x"         "magnet_forearm_y"        
## [159] "magnet_forearm_z"         "classe"
```

These variables / columns should not be used for prediction:
`"X"` index variable
`"user_name"` the person who executed the exercise
`"classe"` the classification result


```r
excludeCols <- c("X", "user_name")
trainingRaw <- trainingRaw[ , !(names(trainingRaw) %in% excludeCols)]
testingRaw <- testingRaw[ , !(names(testingRaw) %in% excludeCols)]
```

Discard columns with more than 95% NA or "" values.


```r
treshold <- dim(trainingRaw)[1] * 0.95
validColumns <- !apply(trainingRaw, 2, function(x) sum(is.na(x)) > treshold  || sum(x=="") > treshold)
trainingRaw <- trainingRaw[, validColumns]
testingRaw <- testingRaw[, validColumns]
```

Discard columns with not enough information content (near zero variables).


```r
zeroCols <- nearZeroVar(trainingRaw, saveMetrics = TRUE)
training <- trainingRaw[, zeroCols$nzv==FALSE]
testing <- testingRaw[, zeroCols$nzv==FALSE]
```

Convert `classe` column into a factor.


```r
training$classe <- factor(training$classe)
```

Split the training dataset into a 60% training and 40% validation dataset.


```r
inTrain <- createDataPartition(y=training$classe, p=0.6, list=FALSE)
validation <- training[-inTrain, ]
training <- training[inTrain, ];
```

Now we have prepared 3 different data sets:
`training` for training the prediction model
`validation` for validating the prediction model
`testing` for applying the prediction model and verifying the out of sample error


# Train a prediction model

Train 2 different models: generalized boosted regression model `gbm`, linear discriminant analysis `lda`.
Note: long running training


```r
modelGBM <- train(classe ~ ., data=training, method="gbm")
```

```
## Loading required package: gbm
```

```
## Loading required package: survival
```

```
## Loading required package: splines
```

```
## 
## Attaching package: 'survival'
```

```
## The following object is masked from 'package:caret':
## 
##     cluster
```

```
## Loading required package: parallel
```

```
## Loaded gbm 2.1.1
```

```
## Loading required package: plyr
```

```
## Loading required namespace: e1071
```

```
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1308
##      2        1.5232             nan     0.1000    0.0887
##      3        1.4641             nan     0.1000    0.0684
##      4        1.4196             nan     0.1000    0.0578
##      5        1.3823             nan     0.1000    0.0515
##      6        1.3488             nan     0.1000    0.0467
##      7        1.3188             nan     0.1000    0.0450
##      8        1.2911             nan     0.1000    0.0434
##      9        1.2655             nan     0.1000    0.0374
##     10        1.2396             nan     0.1000    0.0373
##     20        1.0538             nan     0.1000    0.0235
##     40        0.8383             nan     0.1000    0.0148
##     60        0.7008             nan     0.1000    0.0094
##     80        0.5964             nan     0.1000    0.0052
##    100        0.5218             nan     0.1000    0.0067
##    120        0.4575             nan     0.1000    0.0032
##    140        0.4065             nan     0.1000    0.0022
##    150        0.3849             nan     0.1000    0.0027
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1995
##      2        1.4828             nan     0.1000    0.1358
##      3        1.3948             nan     0.1000    0.1073
##      4        1.3250             nan     0.1000    0.1024
##      5        1.2595             nan     0.1000    0.0938
##      6        1.2002             nan     0.1000    0.0929
##      7        1.1430             nan     0.1000    0.0705
##      8        1.0991             nan     0.1000    0.0656
##      9        1.0587             nan     0.1000    0.0637
##     10        1.0188             nan     0.1000    0.0578
##     20        0.7605             nan     0.1000    0.0320
##     40        0.4689             nan     0.1000    0.0188
##     60        0.3134             nan     0.1000    0.0091
##     80        0.2206             nan     0.1000    0.0050
##    100        0.1583             nan     0.1000    0.0028
##    120        0.1175             nan     0.1000    0.0029
##    140        0.0864             nan     0.1000    0.0018
##    150        0.0759             nan     0.1000    0.0014
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.2520
##      2        1.4487             nan     0.1000    0.1826
##      3        1.3331             nan     0.1000    0.1479
##      4        1.2379             nan     0.1000    0.1223
##      5        1.1599             nan     0.1000    0.1060
##      6        1.0932             nan     0.1000    0.0896
##      7        1.0359             nan     0.1000    0.0943
##      8        0.9787             nan     0.1000    0.0854
##      9        0.9269             nan     0.1000    0.0649
##     10        0.8856             nan     0.1000    0.0712
##     20        0.5807             nan     0.1000    0.0295
##     40        0.2905             nan     0.1000    0.0143
##     60        0.1651             nan     0.1000    0.0084
##     80        0.0990             nan     0.1000    0.0035
##    100        0.0647             nan     0.1000    0.0008
##    120        0.0455             nan     0.1000    0.0010
##    140        0.0331             nan     0.1000    0.0005
##    150        0.0289             nan     0.1000    0.0007
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1285
##      2        1.5243             nan     0.1000    0.0838
##      3        1.4677             nan     0.1000    0.0654
##      4        1.4242             nan     0.1000    0.0509
##      5        1.3893             nan     0.1000    0.0531
##      6        1.3550             nan     0.1000    0.0475
##      7        1.3247             nan     0.1000    0.0448
##      8        1.2972             nan     0.1000    0.0436
##      9        1.2688             nan     0.1000    0.0327
##     10        1.2477             nan     0.1000    0.0334
##     20        1.0621             nan     0.1000    0.0227
##     40        0.8479             nan     0.1000    0.0112
##     60        0.7117             nan     0.1000    0.0088
##     80        0.6091             nan     0.1000    0.0094
##    100        0.5307             nan     0.1000    0.0050
##    120        0.4681             nan     0.1000    0.0038
##    140        0.4143             nan     0.1000    0.0033
##    150        0.3904             nan     0.1000    0.0018
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1979
##      2        1.4830             nan     0.1000    0.1324
##      3        1.3945             nan     0.1000    0.1187
##      4        1.3187             nan     0.1000    0.0963
##      5        1.2573             nan     0.1000    0.0864
##      6        1.2021             nan     0.1000    0.0890
##      7        1.1458             nan     0.1000    0.0744
##      8        1.0989             nan     0.1000    0.0677
##      9        1.0563             nan     0.1000    0.0570
##     10        1.0200             nan     0.1000    0.0544
##     20        0.7590             nan     0.1000    0.0255
##     40        0.4762             nan     0.1000    0.0147
##     60        0.3224             nan     0.1000    0.0094
##     80        0.2253             nan     0.1000    0.0056
##    100        0.1645             nan     0.1000    0.0036
##    120        0.1185             nan     0.1000    0.0019
##    140        0.0895             nan     0.1000    0.0021
##    150        0.0785             nan     0.1000    0.0011
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.2533
##      2        1.4484             nan     0.1000    0.1707
##      3        1.3385             nan     0.1000    0.1513
##      4        1.2418             nan     0.1000    0.1272
##      5        1.1608             nan     0.1000    0.1110
##      6        1.0899             nan     0.1000    0.0918
##      7        1.0336             nan     0.1000    0.1035
##      8        0.9713             nan     0.1000    0.0660
##      9        0.9280             nan     0.1000    0.0698
##     10        0.8848             nan     0.1000    0.0753
##     20        0.5770             nan     0.1000    0.0317
##     40        0.2975             nan     0.1000    0.0124
##     60        0.1671             nan     0.1000    0.0067
##     80        0.1014             nan     0.1000    0.0039
##    100        0.0652             nan     0.1000    0.0020
##    120        0.0459             nan     0.1000    0.0013
##    140        0.0324             nan     0.1000    0.0008
##    150        0.0279             nan     0.1000    0.0009
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1268
##      2        1.5236             nan     0.1000    0.0868
##      3        1.4661             nan     0.1000    0.0681
##      4        1.4214             nan     0.1000    0.0544
##      5        1.3847             nan     0.1000    0.0445
##      6        1.3548             nan     0.1000    0.0483
##      7        1.3238             nan     0.1000    0.0427
##      8        1.2974             nan     0.1000    0.0418
##      9        1.2703             nan     0.1000    0.0368
##     10        1.2462             nan     0.1000    0.0360
##     20        1.0625             nan     0.1000    0.0241
##     40        0.8414             nan     0.1000    0.0112
##     60        0.7068             nan     0.1000    0.0079
##     80        0.6018             nan     0.1000    0.0082
##    100        0.5239             nan     0.1000    0.0050
##    120        0.4600             nan     0.1000    0.0034
##    140        0.4087             nan     0.1000    0.0033
##    150        0.3870             nan     0.1000    0.0025
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1938
##      2        1.4844             nan     0.1000    0.1382
##      3        1.3960             nan     0.1000    0.1072
##      4        1.3252             nan     0.1000    0.1021
##      5        1.2611             nan     0.1000    0.0946
##      6        1.2012             nan     0.1000    0.0744
##      7        1.1532             nan     0.1000    0.0748
##      8        1.1069             nan     0.1000    0.0743
##      9        1.0614             nan     0.1000    0.0677
##     10        1.0205             nan     0.1000    0.0577
##     20        0.7589             nan     0.1000    0.0278
##     40        0.4698             nan     0.1000    0.0150
##     60        0.3141             nan     0.1000    0.0093
##     80        0.2212             nan     0.1000    0.0061
##    100        0.1590             nan     0.1000    0.0036
##    120        0.1194             nan     0.1000    0.0019
##    140        0.0903             nan     0.1000    0.0016
##    150        0.0807             nan     0.1000    0.0014
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.2542
##      2        1.4475             nan     0.1000    0.1788
##      3        1.3332             nan     0.1000    0.1395
##      4        1.2441             nan     0.1000    0.1257
##      5        1.1648             nan     0.1000    0.1134
##      6        1.0936             nan     0.1000    0.0945
##      7        1.0342             nan     0.1000    0.0913
##      8        0.9777             nan     0.1000    0.0855
##      9        0.9256             nan     0.1000    0.0804
##     10        0.8753             nan     0.1000    0.0722
##     20        0.5758             nan     0.1000    0.0335
##     40        0.2922             nan     0.1000    0.0135
##     60        0.1643             nan     0.1000    0.0083
##     80        0.1010             nan     0.1000    0.0035
##    100        0.0673             nan     0.1000    0.0025
##    120        0.0470             nan     0.1000    0.0011
##    140        0.0345             nan     0.1000    0.0007
##    150        0.0294             nan     0.1000    0.0003
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1270
##      2        1.5244             nan     0.1000    0.0886
##      3        1.4664             nan     0.1000    0.0688
##      4        1.4217             nan     0.1000    0.0519
##      5        1.3863             nan     0.1000    0.0551
##      6        1.3509             nan     0.1000    0.0405
##      7        1.3227             nan     0.1000    0.0419
##      8        1.2963             nan     0.1000    0.0428
##      9        1.2674             nan     0.1000    0.0337
##     10        1.2452             nan     0.1000    0.0344
##     20        1.0644             nan     0.1000    0.0228
##     40        0.8470             nan     0.1000    0.0142
##     60        0.7058             nan     0.1000    0.0088
##     80        0.6045             nan     0.1000    0.0063
##    100        0.5218             nan     0.1000    0.0063
##    120        0.4563             nan     0.1000    0.0053
##    140        0.4039             nan     0.1000    0.0030
##    150        0.3819             nan     0.1000    0.0022
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1986
##      2        1.4811             nan     0.1000    0.1451
##      3        1.3886             nan     0.1000    0.1063
##      4        1.3189             nan     0.1000    0.0866
##      5        1.2624             nan     0.1000    0.0799
##      6        1.2109             nan     0.1000    0.0843
##      7        1.1575             nan     0.1000    0.0771
##      8        1.1090             nan     0.1000    0.0699
##      9        1.0654             nan     0.1000    0.0572
##     10        1.0289             nan     0.1000    0.0503
##     20        0.7675             nan     0.1000    0.0289
##     40        0.4712             nan     0.1000    0.0147
##     60        0.3176             nan     0.1000    0.0100
##     80        0.2234             nan     0.1000    0.0060
##    100        0.1616             nan     0.1000    0.0043
##    120        0.1180             nan     0.1000    0.0033
##    140        0.0895             nan     0.1000    0.0013
##    150        0.0787             nan     0.1000    0.0012
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.2530
##      2        1.4464             nan     0.1000    0.1832
##      3        1.3278             nan     0.1000    0.1443
##      4        1.2366             nan     0.1000    0.1227
##      5        1.1587             nan     0.1000    0.0971
##      6        1.0954             nan     0.1000    0.0968
##      7        1.0356             nan     0.1000    0.0873
##      8        0.9820             nan     0.1000    0.0790
##      9        0.9315             nan     0.1000    0.0757
##     10        0.8858             nan     0.1000    0.0645
##     20        0.5865             nan     0.1000    0.0438
##     40        0.2920             nan     0.1000    0.0126
##     60        0.1664             nan     0.1000    0.0062
##     80        0.1037             nan     0.1000    0.0035
##    100        0.0689             nan     0.1000    0.0025
##    120        0.0479             nan     0.1000    0.0014
##    140        0.0346             nan     0.1000    0.0005
##    150        0.0298             nan     0.1000    0.0005
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1286
##      2        1.5227             nan     0.1000    0.0873
##      3        1.4637             nan     0.1000    0.0634
##      4        1.4209             nan     0.1000    0.0594
##      5        1.3825             nan     0.1000    0.0461
##      6        1.3523             nan     0.1000    0.0498
##      7        1.3206             nan     0.1000    0.0459
##      8        1.2925             nan     0.1000    0.0412
##      9        1.2655             nan     0.1000    0.0373
##     10        1.2424             nan     0.1000    0.0308
##     20        1.0619             nan     0.1000    0.0195
##     40        0.8430             nan     0.1000    0.0129
##     60        0.7051             nan     0.1000    0.0066
##     80        0.6065             nan     0.1000    0.0060
##    100        0.5253             nan     0.1000    0.0026
##    120        0.4626             nan     0.1000    0.0040
##    140        0.4110             nan     0.1000    0.0033
##    150        0.3888             nan     0.1000    0.0027
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1927
##      2        1.4841             nan     0.1000    0.1400
##      3        1.3931             nan     0.1000    0.1099
##      4        1.3216             nan     0.1000    0.1044
##      5        1.2553             nan     0.1000    0.0925
##      6        1.1967             nan     0.1000    0.0634
##      7        1.1554             nan     0.1000    0.0788
##      8        1.1071             nan     0.1000    0.0564
##      9        1.0710             nan     0.1000    0.0613
##     10        1.0319             nan     0.1000    0.0586
##     20        0.7659             nan     0.1000    0.0389
##     40        0.4833             nan     0.1000    0.0173
##     60        0.3170             nan     0.1000    0.0105
##     80        0.2258             nan     0.1000    0.0051
##    100        0.1638             nan     0.1000    0.0038
##    120        0.1217             nan     0.1000    0.0030
##    140        0.0931             nan     0.1000    0.0018
##    150        0.0809             nan     0.1000    0.0016
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.2463
##      2        1.4500             nan     0.1000    0.1836
##      3        1.3317             nan     0.1000    0.1447
##      4        1.2404             nan     0.1000    0.1154
##      5        1.1671             nan     0.1000    0.1011
##      6        1.1025             nan     0.1000    0.0990
##      7        1.0397             nan     0.1000    0.0843
##      8        0.9867             nan     0.1000    0.0799
##      9        0.9373             nan     0.1000    0.0643
##     10        0.8970             nan     0.1000    0.0779
##     20        0.5816             nan     0.1000    0.0300
##     40        0.2924             nan     0.1000    0.0160
##     60        0.1679             nan     0.1000    0.0081
##     80        0.1016             nan     0.1000    0.0051
##    100        0.0683             nan     0.1000    0.0025
##    120        0.0484             nan     0.1000    0.0010
##    140        0.0352             nan     0.1000    0.0004
##    150        0.0304             nan     0.1000    0.0006
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1322
##      2        1.5226             nan     0.1000    0.0899
##      3        1.4628             nan     0.1000    0.0659
##      4        1.4190             nan     0.1000    0.0528
##      5        1.3827             nan     0.1000    0.0529
##      6        1.3475             nan     0.1000    0.0449
##      7        1.3183             nan     0.1000    0.0460
##      8        1.2902             nan     0.1000    0.0384
##      9        1.2653             nan     0.1000    0.0398
##     10        1.2384             nan     0.1000    0.0315
##     20        1.0548             nan     0.1000    0.0236
##     40        0.8375             nan     0.1000    0.0123
##     60        0.6999             nan     0.1000    0.0068
##     80        0.5998             nan     0.1000    0.0063
##    100        0.5205             nan     0.1000    0.0044
##    120        0.4570             nan     0.1000    0.0039
##    140        0.4076             nan     0.1000    0.0037
##    150        0.3835             nan     0.1000    0.0024
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1983
##      2        1.4810             nan     0.1000    0.1396
##      3        1.3930             nan     0.1000    0.1152
##      4        1.3199             nan     0.1000    0.1076
##      5        1.2520             nan     0.1000    0.0872
##      6        1.1961             nan     0.1000    0.0827
##      7        1.1439             nan     0.1000    0.0747
##      8        1.0961             nan     0.1000    0.0706
##      9        1.0509             nan     0.1000    0.0552
##     10        1.0142             nan     0.1000    0.0572
##     20        0.7627             nan     0.1000    0.0281
##     40        0.4675             nan     0.1000    0.0193
##     60        0.3138             nan     0.1000    0.0099
##     80        0.2159             nan     0.1000    0.0088
##    100        0.1536             nan     0.1000    0.0041
##    120        0.1141             nan     0.1000    0.0023
##    140        0.0871             nan     0.1000    0.0018
##    150        0.0774             nan     0.1000    0.0013
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.2581
##      2        1.4465             nan     0.1000    0.1786
##      3        1.3301             nan     0.1000    0.1431
##      4        1.2386             nan     0.1000    0.1305
##      5        1.1578             nan     0.1000    0.1035
##      6        1.0922             nan     0.1000    0.1015
##      7        1.0292             nan     0.1000    0.0865
##      8        0.9741             nan     0.1000    0.0840
##      9        0.9231             nan     0.1000    0.0684
##     10        0.8810             nan     0.1000    0.0621
##     20        0.5803             nan     0.1000    0.0316
##     40        0.2952             nan     0.1000    0.0124
##     60        0.1644             nan     0.1000    0.0082
##     80        0.1006             nan     0.1000    0.0028
##    100        0.0663             nan     0.1000    0.0018
##    120        0.0465             nan     0.1000    0.0017
##    140        0.0343             nan     0.1000    0.0008
##    150        0.0296             nan     0.1000    0.0005
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1287
##      2        1.5211             nan     0.1000    0.0923
##      3        1.4608             nan     0.1000    0.0674
##      4        1.4148             nan     0.1000    0.0559
##      5        1.3778             nan     0.1000    0.0545
##      6        1.3429             nan     0.1000    0.0486
##      7        1.3119             nan     0.1000    0.0458
##      8        1.2835             nan     0.1000    0.0364
##      9        1.2603             nan     0.1000    0.0347
##     10        1.2355             nan     0.1000    0.0384
##     20        1.0474             nan     0.1000    0.0210
##     40        0.8302             nan     0.1000    0.0098
##     60        0.6914             nan     0.1000    0.0070
##     80        0.5925             nan     0.1000    0.0067
##    100        0.5132             nan     0.1000    0.0048
##    120        0.4502             nan     0.1000    0.0047
##    140        0.4013             nan     0.1000    0.0033
##    150        0.3773             nan     0.1000    0.0033
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1983
##      2        1.4819             nan     0.1000    0.1387
##      3        1.3921             nan     0.1000    0.1196
##      4        1.3162             nan     0.1000    0.1026
##      5        1.2525             nan     0.1000    0.0859
##      6        1.1972             nan     0.1000    0.0899
##      7        1.1395             nan     0.1000    0.0763
##      8        1.0917             nan     0.1000    0.0663
##      9        1.0503             nan     0.1000    0.0660
##     10        1.0090             nan     0.1000    0.0575
##     20        0.7556             nan     0.1000    0.0369
##     40        0.4560             nan     0.1000    0.0154
##     60        0.2998             nan     0.1000    0.0097
##     80        0.2098             nan     0.1000    0.0059
##    100        0.1494             nan     0.1000    0.0033
##    120        0.1100             nan     0.1000    0.0031
##    140        0.0832             nan     0.1000    0.0009
##    150        0.0735             nan     0.1000    0.0015
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.2541
##      2        1.4478             nan     0.1000    0.1886
##      3        1.3286             nan     0.1000    0.1367
##      4        1.2388             nan     0.1000    0.1425
##      5        1.1505             nan     0.1000    0.1081
##      6        1.0816             nan     0.1000    0.1083
##      7        1.0136             nan     0.1000    0.0887
##      8        0.9584             nan     0.1000    0.0791
##      9        0.9082             nan     0.1000    0.0617
##     10        0.8701             nan     0.1000    0.0678
##     20        0.5673             nan     0.1000    0.0275
##     40        0.2849             nan     0.1000    0.0145
##     60        0.1575             nan     0.1000    0.0073
##     80        0.0962             nan     0.1000    0.0035
##    100        0.0632             nan     0.1000    0.0017
##    120        0.0436             nan     0.1000    0.0010
##    140        0.0313             nan     0.1000    0.0006
##    150        0.0273             nan     0.1000    0.0005
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1260
##      2        1.5253             nan     0.1000    0.0896
##      3        1.4681             nan     0.1000    0.0652
##      4        1.4243             nan     0.1000    0.0540
##      5        1.3889             nan     0.1000    0.0518
##      6        1.3556             nan     0.1000    0.0490
##      7        1.3251             nan     0.1000    0.0438
##      8        1.2976             nan     0.1000    0.0440
##      9        1.2692             nan     0.1000    0.0361
##     10        1.2446             nan     0.1000    0.0377
##     20        1.0616             nan     0.1000    0.0227
##     40        0.8412             nan     0.1000    0.0140
##     60        0.6991             nan     0.1000    0.0083
##     80        0.5958             nan     0.1000    0.0078
##    100        0.5140             nan     0.1000    0.0059
##    120        0.4510             nan     0.1000    0.0054
##    140        0.3989             nan     0.1000    0.0021
##    150        0.3789             nan     0.1000    0.0033
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1900
##      2        1.4854             nan     0.1000    0.1457
##      3        1.3924             nan     0.1000    0.1165
##      4        1.3178             nan     0.1000    0.1024
##      5        1.2526             nan     0.1000    0.0898
##      6        1.1960             nan     0.1000    0.0836
##      7        1.1421             nan     0.1000    0.0728
##      8        1.0956             nan     0.1000    0.0623
##      9        1.0562             nan     0.1000    0.0623
##     10        1.0175             nan     0.1000    0.0590
##     20        0.7566             nan     0.1000    0.0326
##     40        0.4658             nan     0.1000    0.0170
##     60        0.3085             nan     0.1000    0.0078
##     80        0.2125             nan     0.1000    0.0053
##    100        0.1530             nan     0.1000    0.0034
##    120        0.1125             nan     0.1000    0.0022
##    140        0.0849             nan     0.1000    0.0009
##    150        0.0756             nan     0.1000    0.0014
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.2511
##      2        1.4516             nan     0.1000    0.1838
##      3        1.3346             nan     0.1000    0.1478
##      4        1.2415             nan     0.1000    0.1191
##      5        1.1663             nan     0.1000    0.1121
##      6        1.0959             nan     0.1000    0.1049
##      7        1.0310             nan     0.1000    0.0939
##      8        0.9726             nan     0.1000    0.0868
##      9        0.9203             nan     0.1000    0.0768
##     10        0.8739             nan     0.1000    0.0628
##     20        0.5757             nan     0.1000    0.0341
##     40        0.2904             nan     0.1000    0.0157
##     60        0.1629             nan     0.1000    0.0050
##     80        0.0994             nan     0.1000    0.0045
##    100        0.0654             nan     0.1000    0.0028
##    120        0.0444             nan     0.1000    0.0013
##    140        0.0321             nan     0.1000    0.0007
##    150        0.0279             nan     0.1000    0.0005
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1289
##      2        1.5238             nan     0.1000    0.0874
##      3        1.4670             nan     0.1000    0.0684
##      4        1.4218             nan     0.1000    0.0571
##      5        1.3853             nan     0.1000    0.0495
##      6        1.3540             nan     0.1000    0.0447
##      7        1.3255             nan     0.1000    0.0425
##      8        1.2991             nan     0.1000    0.0415
##      9        1.2713             nan     0.1000    0.0408
##     10        1.2441             nan     0.1000    0.0391
##     20        1.0597             nan     0.1000    0.0229
##     40        0.8409             nan     0.1000    0.0122
##     60        0.7037             nan     0.1000    0.0082
##     80        0.5977             nan     0.1000    0.0069
##    100        0.5214             nan     0.1000    0.0057
##    120        0.4568             nan     0.1000    0.0041
##    140        0.4054             nan     0.1000    0.0022
##    150        0.3847             nan     0.1000    0.0037
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1948
##      2        1.4837             nan     0.1000    0.1319
##      3        1.3973             nan     0.1000    0.1145
##      4        1.3232             nan     0.1000    0.0982
##      5        1.2604             nan     0.1000    0.0803
##      6        1.2077             nan     0.1000    0.0821
##      7        1.1562             nan     0.1000    0.0717
##      8        1.1106             nan     0.1000    0.0779
##      9        1.0634             nan     0.1000    0.0574
##     10        1.0268             nan     0.1000    0.0573
##     20        0.7552             nan     0.1000    0.0303
##     40        0.4698             nan     0.1000    0.0116
##     60        0.3101             nan     0.1000    0.0099
##     80        0.2179             nan     0.1000    0.0058
##    100        0.1570             nan     0.1000    0.0041
##    120        0.1162             nan     0.1000    0.0020
##    140        0.0892             nan     0.1000    0.0017
##    150        0.0783             nan     0.1000    0.0005
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.2578
##      2        1.4454             nan     0.1000    0.1879
##      3        1.3261             nan     0.1000    0.1494
##      4        1.2303             nan     0.1000    0.1163
##      5        1.1534             nan     0.1000    0.1127
##      6        1.0828             nan     0.1000    0.0813
##      7        1.0299             nan     0.1000    0.0886
##      8        0.9751             nan     0.1000    0.0824
##      9        0.9221             nan     0.1000    0.0808
##     10        0.8731             nan     0.1000    0.0685
##     20        0.5823             nan     0.1000    0.0491
##     40        0.2903             nan     0.1000    0.0143
##     60        0.1658             nan     0.1000    0.0069
##     80        0.1003             nan     0.1000    0.0025
##    100        0.0669             nan     0.1000    0.0016
##    120        0.0468             nan     0.1000    0.0015
##    140        0.0330             nan     0.1000    0.0007
##    150        0.0285             nan     0.1000    0.0004
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1237
##      2        1.5247             nan     0.1000    0.0845
##      3        1.4676             nan     0.1000    0.0636
##      4        1.4240             nan     0.1000    0.0581
##      5        1.3849             nan     0.1000    0.0491
##      6        1.3534             nan     0.1000    0.0461
##      7        1.3229             nan     0.1000    0.0406
##      8        1.2971             nan     0.1000    0.0452
##      9        1.2668             nan     0.1000    0.0382
##     10        1.2413             nan     0.1000    0.0334
##     20        1.0568             nan     0.1000    0.0209
##     40        0.8414             nan     0.1000    0.0104
##     60        0.7051             nan     0.1000    0.0081
##     80        0.6035             nan     0.1000    0.0058
##    100        0.5230             nan     0.1000    0.0054
##    120        0.4598             nan     0.1000    0.0039
##    140        0.4059             nan     0.1000    0.0026
##    150        0.3837             nan     0.1000    0.0037
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1996
##      2        1.4822             nan     0.1000    0.1331
##      3        1.3940             nan     0.1000    0.1076
##      4        1.3245             nan     0.1000    0.1077
##      5        1.2578             nan     0.1000    0.0774
##      6        1.2075             nan     0.1000    0.0824
##      7        1.1552             nan     0.1000    0.0666
##      8        1.1126             nan     0.1000    0.0805
##      9        1.0636             nan     0.1000    0.0570
##     10        1.0281             nan     0.1000    0.0621
##     20        0.7579             nan     0.1000    0.0273
##     40        0.4685             nan     0.1000    0.0168
##     60        0.3103             nan     0.1000    0.0065
##     80        0.2167             nan     0.1000    0.0056
##    100        0.1574             nan     0.1000    0.0028
##    120        0.1169             nan     0.1000    0.0020
##    140        0.0888             nan     0.1000    0.0009
##    150        0.0777             nan     0.1000    0.0008
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.2535
##      2        1.4480             nan     0.1000    0.1837
##      3        1.3313             nan     0.1000    0.1490
##      4        1.2387             nan     0.1000    0.1157
##      5        1.1654             nan     0.1000    0.0954
##      6        1.1028             nan     0.1000    0.1059
##      7        1.0374             nan     0.1000    0.0850
##      8        0.9847             nan     0.1000    0.0861
##      9        0.9315             nan     0.1000    0.0795
##     10        0.8844             nan     0.1000    0.0654
##     20        0.5802             nan     0.1000    0.0380
##     40        0.2914             nan     0.1000    0.0131
##     60        0.1684             nan     0.1000    0.0069
##     80        0.1053             nan     0.1000    0.0048
##    100        0.0691             nan     0.1000    0.0018
##    120        0.0476             nan     0.1000    0.0015
##    140        0.0350             nan     0.1000    0.0009
##    150        0.0302             nan     0.1000    0.0008
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1319
##      2        1.5226             nan     0.1000    0.0870
##      3        1.4648             nan     0.1000    0.0694
##      4        1.4192             nan     0.1000    0.0543
##      5        1.3829             nan     0.1000    0.0515
##      6        1.3505             nan     0.1000    0.0453
##      7        1.3214             nan     0.1000    0.0442
##      8        1.2944             nan     0.1000    0.0419
##      9        1.2658             nan     0.1000    0.0349
##     10        1.2422             nan     0.1000    0.0368
##     20        1.0597             nan     0.1000    0.0237
##     40        0.8411             nan     0.1000    0.0120
##     60        0.7020             nan     0.1000    0.0106
##     80        0.5985             nan     0.1000    0.0064
##    100        0.5180             nan     0.1000    0.0038
##    120        0.4572             nan     0.1000    0.0053
##    140        0.4042             nan     0.1000    0.0027
##    150        0.3827             nan     0.1000    0.0030
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1994
##      2        1.4826             nan     0.1000    0.1454
##      3        1.3908             nan     0.1000    0.1074
##      4        1.3207             nan     0.1000    0.1040
##      5        1.2556             nan     0.1000    0.0899
##      6        1.1985             nan     0.1000    0.0807
##      7        1.1479             nan     0.1000    0.0640
##      8        1.1066             nan     0.1000    0.0557
##      9        1.0710             nan     0.1000    0.0576
##     10        1.0350             nan     0.1000    0.0657
##     20        0.7594             nan     0.1000    0.0302
##     40        0.4618             nan     0.1000    0.0143
##     60        0.3096             nan     0.1000    0.0097
##     80        0.2156             nan     0.1000    0.0062
##    100        0.1557             nan     0.1000    0.0044
##    120        0.1170             nan     0.1000    0.0021
##    140        0.0900             nan     0.1000    0.0017
##    150        0.0789             nan     0.1000    0.0018
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.2515
##      2        1.4483             nan     0.1000    0.1842
##      3        1.3325             nan     0.1000    0.1460
##      4        1.2399             nan     0.1000    0.1350
##      5        1.1560             nan     0.1000    0.1035
##      6        1.0901             nan     0.1000    0.1092
##      7        1.0222             nan     0.1000    0.0854
##      8        0.9696             nan     0.1000    0.0861
##      9        0.9162             nan     0.1000    0.0667
##     10        0.8751             nan     0.1000    0.0616
##     20        0.5764             nan     0.1000    0.0289
##     40        0.2902             nan     0.1000    0.0122
##     60        0.1642             nan     0.1000    0.0062
##     80        0.1018             nan     0.1000    0.0034
##    100        0.0683             nan     0.1000    0.0022
##    120        0.0487             nan     0.1000    0.0013
##    140        0.0363             nan     0.1000    0.0009
##    150        0.0311             nan     0.1000    0.0004
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1242
##      2        1.5257             nan     0.1000    0.0903
##      3        1.4676             nan     0.1000    0.0670
##      4        1.4234             nan     0.1000    0.0521
##      5        1.3882             nan     0.1000    0.0517
##      6        1.3545             nan     0.1000    0.0457
##      7        1.3256             nan     0.1000    0.0458
##      8        1.2969             nan     0.1000    0.0380
##      9        1.2718             nan     0.1000    0.0357
##     10        1.2470             nan     0.1000    0.0357
##     20        1.0641             nan     0.1000    0.0206
##     40        0.8444             nan     0.1000    0.0125
##     60        0.7066             nan     0.1000    0.0089
##     80        0.6024             nan     0.1000    0.0058
##    100        0.5231             nan     0.1000    0.0054
##    120        0.4594             nan     0.1000    0.0036
##    140        0.4069             nan     0.1000    0.0030
##    150        0.3849             nan     0.1000    0.0028
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1911
##      2        1.4853             nan     0.1000    0.1367
##      3        1.3981             nan     0.1000    0.1085
##      4        1.3295             nan     0.1000    0.1036
##      5        1.2631             nan     0.1000    0.0842
##      6        1.2075             nan     0.1000    0.0677
##      7        1.1634             nan     0.1000    0.0720
##      8        1.1187             nan     0.1000    0.0834
##      9        1.0695             nan     0.1000    0.0543
##     10        1.0350             nan     0.1000    0.0494
##     20        0.7582             nan     0.1000    0.0305
##     40        0.4679             nan     0.1000    0.0151
##     60        0.3135             nan     0.1000    0.0103
##     80        0.2130             nan     0.1000    0.0043
##    100        0.1562             nan     0.1000    0.0040
##    120        0.1167             nan     0.1000    0.0022
##    140        0.0882             nan     0.1000    0.0020
##    150        0.0775             nan     0.1000    0.0016
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.2539
##      2        1.4506             nan     0.1000    0.1843
##      3        1.3347             nan     0.1000    0.1440
##      4        1.2417             nan     0.1000    0.1172
##      5        1.1662             nan     0.1000    0.1087
##      6        1.0981             nan     0.1000    0.0920
##      7        1.0404             nan     0.1000    0.0800
##      8        0.9895             nan     0.1000    0.0784
##      9        0.9405             nan     0.1000    0.0634
##     10        0.9010             nan     0.1000    0.0717
##     20        0.5931             nan     0.1000    0.0321
##     40        0.2958             nan     0.1000    0.0130
##     60        0.1675             nan     0.1000    0.0084
##     80        0.1004             nan     0.1000    0.0044
##    100        0.0663             nan     0.1000    0.0020
##    120        0.0468             nan     0.1000    0.0012
##    140        0.0350             nan     0.1000    0.0009
##    150        0.0298             nan     0.1000    0.0006
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1262
##      2        1.5242             nan     0.1000    0.0864
##      3        1.4666             nan     0.1000    0.0668
##      4        1.4215             nan     0.1000    0.0525
##      5        1.3865             nan     0.1000    0.0524
##      6        1.3523             nan     0.1000    0.0472
##      7        1.3222             nan     0.1000    0.0439
##      8        1.2952             nan     0.1000    0.0451
##      9        1.2655             nan     0.1000    0.0307
##     10        1.2448             nan     0.1000    0.0330
##     20        1.0585             nan     0.1000    0.0187
##     40        0.8407             nan     0.1000    0.0120
##     60        0.6994             nan     0.1000    0.0089
##     80        0.5945             nan     0.1000    0.0069
##    100        0.5156             nan     0.1000    0.0050
##    120        0.4519             nan     0.1000    0.0041
##    140        0.4013             nan     0.1000    0.0025
##    150        0.3794             nan     0.1000    0.0021
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1889
##      2        1.4852             nan     0.1000    0.1318
##      3        1.3980             nan     0.1000    0.1123
##      4        1.3254             nan     0.1000    0.0922
##      5        1.2658             nan     0.1000    0.0988
##      6        1.2027             nan     0.1000    0.1045
##      7        1.1393             nan     0.1000    0.0661
##      8        1.0978             nan     0.1000    0.0680
##      9        1.0559             nan     0.1000    0.0568
##     10        1.0205             nan     0.1000    0.0577
##     20        0.7524             nan     0.1000    0.0301
##     40        0.4623             nan     0.1000    0.0194
##     60        0.3063             nan     0.1000    0.0101
##     80        0.2142             nan     0.1000    0.0053
##    100        0.1514             nan     0.1000    0.0034
##    120        0.1113             nan     0.1000    0.0018
##    140        0.0846             nan     0.1000    0.0016
##    150        0.0731             nan     0.1000    0.0011
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.2460
##      2        1.4495             nan     0.1000    0.1799
##      3        1.3347             nan     0.1000    0.1499
##      4        1.2365             nan     0.1000    0.1236
##      5        1.1583             nan     0.1000    0.1125
##      6        1.0888             nan     0.1000    0.0968
##      7        1.0265             nan     0.1000    0.0852
##      8        0.9729             nan     0.1000    0.0736
##      9        0.9257             nan     0.1000    0.0846
##     10        0.8746             nan     0.1000    0.0711
##     20        0.5616             nan     0.1000    0.0344
##     40        0.2870             nan     0.1000    0.0158
##     60        0.1590             nan     0.1000    0.0083
##     80        0.0963             nan     0.1000    0.0030
##    100        0.0621             nan     0.1000    0.0015
##    120        0.0423             nan     0.1000    0.0009
##    140        0.0306             nan     0.1000    0.0008
##    150        0.0263             nan     0.1000    0.0005
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1295
##      2        1.5229             nan     0.1000    0.0891
##      3        1.4642             nan     0.1000    0.0675
##      4        1.4199             nan     0.1000    0.0538
##      5        1.3841             nan     0.1000    0.0475
##      6        1.3517             nan     0.1000    0.0512
##      7        1.3204             nan     0.1000    0.0447
##      8        1.2929             nan     0.1000    0.0383
##      9        1.2668             nan     0.1000    0.0346
##     10        1.2448             nan     0.1000    0.0376
##     20        1.0570             nan     0.1000    0.0217
##     40        0.8375             nan     0.1000    0.0118
##     60        0.7009             nan     0.1000    0.0070
##     80        0.5977             nan     0.1000    0.0061
##    100        0.5185             nan     0.1000    0.0053
##    120        0.4515             nan     0.1000    0.0039
##    140        0.4019             nan     0.1000    0.0042
##    150        0.3774             nan     0.1000    0.0024
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1956
##      2        1.4830             nan     0.1000    0.1335
##      3        1.3957             nan     0.1000    0.1171
##      4        1.3201             nan     0.1000    0.0874
##      5        1.2632             nan     0.1000    0.0971
##      6        1.2002             nan     0.1000    0.0801
##      7        1.1480             nan     0.1000    0.0797
##      8        1.0994             nan     0.1000    0.0748
##      9        1.0541             nan     0.1000    0.0552
##     10        1.0181             nan     0.1000    0.0619
##     20        0.7512             nan     0.1000    0.0325
##     40        0.4639             nan     0.1000    0.0184
##     60        0.3075             nan     0.1000    0.0139
##     80        0.2112             nan     0.1000    0.0049
##    100        0.1508             nan     0.1000    0.0052
##    120        0.1101             nan     0.1000    0.0028
##    140        0.0813             nan     0.1000    0.0018
##    150        0.0708             nan     0.1000    0.0014
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.2498
##      2        1.4496             nan     0.1000    0.1853
##      3        1.3327             nan     0.1000    0.1594
##      4        1.2347             nan     0.1000    0.1281
##      5        1.1529             nan     0.1000    0.1132
##      6        1.0815             nan     0.1000    0.0991
##      7        1.0207             nan     0.1000    0.0890
##      8        0.9650             nan     0.1000    0.0794
##      9        0.9159             nan     0.1000    0.0680
##     10        0.8737             nan     0.1000    0.0562
##     20        0.5595             nan     0.1000    0.0353
##     40        0.2873             nan     0.1000    0.0165
##     60        0.1570             nan     0.1000    0.0066
##     80        0.0944             nan     0.1000    0.0029
##    100        0.0615             nan     0.1000    0.0012
##    120        0.0421             nan     0.1000    0.0007
##    140        0.0297             nan     0.1000    0.0006
##    150        0.0254             nan     0.1000    0.0005
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1254
##      2        1.5232             nan     0.1000    0.0914
##      3        1.4645             nan     0.1000    0.0690
##      4        1.4199             nan     0.1000    0.0522
##      5        1.3855             nan     0.1000    0.0513
##      6        1.3522             nan     0.1000    0.0508
##      7        1.3202             nan     0.1000    0.0405
##      8        1.2938             nan     0.1000    0.0372
##      9        1.2690             nan     0.1000    0.0384
##     10        1.2423             nan     0.1000    0.0310
##     20        1.0588             nan     0.1000    0.0232
##     40        0.8411             nan     0.1000    0.0114
##     60        0.7018             nan     0.1000    0.0073
##     80        0.6025             nan     0.1000    0.0072
##    100        0.5216             nan     0.1000    0.0048
##    120        0.4576             nan     0.1000    0.0029
##    140        0.4066             nan     0.1000    0.0039
##    150        0.3843             nan     0.1000    0.0022
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1937
##      2        1.4827             nan     0.1000    0.1508
##      3        1.3852             nan     0.1000    0.1096
##      4        1.3135             nan     0.1000    0.1109
##      5        1.2446             nan     0.1000    0.0899
##      6        1.1886             nan     0.1000    0.0727
##      7        1.1428             nan     0.1000    0.0711
##      8        1.0982             nan     0.1000    0.0648
##      9        1.0574             nan     0.1000    0.0575
##     10        1.0209             nan     0.1000    0.0609
##     20        0.7540             nan     0.1000    0.0273
##     40        0.4693             nan     0.1000    0.0172
##     60        0.3135             nan     0.1000    0.0081
##     80        0.2155             nan     0.1000    0.0057
##    100        0.1535             nan     0.1000    0.0035
##    120        0.1129             nan     0.1000    0.0021
##    140        0.0858             nan     0.1000    0.0013
##    150        0.0760             nan     0.1000    0.0008
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.2504
##      2        1.4478             nan     0.1000    0.1935
##      3        1.3247             nan     0.1000    0.1416
##      4        1.2330             nan     0.1000    0.1221
##      5        1.1541             nan     0.1000    0.1124
##      6        1.0825             nan     0.1000    0.0875
##      7        1.0271             nan     0.1000    0.0777
##      8        0.9770             nan     0.1000    0.0789
##      9        0.9274             nan     0.1000    0.0767
##     10        0.8808             nan     0.1000    0.0595
##     20        0.5794             nan     0.1000    0.0337
##     40        0.2936             nan     0.1000    0.0181
##     60        0.1633             nan     0.1000    0.0067
##     80        0.0986             nan     0.1000    0.0037
##    100        0.0635             nan     0.1000    0.0025
##    120        0.0447             nan     0.1000    0.0009
##    140        0.0321             nan     0.1000    0.0009
##    150        0.0278             nan     0.1000    0.0004
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1276
##      2        1.5261             nan     0.1000    0.0843
##      3        1.4696             nan     0.1000    0.0683
##      4        1.4253             nan     0.1000    0.0526
##      5        1.3904             nan     0.1000    0.0545
##      6        1.3548             nan     0.1000    0.0481
##      7        1.3251             nan     0.1000    0.0353
##      8        1.3015             nan     0.1000    0.0483
##      9        1.2708             nan     0.1000    0.0354
##     10        1.2465             nan     0.1000    0.0350
##     20        1.0628             nan     0.1000    0.0212
##     40        0.8470             nan     0.1000    0.0126
##     60        0.7099             nan     0.1000    0.0087
##     80        0.6084             nan     0.1000    0.0063
##    100        0.5252             nan     0.1000    0.0047
##    120        0.4608             nan     0.1000    0.0035
##    140        0.4098             nan     0.1000    0.0036
##    150        0.3880             nan     0.1000    0.0019
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1923
##      2        1.4856             nan     0.1000    0.1331
##      3        1.3973             nan     0.1000    0.1211
##      4        1.3213             nan     0.1000    0.0983
##      5        1.2581             nan     0.1000    0.0872
##      6        1.2028             nan     0.1000    0.0805
##      7        1.1528             nan     0.1000    0.0648
##      8        1.1122             nan     0.1000    0.0670
##      9        1.0700             nan     0.1000    0.0566
##     10        1.0345             nan     0.1000    0.0538
##     20        0.7714             nan     0.1000    0.0372
##     40        0.4745             nan     0.1000    0.0212
##     60        0.3171             nan     0.1000    0.0073
##     80        0.2217             nan     0.1000    0.0051
##    100        0.1587             nan     0.1000    0.0044
##    120        0.1182             nan     0.1000    0.0038
##    140        0.0904             nan     0.1000    0.0017
##    150        0.0801             nan     0.1000    0.0014
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.2526
##      2        1.4506             nan     0.1000    0.1799
##      3        1.3360             nan     0.1000    0.1575
##      4        1.2386             nan     0.1000    0.1209
##      5        1.1613             nan     0.1000    0.1055
##      6        1.0945             nan     0.1000    0.0969
##      7        1.0331             nan     0.1000    0.0796
##      8        0.9815             nan     0.1000    0.0932
##      9        0.9259             nan     0.1000    0.0679
##     10        0.8849             nan     0.1000    0.0728
##     20        0.5771             nan     0.1000    0.0292
##     40        0.2942             nan     0.1000    0.0180
##     60        0.1649             nan     0.1000    0.0074
##     80        0.1018             nan     0.1000    0.0024
##    100        0.0693             nan     0.1000    0.0018
##    120        0.0488             nan     0.1000    0.0008
##    140        0.0359             nan     0.1000    0.0004
##    150        0.0310             nan     0.1000    0.0008
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1282
##      2        1.5230             nan     0.1000    0.0850
##      3        1.4654             nan     0.1000    0.0656
##      4        1.4214             nan     0.1000    0.0555
##      5        1.3850             nan     0.1000    0.0527
##      6        1.3508             nan     0.1000    0.0404
##      7        1.3242             nan     0.1000    0.0415
##      8        1.2975             nan     0.1000    0.0438
##      9        1.2687             nan     0.1000    0.0386
##     10        1.2428             nan     0.1000    0.0352
##     20        1.0595             nan     0.1000    0.0214
##     40        0.8414             nan     0.1000    0.0099
##     60        0.7028             nan     0.1000    0.0083
##     80        0.5997             nan     0.1000    0.0057
##    100        0.5204             nan     0.1000    0.0048
##    120        0.4547             nan     0.1000    0.0042
##    140        0.4042             nan     0.1000    0.0028
##    150        0.3822             nan     0.1000    0.0033
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1961
##      2        1.4833             nan     0.1000    0.1290
##      3        1.3968             nan     0.1000    0.1099
##      4        1.3258             nan     0.1000    0.0984
##      5        1.2633             nan     0.1000    0.0830
##      6        1.2103             nan     0.1000    0.0824
##      7        1.1573             nan     0.1000    0.0735
##      8        1.1109             nan     0.1000    0.0723
##      9        1.0650             nan     0.1000    0.0588
##     10        1.0284             nan     0.1000    0.0575
##     20        0.7599             nan     0.1000    0.0312
##     40        0.4593             nan     0.1000    0.0187
##     60        0.3002             nan     0.1000    0.0105
##     80        0.2120             nan     0.1000    0.0057
##    100        0.1522             nan     0.1000    0.0038
##    120        0.1121             nan     0.1000    0.0026
##    140        0.0848             nan     0.1000    0.0023
##    150        0.0742             nan     0.1000    0.0012
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.2508
##      2        1.4481             nan     0.1000    0.1760
##      3        1.3342             nan     0.1000    0.1468
##      4        1.2391             nan     0.1000    0.1412
##      5        1.1508             nan     0.1000    0.1080
##      6        1.0815             nan     0.1000    0.0857
##      7        1.0264             nan     0.1000    0.0801
##      8        0.9761             nan     0.1000    0.0765
##      9        0.9285             nan     0.1000    0.0770
##     10        0.8814             nan     0.1000    0.0610
##     20        0.5752             nan     0.1000    0.0392
##     40        0.2872             nan     0.1000    0.0131
##     60        0.1612             nan     0.1000    0.0067
##     80        0.0981             nan     0.1000    0.0030
##    100        0.0640             nan     0.1000    0.0009
##    120        0.0457             nan     0.1000    0.0011
##    140        0.0331             nan     0.1000    0.0008
##    150        0.0281             nan     0.1000    0.0010
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1253
##      2        1.5268             nan     0.1000    0.0871
##      3        1.4688             nan     0.1000    0.0655
##      4        1.4254             nan     0.1000    0.0572
##      5        1.3880             nan     0.1000    0.0466
##      6        1.3571             nan     0.1000    0.0504
##      7        1.3251             nan     0.1000    0.0450
##      8        1.2943             nan     0.1000    0.0422
##      9        1.2685             nan     0.1000    0.0374
##     10        1.2439             nan     0.1000    0.0354
##     20        1.0583             nan     0.1000    0.0201
##     40        0.8411             nan     0.1000    0.0141
##     60        0.7002             nan     0.1000    0.0098
##     80        0.5981             nan     0.1000    0.0061
##    100        0.5183             nan     0.1000    0.0035
##    120        0.4565             nan     0.1000    0.0055
##    140        0.4034             nan     0.1000    0.0047
##    150        0.3796             nan     0.1000    0.0022
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1951
##      2        1.4831             nan     0.1000    0.1348
##      3        1.3964             nan     0.1000    0.1261
##      4        1.3153             nan     0.1000    0.0985
##      5        1.2514             nan     0.1000    0.0831
##      6        1.1982             nan     0.1000    0.0916
##      7        1.1430             nan     0.1000    0.0666
##      8        1.1009             nan     0.1000    0.0692
##      9        1.0583             nan     0.1000    0.0660
##     10        1.0181             nan     0.1000    0.0584
##     20        0.7593             nan     0.1000    0.0267
##     40        0.4689             nan     0.1000    0.0157
##     60        0.3123             nan     0.1000    0.0106
##     80        0.2201             nan     0.1000    0.0061
##    100        0.1553             nan     0.1000    0.0035
##    120        0.1137             nan     0.1000    0.0021
##    140        0.0854             nan     0.1000    0.0016
##    150        0.0738             nan     0.1000    0.0018
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.2492
##      2        1.4500             nan     0.1000    0.1870
##      3        1.3296             nan     0.1000    0.1467
##      4        1.2362             nan     0.1000    0.1344
##      5        1.1531             nan     0.1000    0.1078
##      6        1.0845             nan     0.1000    0.1009
##      7        1.0224             nan     0.1000    0.0930
##      8        0.9656             nan     0.1000    0.0691
##      9        0.9216             nan     0.1000    0.0661
##     10        0.8805             nan     0.1000    0.0621
##     20        0.5793             nan     0.1000    0.0269
##     40        0.2970             nan     0.1000    0.0159
##     60        0.1639             nan     0.1000    0.0076
##     80        0.0994             nan     0.1000    0.0042
##    100        0.0649             nan     0.1000    0.0015
##    120        0.0446             nan     0.1000    0.0009
##    140        0.0326             nan     0.1000    0.0007
##    150        0.0279             nan     0.1000    0.0006
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1266
##      2        1.5241             nan     0.1000    0.0906
##      3        1.4641             nan     0.1000    0.0689
##      4        1.4190             nan     0.1000    0.0543
##      5        1.3837             nan     0.1000    0.0502
##      6        1.3502             nan     0.1000    0.0450
##      7        1.3214             nan     0.1000    0.0428
##      8        1.2944             nan     0.1000    0.0420
##      9        1.2676             nan     0.1000    0.0401
##     10        1.2411             nan     0.1000    0.0330
##     20        1.0593             nan     0.1000    0.0202
##     40        0.8408             nan     0.1000    0.0104
##     60        0.7024             nan     0.1000    0.0102
##     80        0.6007             nan     0.1000    0.0046
##    100        0.5189             nan     0.1000    0.0062
##    120        0.4567             nan     0.1000    0.0043
##    140        0.4035             nan     0.1000    0.0030
##    150        0.3816             nan     0.1000    0.0022
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1991
##      2        1.4820             nan     0.1000    0.1414
##      3        1.3914             nan     0.1000    0.1119
##      4        1.3187             nan     0.1000    0.0941
##      5        1.2578             nan     0.1000    0.0804
##      6        1.2054             nan     0.1000    0.0851
##      7        1.1530             nan     0.1000    0.0760
##      8        1.1055             nan     0.1000    0.0649
##      9        1.0653             nan     0.1000    0.0628
##     10        1.0263             nan     0.1000    0.0528
##     20        0.7684             nan     0.1000    0.0355
##     40        0.4686             nan     0.1000    0.0170
##     60        0.3101             nan     0.1000    0.0089
##     80        0.2142             nan     0.1000    0.0049
##    100        0.1578             nan     0.1000    0.0044
##    120        0.1159             nan     0.1000    0.0016
##    140        0.0877             nan     0.1000    0.0019
##    150        0.0774             nan     0.1000    0.0016
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.2575
##      2        1.4483             nan     0.1000    0.1832
##      3        1.3325             nan     0.1000    0.1542
##      4        1.2332             nan     0.1000    0.1225
##      5        1.1555             nan     0.1000    0.0957
##      6        1.0936             nan     0.1000    0.0982
##      7        1.0304             nan     0.1000    0.0941
##      8        0.9716             nan     0.1000    0.0874
##      9        0.9170             nan     0.1000    0.0796
##     10        0.8688             nan     0.1000    0.0643
##     20        0.5672             nan     0.1000    0.0353
##     40        0.2911             nan     0.1000    0.0159
##     60        0.1602             nan     0.1000    0.0082
##     80        0.0966             nan     0.1000    0.0035
##    100        0.0622             nan     0.1000    0.0016
##    120        0.0434             nan     0.1000    0.0008
##    140        0.0313             nan     0.1000    0.0005
##    150        0.0269             nan     0.1000    0.0005
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1245
##      2        1.5224             nan     0.1000    0.0923
##      3        1.4627             nan     0.1000    0.0704
##      4        1.4167             nan     0.1000    0.0548
##      5        1.3806             nan     0.1000    0.0509
##      6        1.3462             nan     0.1000    0.0433
##      7        1.3183             nan     0.1000    0.0416
##      8        1.2911             nan     0.1000    0.0446
##      9        1.2619             nan     0.1000    0.0365
##     10        1.2387             nan     0.1000    0.0347
##     20        1.0579             nan     0.1000    0.0216
##     40        0.8401             nan     0.1000    0.0115
##     60        0.7025             nan     0.1000    0.0097
##     80        0.5975             nan     0.1000    0.0066
##    100        0.5175             nan     0.1000    0.0042
##    120        0.4540             nan     0.1000    0.0049
##    140        0.4032             nan     0.1000    0.0031
##    150        0.3812             nan     0.1000    0.0026
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1919
##      2        1.4829             nan     0.1000    0.1379
##      3        1.3938             nan     0.1000    0.1108
##      4        1.3201             nan     0.1000    0.0892
##      5        1.2612             nan     0.1000    0.0983
##      6        1.1990             nan     0.1000    0.0846
##      7        1.1449             nan     0.1000    0.0745
##      8        1.0987             nan     0.1000    0.0711
##      9        1.0554             nan     0.1000    0.0517
##     10        1.0228             nan     0.1000    0.0594
##     20        0.7562             nan     0.1000    0.0318
##     40        0.4655             nan     0.1000    0.0112
##     60        0.3124             nan     0.1000    0.0102
##     80        0.2165             nan     0.1000    0.0055
##    100        0.1553             nan     0.1000    0.0029
##    120        0.1139             nan     0.1000    0.0026
##    140        0.0868             nan     0.1000    0.0021
##    150        0.0764             nan     0.1000    0.0025
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.2493
##      2        1.4486             nan     0.1000    0.1738
##      3        1.3362             nan     0.1000    0.1452
##      4        1.2432             nan     0.1000    0.1305
##      5        1.1616             nan     0.1000    0.0975
##      6        1.0993             nan     0.1000    0.1062
##      7        1.0340             nan     0.1000    0.0820
##      8        0.9816             nan     0.1000    0.0863
##      9        0.9295             nan     0.1000    0.0684
##     10        0.8869             nan     0.1000    0.0632
##     20        0.5762             nan     0.1000    0.0359
##     40        0.2857             nan     0.1000    0.0130
##     60        0.1599             nan     0.1000    0.0062
##     80        0.0975             nan     0.1000    0.0037
##    100        0.0661             nan     0.1000    0.0017
##    120        0.0462             nan     0.1000    0.0011
##    140        0.0342             nan     0.1000    0.0007
##    150        0.0296             nan     0.1000    0.0004
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1320
##      2        1.5216             nan     0.1000    0.0883
##      3        1.4623             nan     0.1000    0.0679
##      4        1.4178             nan     0.1000    0.0568
##      5        1.3808             nan     0.1000    0.0525
##      6        1.3473             nan     0.1000    0.0451
##      7        1.3184             nan     0.1000    0.0414
##      8        1.2917             nan     0.1000    0.0405
##      9        1.2644             nan     0.1000    0.0353
##     10        1.2416             nan     0.1000    0.0373
##     20        1.0567             nan     0.1000    0.0238
##     40        0.8395             nan     0.1000    0.0113
##     60        0.6985             nan     0.1000    0.0092
##     80        0.5976             nan     0.1000    0.0061
##    100        0.5170             nan     0.1000    0.0048
##    120        0.4520             nan     0.1000    0.0040
##    140        0.3981             nan     0.1000    0.0036
##    150        0.3755             nan     0.1000    0.0034
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1981
##      2        1.4793             nan     0.1000    0.1368
##      3        1.3914             nan     0.1000    0.1140
##      4        1.3190             nan     0.1000    0.0904
##      5        1.2582             nan     0.1000    0.0896
##      6        1.2038             nan     0.1000    0.0860
##      7        1.1490             nan     0.1000    0.0764
##      8        1.1013             nan     0.1000    0.0716
##      9        1.0573             nan     0.1000    0.0481
##     10        1.0251             nan     0.1000    0.0704
##     20        0.7599             nan     0.1000    0.0372
##     40        0.4661             nan     0.1000    0.0138
##     60        0.3097             nan     0.1000    0.0126
##     80        0.2149             nan     0.1000    0.0052
##    100        0.1549             nan     0.1000    0.0050
##    120        0.1135             nan     0.1000    0.0033
##    140        0.0849             nan     0.1000    0.0024
##    150        0.0729             nan     0.1000    0.0006
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.2532
##      2        1.4464             nan     0.1000    0.1830
##      3        1.3282             nan     0.1000    0.1512
##      4        1.2295             nan     0.1000    0.1178
##      5        1.1523             nan     0.1000    0.1098
##      6        1.0826             nan     0.1000    0.0889
##      7        1.0257             nan     0.1000    0.0914
##      8        0.9691             nan     0.1000    0.0826
##      9        0.9173             nan     0.1000    0.0649
##     10        0.8754             nan     0.1000    0.0624
##     20        0.5740             nan     0.1000    0.0378
##     40        0.2911             nan     0.1000    0.0118
##     60        0.1627             nan     0.1000    0.0071
##     80        0.0999             nan     0.1000    0.0036
##    100        0.0662             nan     0.1000    0.0020
##    120        0.0455             nan     0.1000    0.0011
##    140        0.0327             nan     0.1000    0.0006
##    150        0.0274             nan     0.1000    0.0004
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1253
##      2        1.5238             nan     0.1000    0.0889
##      3        1.4648             nan     0.1000    0.0686
##      4        1.4195             nan     0.1000    0.0567
##      5        1.3817             nan     0.1000    0.0471
##      6        1.3510             nan     0.1000    0.0466
##      7        1.3205             nan     0.1000    0.0422
##      8        1.2932             nan     0.1000    0.0368
##      9        1.2675             nan     0.1000    0.0415
##     10        1.2395             nan     0.1000    0.0367
##     20        1.0495             nan     0.1000    0.0205
##     40        0.8338             nan     0.1000    0.0130
##     60        0.6940             nan     0.1000    0.0097
##     80        0.5871             nan     0.1000    0.0068
##    100        0.5088             nan     0.1000    0.0052
##    120        0.4468             nan     0.1000    0.0029
##    140        0.3975             nan     0.1000    0.0026
##    150        0.3760             nan     0.1000    0.0032
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1846
##      2        1.4871             nan     0.1000    0.1510
##      3        1.3904             nan     0.1000    0.1127
##      4        1.3175             nan     0.1000    0.1007
##      5        1.2557             nan     0.1000    0.1067
##      6        1.1898             nan     0.1000    0.0831
##      7        1.1375             nan     0.1000    0.0647
##      8        1.0960             nan     0.1000    0.0649
##      9        1.0554             nan     0.1000    0.0597
##     10        1.0175             nan     0.1000    0.0543
##     20        0.7445             nan     0.1000    0.0333
##     40        0.4550             nan     0.1000    0.0141
##     60        0.3035             nan     0.1000    0.0086
##     80        0.2149             nan     0.1000    0.0082
##    100        0.1538             nan     0.1000    0.0038
##    120        0.1137             nan     0.1000    0.0027
##    140        0.0858             nan     0.1000    0.0014
##    150        0.0745             nan     0.1000    0.0015
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.2454
##      2        1.4498             nan     0.1000    0.1978
##      3        1.3270             nan     0.1000    0.1564
##      4        1.2283             nan     0.1000    0.1330
##      5        1.1461             nan     0.1000    0.1068
##      6        1.0790             nan     0.1000    0.0942
##      7        1.0192             nan     0.1000    0.0986
##      8        0.9581             nan     0.1000    0.0736
##      9        0.9121             nan     0.1000    0.0653
##     10        0.8710             nan     0.1000    0.0658
##     20        0.5724             nan     0.1000    0.0471
##     40        0.2868             nan     0.1000    0.0142
##     60        0.1610             nan     0.1000    0.0068
##     80        0.0971             nan     0.1000    0.0038
##    100        0.0659             nan     0.1000    0.0020
##    120        0.0463             nan     0.1000    0.0012
##    140        0.0338             nan     0.1000    0.0010
##    150        0.0290             nan     0.1000    0.0009
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1303
##      2        1.5245             nan     0.1000    0.0870
##      3        1.4680             nan     0.1000    0.0681
##      4        1.4239             nan     0.1000    0.0528
##      5        1.3886             nan     0.1000    0.0475
##      6        1.3568             nan     0.1000    0.0490
##      7        1.3258             nan     0.1000    0.0425
##      8        1.2994             nan     0.1000    0.0423
##      9        1.2712             nan     0.1000    0.0359
##     10        1.2475             nan     0.1000    0.0338
##     20        1.0632             nan     0.1000    0.0210
##     40        0.8457             nan     0.1000    0.0133
##     60        0.7075             nan     0.1000    0.0107
##     80        0.6037             nan     0.1000    0.0062
##    100        0.5225             nan     0.1000    0.0058
##    120        0.4565             nan     0.1000    0.0035
##    140        0.4041             nan     0.1000    0.0029
##    150        0.3816             nan     0.1000    0.0033
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1915
##      2        1.4868             nan     0.1000    0.1373
##      3        1.3981             nan     0.1000    0.1107
##      4        1.3259             nan     0.1000    0.0993
##      5        1.2621             nan     0.1000    0.0927
##      6        1.2038             nan     0.1000    0.0778
##      7        1.1546             nan     0.1000    0.0818
##      8        1.1047             nan     0.1000    0.0666
##      9        1.0630             nan     0.1000    0.0649
##     10        1.0236             nan     0.1000    0.0580
##     20        0.7659             nan     0.1000    0.0354
##     40        0.4688             nan     0.1000    0.0147
##     60        0.3086             nan     0.1000    0.0105
##     80        0.2127             nan     0.1000    0.0059
##    100        0.1531             nan     0.1000    0.0025
##    120        0.1145             nan     0.1000    0.0024
##    140        0.0860             nan     0.1000    0.0015
##    150        0.0763             nan     0.1000    0.0010
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.2415
##      2        1.4543             nan     0.1000    0.1847
##      3        1.3341             nan     0.1000    0.1452
##      4        1.2410             nan     0.1000    0.1280
##      5        1.1593             nan     0.1000    0.0925
##      6        1.0993             nan     0.1000    0.1217
##      7        1.0262             nan     0.1000    0.0873
##      8        0.9714             nan     0.1000    0.0859
##      9        0.9184             nan     0.1000    0.0686
##     10        0.8761             nan     0.1000    0.0651
##     20        0.5813             nan     0.1000    0.0361
##     40        0.2921             nan     0.1000    0.0194
##     60        0.1631             nan     0.1000    0.0058
##     80        0.0990             nan     0.1000    0.0044
##    100        0.0647             nan     0.1000    0.0023
##    120        0.0455             nan     0.1000    0.0008
##    140        0.0326             nan     0.1000    0.0006
##    150        0.0280             nan     0.1000    0.0005
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1222
##      2        1.5246             nan     0.1000    0.0873
##      3        1.4675             nan     0.1000    0.0671
##      4        1.4227             nan     0.1000    0.0549
##      5        1.3856             nan     0.1000    0.0513
##      6        1.3525             nan     0.1000    0.0479
##      7        1.3222             nan     0.1000    0.0467
##      8        1.2940             nan     0.1000    0.0359
##      9        1.2707             nan     0.1000    0.0373
##     10        1.2453             nan     0.1000    0.0377
##     20        1.0607             nan     0.1000    0.0200
##     40        0.8436             nan     0.1000    0.0112
##     60        0.7068             nan     0.1000    0.0089
##     80        0.6034             nan     0.1000    0.0050
##    100        0.5210             nan     0.1000    0.0052
##    120        0.4584             nan     0.1000    0.0035
##    140        0.4089             nan     0.1000    0.0036
##    150        0.3841             nan     0.1000    0.0030
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1961
##      2        1.4839             nan     0.1000    0.1350
##      3        1.3955             nan     0.1000    0.1149
##      4        1.3230             nan     0.1000    0.0900
##      5        1.2643             nan     0.1000    0.0939
##      6        1.2041             nan     0.1000    0.0912
##      7        1.1466             nan     0.1000    0.0682
##      8        1.1027             nan     0.1000    0.0631
##      9        1.0636             nan     0.1000    0.0657
##     10        1.0234             nan     0.1000    0.0513
##     20        0.7615             nan     0.1000    0.0324
##     40        0.4745             nan     0.1000    0.0170
##     60        0.3160             nan     0.1000    0.0073
##     80        0.2194             nan     0.1000    0.0052
##    100        0.1569             nan     0.1000    0.0034
##    120        0.1173             nan     0.1000    0.0023
##    140        0.0897             nan     0.1000    0.0018
##    150        0.0782             nan     0.1000    0.0011
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.2447
##      2        1.4517             nan     0.1000    0.1828
##      3        1.3356             nan     0.1000    0.1336
##      4        1.2480             nan     0.1000    0.1388
##      5        1.1614             nan     0.1000    0.1046
##      6        1.0959             nan     0.1000    0.0990
##      7        1.0337             nan     0.1000    0.0855
##      8        0.9797             nan     0.1000    0.0890
##      9        0.9262             nan     0.1000    0.0782
##     10        0.8773             nan     0.1000    0.0587
##     20        0.5745             nan     0.1000    0.0346
##     40        0.2936             nan     0.1000    0.0129
##     60        0.1660             nan     0.1000    0.0076
##     80        0.1042             nan     0.1000    0.0031
##    100        0.0699             nan     0.1000    0.0024
##    120        0.0489             nan     0.1000    0.0010
##    140        0.0344             nan     0.1000    0.0002
##    150        0.0297             nan     0.1000    0.0007
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1264
##      2        1.5219             nan     0.1000    0.0891
##      3        1.4626             nan     0.1000    0.0705
##      4        1.4157             nan     0.1000    0.0536
##      5        1.3802             nan     0.1000    0.0561
##      6        1.3440             nan     0.1000    0.0471
##      7        1.3136             nan     0.1000    0.0360
##      8        1.2892             nan     0.1000    0.0386
##      9        1.2637             nan     0.1000    0.0351
##     10        1.2407             nan     0.1000    0.0363
##     20        1.0559             nan     0.1000    0.0212
##     40        0.8381             nan     0.1000    0.0121
##     60        0.7019             nan     0.1000    0.0070
##     80        0.6026             nan     0.1000    0.0072
##    100        0.5201             nan     0.1000    0.0050
##    120        0.4594             nan     0.1000    0.0034
##    140        0.4093             nan     0.1000    0.0024
##    150        0.3881             nan     0.1000    0.0017
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1955
##      2        1.4829             nan     0.1000    0.1390
##      3        1.3920             nan     0.1000    0.1102
##      4        1.3202             nan     0.1000    0.0946
##      5        1.2592             nan     0.1000    0.0813
##      6        1.2066             nan     0.1000    0.0866
##      7        1.1537             nan     0.1000    0.0875
##      8        1.0999             nan     0.1000    0.0601
##      9        1.0617             nan     0.1000    0.0562
##     10        1.0252             nan     0.1000    0.0589
##     20        0.7620             nan     0.1000    0.0340
##     40        0.4692             nan     0.1000    0.0192
##     60        0.3106             nan     0.1000    0.0102
##     80        0.2171             nan     0.1000    0.0058
##    100        0.1538             nan     0.1000    0.0026
##    120        0.1138             nan     0.1000    0.0021
##    140        0.0858             nan     0.1000    0.0015
##    150        0.0755             nan     0.1000    0.0014
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.2505
##      2        1.4463             nan     0.1000    0.1845
##      3        1.3289             nan     0.1000    0.1477
##      4        1.2345             nan     0.1000    0.1373
##      5        1.1494             nan     0.1000    0.1037
##      6        1.0822             nan     0.1000    0.0952
##      7        1.0227             nan     0.1000    0.0820
##      8        0.9708             nan     0.1000    0.0842
##      9        0.9195             nan     0.1000    0.0663
##     10        0.8768             nan     0.1000    0.0645
##     20        0.5687             nan     0.1000    0.0345
##     40        0.2915             nan     0.1000    0.0159
##     60        0.1635             nan     0.1000    0.0077
##     80        0.1006             nan     0.1000    0.0040
##    100        0.0658             nan     0.1000    0.0019
##    120        0.0466             nan     0.1000    0.0008
##    140        0.0338             nan     0.1000    0.0011
##    150        0.0289             nan     0.1000    0.0007
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.2541
##      2        1.4488             nan     0.1000    0.1904
##      3        1.3271             nan     0.1000    0.1390
##      4        1.2382             nan     0.1000    0.1240
##      5        1.1602             nan     0.1000    0.1036
##      6        1.0946             nan     0.1000    0.1003
##      7        1.0316             nan     0.1000    0.0806
##      8        0.9798             nan     0.1000    0.0752
##      9        0.9323             nan     0.1000    0.0807
##     10        0.8837             nan     0.1000    0.0750
##     20        0.5766             nan     0.1000    0.0277
##     40        0.2960             nan     0.1000    0.0163
##     60        0.1652             nan     0.1000    0.0065
##     80        0.1022             nan     0.1000    0.0036
##    100        0.0688             nan     0.1000    0.0017
##    120        0.0476             nan     0.1000    0.0015
##    140        0.0345             nan     0.1000    0.0008
##    150        0.0296             nan     0.1000    0.0004
```

```r
modelLGA <- train(classe ~ ., data=training, method="lda")
```

```
## Loading required package: MASS
```

```
## Warning in lda.default(x, grouping, ...): variables are collinear

## Warning in lda.default(x, grouping, ...): variables are collinear

## Warning in lda.default(x, grouping, ...): variables are collinear

## Warning in lda.default(x, grouping, ...): variables are collinear

## Warning in lda.default(x, grouping, ...): variables are collinear

## Warning in lda.default(x, grouping, ...): variables are collinear

## Warning in lda.default(x, grouping, ...): variables are collinear

## Warning in lda.default(x, grouping, ...): variables are collinear

## Warning in lda.default(x, grouping, ...): variables are collinear

## Warning in lda.default(x, grouping, ...): variables are collinear

## Warning in lda.default(x, grouping, ...): variables are collinear

## Warning in lda.default(x, grouping, ...): variables are collinear

## Warning in lda.default(x, grouping, ...): variables are collinear

## Warning in lda.default(x, grouping, ...): variables are collinear

## Warning in lda.default(x, grouping, ...): variables are collinear

## Warning in lda.default(x, grouping, ...): variables are collinear

## Warning in lda.default(x, grouping, ...): variables are collinear

## Warning in lda.default(x, grouping, ...): variables are collinear

## Warning in lda.default(x, grouping, ...): variables are collinear

## Warning in lda.default(x, grouping, ...): variables are collinear

## Warning in lda.default(x, grouping, ...): variables are collinear

## Warning in lda.default(x, grouping, ...): variables are collinear

## Warning in lda.default(x, grouping, ...): variables are collinear

## Warning in lda.default(x, grouping, ...): variables are collinear

## Warning in lda.default(x, grouping, ...): variables are collinear

## Warning in lda.default(x, grouping, ...): variables are collinear
```

Predict using the validation data set and show the confusion matrix.


```r
predictionGBM <- predict(modelGBM, validation)
confusionMatrix(predictionGBM, validation$classe)
```

```
## Confusion Matrix and Statistics
## 
##           Reference
## Prediction    A    B    C    D    E
##          A 2230    4    0    0    0
##          B    2 1512    1    0    0
##          C    0    2 1362    9    0
##          D    0    0    5 1268    0
##          E    0    0    0    9 1442
## 
## Overall Statistics
##                                           
##                Accuracy : 0.9959          
##                  95% CI : (0.9942, 0.9972)
##     No Information Rate : 0.2845          
##     P-Value [Acc > NIR] : < 2.2e-16       
##                                           
##                   Kappa : 0.9948          
##  Mcnemar's Test P-Value : NA              
## 
## Statistics by Class:
## 
##                      Class: A Class: B Class: C Class: D Class: E
## Sensitivity            0.9991   0.9960   0.9956   0.9860   1.0000
## Specificity            0.9993   0.9995   0.9983   0.9992   0.9986
## Pos Pred Value         0.9982   0.9980   0.9920   0.9961   0.9938
## Neg Pred Value         0.9996   0.9991   0.9991   0.9973   1.0000
## Prevalence             0.2845   0.1935   0.1744   0.1639   0.1838
## Detection Rate         0.2842   0.1927   0.1736   0.1616   0.1838
## Detection Prevalence   0.2847   0.1931   0.1750   0.1622   0.1849
## Balanced Accuracy      0.9992   0.9978   0.9970   0.9926   0.9993
```

```r
predictionLGA <- predict(modelLGA, validation)
confusionMatrix(predictionLGA, validation$classe)
```

```
## Confusion Matrix and Statistics
## 
##           Reference
## Prediction    A    B    C    D    E
##          A 2025  162    3    0    0
##          B  183 1112  120    1    0
##          C   24  234 1212  135    3
##          D    0   10   31 1076  135
##          E    0    0    2   74 1304
## 
## Overall Statistics
##                                           
##                Accuracy : 0.8576          
##                  95% CI : (0.8497, 0.8653)
##     No Information Rate : 0.2845          
##     P-Value [Acc > NIR] : < 2.2e-16       
##                                           
##                   Kappa : 0.8201          
##  Mcnemar's Test P-Value : NA              
## 
## Statistics by Class:
## 
##                      Class: A Class: B Class: C Class: D Class: E
## Sensitivity            0.9073   0.7325   0.8860   0.8367   0.9043
## Specificity            0.9706   0.9520   0.9389   0.9732   0.9881
## Pos Pred Value         0.9247   0.7853   0.7537   0.8594   0.9449
## Neg Pred Value         0.9634   0.9369   0.9750   0.9682   0.9787
## Prevalence             0.2845   0.1935   0.1744   0.1639   0.1838
## Detection Rate         0.2581   0.1417   0.1545   0.1371   0.1662
## Detection Prevalence   0.2791   0.1805   0.2049   0.1596   0.1759
## Balanced Accuracy      0.9389   0.8423   0.9124   0.9049   0.9462
```

In our case the GBM performs much better than the LGA. 

For the sake of curiosity let's define a combined model (random forrest `rf`) and verify


```r
trainginGBMLGA <- data.frame(predictionGBM, predictionLGA, classe=validation$classe)
modelGBMLGA <- train(classe ~ ., method="rf", data=trainginGBMLGA)
```

```
## Loading required package: randomForest
```

```
## randomForest 4.6-10
```

```
## Type rfNews() to see new features/changes/bug fixes.
```

```r
predictionGMALGA <- predict(modelGBMLGA, trainginGBMLGA)
confusionMatrix(predictionGMALGA, trainginGBMLGA$classe)
```

```
## Confusion Matrix and Statistics
## 
##           Reference
## Prediction    A    B    C    D    E
##          A 2230    4    0    0    0
##          B    2 1512    1    0    0
##          C    0    2 1362    9    0
##          D    0    0    5 1268    0
##          E    0    0    0    9 1442
## 
## Overall Statistics
##                                           
##                Accuracy : 0.9959          
##                  95% CI : (0.9942, 0.9972)
##     No Information Rate : 0.2845          
##     P-Value [Acc > NIR] : < 2.2e-16       
##                                           
##                   Kappa : 0.9948          
##  Mcnemar's Test P-Value : NA              
## 
## Statistics by Class:
## 
##                      Class: A Class: B Class: C Class: D Class: E
## Sensitivity            0.9991   0.9960   0.9956   0.9860   1.0000
## Specificity            0.9993   0.9995   0.9983   0.9992   0.9986
## Pos Pred Value         0.9982   0.9980   0.9920   0.9961   0.9938
## Neg Pred Value         0.9996   0.9991   0.9991   0.9973   1.0000
## Prevalence             0.2845   0.1935   0.1744   0.1639   0.1838
## Detection Rate         0.2842   0.1927   0.1736   0.1616   0.1838
## Detection Prevalence   0.2847   0.1931   0.1750   0.1622   0.1849
## Balanced Accuracy      0.9992   0.9978   0.9970   0.9926   0.9993
```

The accuracy of the combined model is only slightly higher.

# Evaluate the final model (GBM) on the testing dataset


```r
resultGBM  <- predict(modelGBM, testing)
resultGBM
```

```
##  [1] B A B A A E D B A A B C B A E E A B B B
## Levels: A B C D E
```

