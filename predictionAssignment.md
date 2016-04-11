# Coursera: Practical Machine Learning - Prediction Assignment (https://www.coursera.org/learn/practical-machine-learning)
Andreas Hoelzl [GitHub](https://github.com/andywoodly)  

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
inTrain <- createDataPartition(y=training$classe, p=0.3, list=FALSE)
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
##      1        1.6094             nan     0.1000    0.1370
##      2        1.5187             nan     0.1000    0.0886
##      3        1.4570             nan     0.1000    0.0671
##      4        1.4102             nan     0.1000    0.0550
##      5        1.3730             nan     0.1000    0.0490
##      6        1.3403             nan     0.1000    0.0474
##      7        1.3084             nan     0.1000    0.0415
##      8        1.2816             nan     0.1000    0.0324
##      9        1.2581             nan     0.1000    0.0406
##     10        1.2294             nan     0.1000    0.0409
##     20        1.0398             nan     0.1000    0.0221
##     40        0.8209             nan     0.1000    0.0116
##     60        0.6868             nan     0.1000    0.0083
##     80        0.5836             nan     0.1000    0.0052
##    100        0.5040             nan     0.1000    0.0045
##    120        0.4436             nan     0.1000    0.0032
##    140        0.3925             nan     0.1000    0.0023
##    150        0.3710             nan     0.1000    0.0028
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.2092
##      2        1.4723             nan     0.1000    0.1329
##      3        1.3809             nan     0.1000    0.1147
##      4        1.3052             nan     0.1000    0.1021
##      5        1.2404             nan     0.1000    0.0852
##      6        1.1839             nan     0.1000    0.0766
##      7        1.1336             nan     0.1000    0.0707
##      8        1.0879             nan     0.1000    0.0783
##      9        1.0388             nan     0.1000    0.0562
##     10        1.0030             nan     0.1000    0.0502
##     20        0.7390             nan     0.1000    0.0352
##     40        0.4652             nan     0.1000    0.0170
##     60        0.3072             nan     0.1000    0.0085
##     80        0.2171             nan     0.1000    0.0047
##    100        0.1593             nan     0.1000    0.0045
##    120        0.1210             nan     0.1000    0.0027
##    140        0.0897             nan     0.1000    0.0019
##    150        0.0792             nan     0.1000    0.0012
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.2677
##      2        1.4396             nan     0.1000    0.1804
##      3        1.3242             nan     0.1000    0.1455
##      4        1.2308             nan     0.1000    0.1242
##      5        1.1483             nan     0.1000    0.1021
##      6        1.0808             nan     0.1000    0.0784
##      7        1.0290             nan     0.1000    0.0783
##      8        0.9772             nan     0.1000    0.0774
##      9        0.9247             nan     0.1000    0.0800
##     10        0.8749             nan     0.1000    0.0713
##     20        0.5689             nan     0.1000    0.0352
##     40        0.2931             nan     0.1000    0.0121
##     60        0.1692             nan     0.1000    0.0087
##     80        0.1055             nan     0.1000    0.0034
##    100        0.0710             nan     0.1000    0.0023
##    120        0.0485             nan     0.1000    0.0011
##    140        0.0348             nan     0.1000    0.0004
##    150        0.0299             nan     0.1000    0.0006
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1280
##      2        1.5194             nan     0.1000    0.0941
##      3        1.4580             nan     0.1000    0.0647
##      4        1.4137             nan     0.1000    0.0605
##      5        1.3755             nan     0.1000    0.0592
##      6        1.3371             nan     0.1000    0.0470
##      7        1.3071             nan     0.1000    0.0448
##      8        1.2784             nan     0.1000    0.0385
##      9        1.2539             nan     0.1000    0.0336
##     10        1.2291             nan     0.1000    0.0326
##     20        1.0447             nan     0.1000    0.0201
##     40        0.8290             nan     0.1000    0.0130
##     60        0.6941             nan     0.1000    0.0093
##     80        0.5923             nan     0.1000    0.0043
##    100        0.5139             nan     0.1000    0.0042
##    120        0.4493             nan     0.1000    0.0022
##    140        0.4016             nan     0.1000    0.0018
##    150        0.3782             nan     0.1000    0.0021
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.2105
##      2        1.4791             nan     0.1000    0.1415
##      3        1.3868             nan     0.1000    0.1104
##      4        1.3126             nan     0.1000    0.0919
##      5        1.2507             nan     0.1000    0.0874
##      6        1.1943             nan     0.1000    0.0907
##      7        1.1377             nan     0.1000    0.0703
##      8        1.0939             nan     0.1000    0.0583
##      9        1.0550             nan     0.1000    0.0618
##     10        1.0167             nan     0.1000    0.0426
##     20        0.7534             nan     0.1000    0.0314
##     40        0.4756             nan     0.1000    0.0154
##     60        0.3243             nan     0.1000    0.0084
##     80        0.2257             nan     0.1000    0.0064
##    100        0.1640             nan     0.1000    0.0043
##    120        0.1209             nan     0.1000    0.0029
##    140        0.0913             nan     0.1000    0.0016
##    150        0.0809             nan     0.1000    0.0012
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.2542
##      2        1.4443             nan     0.1000    0.1836
##      3        1.3256             nan     0.1000    0.1495
##      4        1.2295             nan     0.1000    0.1171
##      5        1.1516             nan     0.1000    0.1063
##      6        1.0815             nan     0.1000    0.0957
##      7        1.0218             nan     0.1000    0.0868
##      8        0.9669             nan     0.1000    0.0885
##      9        0.9109             nan     0.1000    0.0689
##     10        0.8683             nan     0.1000    0.0596
##     20        0.5647             nan     0.1000    0.0261
##     40        0.2955             nan     0.1000    0.0147
##     60        0.1751             nan     0.1000    0.0075
##     80        0.1097             nan     0.1000    0.0043
##    100        0.0714             nan     0.1000    0.0011
##    120        0.0498             nan     0.1000    0.0012
##    140        0.0354             nan     0.1000    0.0003
##    150        0.0302             nan     0.1000    0.0006
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1374
##      2        1.5183             nan     0.1000    0.0881
##      3        1.4572             nan     0.1000    0.0719
##      4        1.4099             nan     0.1000    0.0544
##      5        1.3727             nan     0.1000    0.0541
##      6        1.3378             nan     0.1000    0.0416
##      7        1.3094             nan     0.1000    0.0479
##      8        1.2793             nan     0.1000    0.0411
##      9        1.2532             nan     0.1000    0.0381
##     10        1.2265             nan     0.1000    0.0311
##     20        1.0395             nan     0.1000    0.0214
##     40        0.8201             nan     0.1000    0.0100
##     60        0.6864             nan     0.1000    0.0077
##     80        0.5806             nan     0.1000    0.0058
##    100        0.5044             nan     0.1000    0.0025
##    120        0.4433             nan     0.1000    0.0029
##    140        0.3914             nan     0.1000    0.0025
##    150        0.3681             nan     0.1000    0.0037
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.2006
##      2        1.4790             nan     0.1000    0.1446
##      3        1.3829             nan     0.1000    0.1171
##      4        1.3085             nan     0.1000    0.0921
##      5        1.2477             nan     0.1000    0.0894
##      6        1.1882             nan     0.1000    0.0784
##      7        1.1378             nan     0.1000    0.0755
##      8        1.0912             nan     0.1000    0.0751
##      9        1.0447             nan     0.1000    0.0567
##     10        1.0071             nan     0.1000    0.0507
##     20        0.7482             nan     0.1000    0.0300
##     40        0.4635             nan     0.1000    0.0126
##     60        0.3110             nan     0.1000    0.0097
##     80        0.2189             nan     0.1000    0.0044
##    100        0.1585             nan     0.1000    0.0037
##    120        0.1185             nan     0.1000    0.0019
##    140        0.0912             nan     0.1000    0.0018
##    150        0.0802             nan     0.1000    0.0016
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.2577
##      2        1.4446             nan     0.1000    0.1754
##      3        1.3311             nan     0.1000    0.1478
##      4        1.2358             nan     0.1000    0.1276
##      5        1.1552             nan     0.1000    0.1109
##      6        1.0850             nan     0.1000    0.0912
##      7        1.0268             nan     0.1000    0.0874
##      8        0.9711             nan     0.1000    0.0790
##      9        0.9198             nan     0.1000    0.0657
##     10        0.8770             nan     0.1000    0.0594
##     20        0.5682             nan     0.1000    0.0427
##     40        0.2946             nan     0.1000    0.0098
##     60        0.1691             nan     0.1000    0.0075
##     80        0.1061             nan     0.1000    0.0039
##    100        0.0715             nan     0.1000    0.0025
##    120        0.0500             nan     0.1000    0.0009
##    140        0.0353             nan     0.1000    0.0008
##    150        0.0301             nan     0.1000    0.0007
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1347
##      2        1.5193             nan     0.1000    0.0865
##      3        1.4591             nan     0.1000    0.0678
##      4        1.4123             nan     0.1000    0.0622
##      5        1.3715             nan     0.1000    0.0479
##      6        1.3384             nan     0.1000    0.0435
##      7        1.3096             nan     0.1000    0.0441
##      8        1.2808             nan     0.1000    0.0356
##      9        1.2569             nan     0.1000    0.0425
##     10        1.2278             nan     0.1000    0.0325
##     20        1.0423             nan     0.1000    0.0227
##     40        0.8194             nan     0.1000    0.0117
##     60        0.6817             nan     0.1000    0.0073
##     80        0.5826             nan     0.1000    0.0054
##    100        0.5053             nan     0.1000    0.0047
##    120        0.4402             nan     0.1000    0.0020
##    140        0.3917             nan     0.1000    0.0038
##    150        0.3666             nan     0.1000    0.0017
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1906
##      2        1.4821             nan     0.1000    0.1528
##      3        1.3832             nan     0.1000    0.1213
##      4        1.3047             nan     0.1000    0.0879
##      5        1.2463             nan     0.1000    0.0818
##      6        1.1912             nan     0.1000    0.0923
##      7        1.1326             nan     0.1000    0.0642
##      8        1.0897             nan     0.1000    0.0731
##      9        1.0445             nan     0.1000    0.0536
##     10        1.0091             nan     0.1000    0.0599
##     20        0.7447             nan     0.1000    0.0279
##     40        0.4686             nan     0.1000    0.0184
##     60        0.3152             nan     0.1000    0.0076
##     80        0.2198             nan     0.1000    0.0037
##    100        0.1582             nan     0.1000    0.0036
##    120        0.1177             nan     0.1000    0.0013
##    140        0.0895             nan     0.1000    0.0011
##    150        0.0781             nan     0.1000    0.0015
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.2583
##      2        1.4388             nan     0.1000    0.1826
##      3        1.3178             nan     0.1000    0.1504
##      4        1.2199             nan     0.1000    0.1173
##      5        1.1419             nan     0.1000    0.1128
##      6        1.0707             nan     0.1000    0.0823
##      7        1.0171             nan     0.1000    0.0902
##      8        0.9596             nan     0.1000    0.0801
##      9        0.9094             nan     0.1000    0.0680
##     10        0.8668             nan     0.1000    0.0707
##     20        0.5698             nan     0.1000    0.0396
##     40        0.2985             nan     0.1000    0.0140
##     60        0.1733             nan     0.1000    0.0070
##     80        0.1088             nan     0.1000    0.0037
##    100        0.0723             nan     0.1000    0.0017
##    120        0.0491             nan     0.1000    0.0010
##    140        0.0354             nan     0.1000    0.0006
##    150        0.0309             nan     0.1000    0.0005
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1252
##      2        1.5245             nan     0.1000    0.0868
##      3        1.4664             nan     0.1000    0.0721
##      4        1.4203             nan     0.1000    0.0558
##      5        1.3824             nan     0.1000    0.0442
##      6        1.3515             nan     0.1000    0.0487
##      7        1.3197             nan     0.1000    0.0446
##      8        1.2905             nan     0.1000    0.0362
##      9        1.2634             nan     0.1000    0.0364
##     10        1.2392             nan     0.1000    0.0341
##     20        1.0561             nan     0.1000    0.0237
##     40        0.8311             nan     0.1000    0.0109
##     60        0.6895             nan     0.1000    0.0078
##     80        0.5914             nan     0.1000    0.0049
##    100        0.5114             nan     0.1000    0.0051
##    120        0.4464             nan     0.1000    0.0037
##    140        0.3929             nan     0.1000    0.0030
##    150        0.3707             nan     0.1000    0.0022
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1868
##      2        1.4864             nan     0.1000    0.1381
##      3        1.3960             nan     0.1000    0.1169
##      4        1.3214             nan     0.1000    0.0902
##      5        1.2627             nan     0.1000    0.0929
##      6        1.2029             nan     0.1000    0.0775
##      7        1.1511             nan     0.1000    0.0725
##      8        1.1037             nan     0.1000    0.0686
##      9        1.0583             nan     0.1000    0.0595
##     10        1.0214             nan     0.1000    0.0487
##     20        0.7520             nan     0.1000    0.0263
##     40        0.4673             nan     0.1000    0.0159
##     60        0.3069             nan     0.1000    0.0082
##     80        0.2120             nan     0.1000    0.0071
##    100        0.1508             nan     0.1000    0.0031
##    120        0.1115             nan     0.1000    0.0015
##    140        0.0841             nan     0.1000    0.0014
##    150        0.0746             nan     0.1000    0.0015
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.2485
##      2        1.4491             nan     0.1000    0.1797
##      3        1.3323             nan     0.1000    0.1629
##      4        1.2289             nan     0.1000    0.1197
##      5        1.1498             nan     0.1000    0.0986
##      6        1.0856             nan     0.1000    0.0905
##      7        1.0283             nan     0.1000    0.0771
##      8        0.9775             nan     0.1000    0.0899
##      9        0.9209             nan     0.1000    0.0670
##     10        0.8771             nan     0.1000    0.0504
##     20        0.5844             nan     0.1000    0.0433
##     40        0.2903             nan     0.1000    0.0146
##     60        0.1654             nan     0.1000    0.0079
##     80        0.1015             nan     0.1000    0.0044
##    100        0.0665             nan     0.1000    0.0021
##    120        0.0454             nan     0.1000    0.0006
##    140        0.0327             nan     0.1000    0.0007
##    150        0.0278             nan     0.1000    0.0004
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1330
##      2        1.5204             nan     0.1000    0.0875
##      3        1.4590             nan     0.1000    0.0680
##      4        1.4151             nan     0.1000    0.0580
##      5        1.3773             nan     0.1000    0.0554
##      6        1.3417             nan     0.1000    0.0492
##      7        1.3106             nan     0.1000    0.0357
##      8        1.2853             nan     0.1000    0.0457
##      9        1.2543             nan     0.1000    0.0352
##     10        1.2321             nan     0.1000    0.0368
##     20        1.0423             nan     0.1000    0.0213
##     40        0.8210             nan     0.1000    0.0100
##     60        0.6794             nan     0.1000    0.0081
##     80        0.5841             nan     0.1000    0.0061
##    100        0.5066             nan     0.1000    0.0039
##    120        0.4477             nan     0.1000    0.0035
##    140        0.3962             nan     0.1000    0.0029
##    150        0.3740             nan     0.1000    0.0018
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1962
##      2        1.4806             nan     0.1000    0.1435
##      3        1.3880             nan     0.1000    0.1062
##      4        1.3173             nan     0.1000    0.1052
##      5        1.2503             nan     0.1000    0.0826
##      6        1.1939             nan     0.1000    0.0732
##      7        1.1457             nan     0.1000    0.0699
##      8        1.1010             nan     0.1000    0.0746
##      9        1.0547             nan     0.1000    0.0620
##     10        1.0139             nan     0.1000    0.0492
##     20        0.7519             nan     0.1000    0.0275
##     40        0.4680             nan     0.1000    0.0144
##     60        0.3212             nan     0.1000    0.0114
##     80        0.2197             nan     0.1000    0.0050
##    100        0.1618             nan     0.1000    0.0034
##    120        0.1200             nan     0.1000    0.0026
##    140        0.0921             nan     0.1000    0.0011
##    150        0.0819             nan     0.1000    0.0014
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.2662
##      2        1.4396             nan     0.1000    0.1814
##      3        1.3231             nan     0.1000    0.1480
##      4        1.2265             nan     0.1000    0.1317
##      5        1.1436             nan     0.1000    0.0985
##      6        1.0783             nan     0.1000    0.0897
##      7        1.0193             nan     0.1000    0.0826
##      8        0.9670             nan     0.1000    0.0799
##      9        0.9157             nan     0.1000    0.0882
##     10        0.8612             nan     0.1000    0.0661
##     20        0.5678             nan     0.1000    0.0311
##     40        0.2944             nan     0.1000    0.0122
##     60        0.1721             nan     0.1000    0.0050
##     80        0.1120             nan     0.1000    0.0020
##    100        0.0766             nan     0.1000    0.0013
##    120        0.0534             nan     0.1000    0.0009
##    140        0.0381             nan     0.1000    0.0011
##    150        0.0324             nan     0.1000    0.0004
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1341
##      2        1.5196             nan     0.1000    0.0898
##      3        1.4584             nan     0.1000    0.0679
##      4        1.4132             nan     0.1000    0.0562
##      5        1.3758             nan     0.1000    0.0565
##      6        1.3391             nan     0.1000    0.0414
##      7        1.3098             nan     0.1000    0.0456
##      8        1.2778             nan     0.1000    0.0362
##      9        1.2524             nan     0.1000    0.0373
##     10        1.2281             nan     0.1000    0.0343
##     20        1.0432             nan     0.1000    0.0219
##     40        0.8185             nan     0.1000    0.0136
##     60        0.6810             nan     0.1000    0.0072
##     80        0.5779             nan     0.1000    0.0060
##    100        0.4965             nan     0.1000    0.0038
##    120        0.4384             nan     0.1000    0.0035
##    140        0.3887             nan     0.1000    0.0026
##    150        0.3665             nan     0.1000    0.0026
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.2008
##      2        1.4767             nan     0.1000    0.1418
##      3        1.3825             nan     0.1000    0.1171
##      4        1.3073             nan     0.1000    0.0918
##      5        1.2464             nan     0.1000    0.0832
##      6        1.1915             nan     0.1000    0.0802
##      7        1.1401             nan     0.1000    0.0663
##      8        1.0966             nan     0.1000    0.0646
##      9        1.0554             nan     0.1000    0.0639
##     10        1.0158             nan     0.1000    0.0507
##     20        0.7553             nan     0.1000    0.0319
##     40        0.4724             nan     0.1000    0.0185
##     60        0.3122             nan     0.1000    0.0077
##     80        0.2191             nan     0.1000    0.0066
##    100        0.1572             nan     0.1000    0.0032
##    120        0.1177             nan     0.1000    0.0022
##    140        0.0893             nan     0.1000    0.0012
##    150        0.0790             nan     0.1000    0.0013
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.2554
##      2        1.4442             nan     0.1000    0.1774
##      3        1.3310             nan     0.1000    0.1551
##      4        1.2348             nan     0.1000    0.1234
##      5        1.1546             nan     0.1000    0.0985
##      6        1.0884             nan     0.1000    0.0903
##      7        1.0283             nan     0.1000    0.0882
##      8        0.9728             nan     0.1000    0.0762
##      9        0.9248             nan     0.1000    0.0679
##     10        0.8811             nan     0.1000    0.0717
##     20        0.5674             nan     0.1000    0.0252
##     40        0.2975             nan     0.1000    0.0171
##     60        0.1665             nan     0.1000    0.0061
##     80        0.1043             nan     0.1000    0.0022
##    100        0.0689             nan     0.1000    0.0012
##    120        0.0474             nan     0.1000    0.0013
##    140        0.0342             nan     0.1000    0.0007
##    150        0.0291             nan     0.1000    0.0005
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1298
##      2        1.5195             nan     0.1000    0.0875
##      3        1.4595             nan     0.1000    0.0686
##      4        1.4133             nan     0.1000    0.0579
##      5        1.3746             nan     0.1000    0.0456
##      6        1.3439             nan     0.1000    0.0478
##      7        1.3118             nan     0.1000    0.0429
##      8        1.2843             nan     0.1000    0.0355
##      9        1.2607             nan     0.1000    0.0392
##     10        1.2348             nan     0.1000    0.0306
##     20        1.0427             nan     0.1000    0.0236
##     40        0.8208             nan     0.1000    0.0109
##     60        0.6814             nan     0.1000    0.0088
##     80        0.5783             nan     0.1000    0.0050
##    100        0.4983             nan     0.1000    0.0039
##    120        0.4370             nan     0.1000    0.0047
##    140        0.3847             nan     0.1000    0.0020
##    150        0.3635             nan     0.1000    0.0027
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1962
##      2        1.4820             nan     0.1000    0.1396
##      3        1.3896             nan     0.1000    0.1133
##      4        1.3149             nan     0.1000    0.0973
##      5        1.2520             nan     0.1000    0.0782
##      6        1.2009             nan     0.1000    0.0744
##      7        1.1512             nan     0.1000    0.0793
##      8        1.1003             nan     0.1000    0.0690
##      9        1.0563             nan     0.1000    0.0588
##     10        1.0195             nan     0.1000    0.0508
##     20        0.7481             nan     0.1000    0.0273
##     40        0.4622             nan     0.1000    0.0115
##     60        0.3174             nan     0.1000    0.0095
##     80        0.2250             nan     0.1000    0.0062
##    100        0.1664             nan     0.1000    0.0036
##    120        0.1245             nan     0.1000    0.0015
##    140        0.0932             nan     0.1000    0.0018
##    150        0.0829             nan     0.1000    0.0012
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.2529
##      2        1.4457             nan     0.1000    0.1728
##      3        1.3319             nan     0.1000    0.1479
##      4        1.2373             nan     0.1000    0.1152
##      5        1.1618             nan     0.1000    0.1054
##      6        1.0917             nan     0.1000    0.0929
##      7        1.0302             nan     0.1000    0.0825
##      8        0.9772             nan     0.1000    0.0778
##      9        0.9255             nan     0.1000    0.0825
##     10        0.8739             nan     0.1000    0.0685
##     20        0.5698             nan     0.1000    0.0344
##     40        0.2960             nan     0.1000    0.0132
##     60        0.1691             nan     0.1000    0.0071
##     80        0.1060             nan     0.1000    0.0029
##    100        0.0715             nan     0.1000    0.0015
##    120        0.0496             nan     0.1000    0.0015
##    140        0.0359             nan     0.1000    0.0007
##    150        0.0309             nan     0.1000    0.0003
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1205
##      2        1.5255             nan     0.1000    0.0801
##      3        1.4683             nan     0.1000    0.0611
##      4        1.4257             nan     0.1000    0.0558
##      5        1.3866             nan     0.1000    0.0468
##      6        1.3553             nan     0.1000    0.0464
##      7        1.3252             nan     0.1000    0.0420
##      8        1.2986             nan     0.1000    0.0381
##      9        1.2708             nan     0.1000    0.0436
##     10        1.2408             nan     0.1000    0.0325
##     20        1.0588             nan     0.1000    0.0207
##     40        0.8370             nan     0.1000    0.0109
##     60        0.6955             nan     0.1000    0.0077
##     80        0.5925             nan     0.1000    0.0063
##    100        0.5125             nan     0.1000    0.0058
##    120        0.4507             nan     0.1000    0.0040
##    140        0.3980             nan     0.1000    0.0031
##    150        0.3744             nan     0.1000    0.0036
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1831
##      2        1.4822             nan     0.1000    0.1421
##      3        1.3906             nan     0.1000    0.1084
##      4        1.3185             nan     0.1000    0.0858
##      5        1.2606             nan     0.1000    0.0850
##      6        1.2052             nan     0.1000    0.0592
##      7        1.1646             nan     0.1000    0.0703
##      8        1.1176             nan     0.1000    0.0612
##      9        1.0776             nan     0.1000    0.0651
##     10        1.0354             nan     0.1000    0.0548
##     20        0.7696             nan     0.1000    0.0292
##     40        0.4783             nan     0.1000    0.0183
##     60        0.3171             nan     0.1000    0.0076
##     80        0.2238             nan     0.1000    0.0067
##    100        0.1617             nan     0.1000    0.0024
##    120        0.1192             nan     0.1000    0.0010
##    140        0.0905             nan     0.1000    0.0009
##    150        0.0798             nan     0.1000    0.0010
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.2537
##      2        1.4476             nan     0.1000    0.1748
##      3        1.3317             nan     0.1000    0.1448
##      4        1.2378             nan     0.1000    0.1122
##      5        1.1635             nan     0.1000    0.1103
##      6        1.0909             nan     0.1000    0.0985
##      7        1.0281             nan     0.1000    0.0804
##      8        0.9747             nan     0.1000    0.0770
##      9        0.9243             nan     0.1000    0.0710
##     10        0.8790             nan     0.1000    0.0616
##     20        0.5739             nan     0.1000    0.0341
##     40        0.2951             nan     0.1000    0.0133
##     60        0.1718             nan     0.1000    0.0060
##     80        0.1084             nan     0.1000    0.0028
##    100        0.0736             nan     0.1000    0.0018
##    120        0.0510             nan     0.1000    0.0006
##    140        0.0364             nan     0.1000    0.0007
##    150        0.0308             nan     0.1000    0.0004
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1335
##      2        1.5181             nan     0.1000    0.0903
##      3        1.4560             nan     0.1000    0.0688
##      4        1.4094             nan     0.1000    0.0594
##      5        1.3705             nan     0.1000    0.0461
##      6        1.3390             nan     0.1000    0.0481
##      7        1.3068             nan     0.1000    0.0423
##      8        1.2791             nan     0.1000    0.0420
##      9        1.2490             nan     0.1000    0.0349
##     10        1.2260             nan     0.1000    0.0358
##     20        1.0379             nan     0.1000    0.0216
##     40        0.8158             nan     0.1000    0.0114
##     60        0.6766             nan     0.1000    0.0062
##     80        0.5768             nan     0.1000    0.0056
##    100        0.5020             nan     0.1000    0.0053
##    120        0.4401             nan     0.1000    0.0039
##    140        0.3893             nan     0.1000    0.0040
##    150        0.3658             nan     0.1000    0.0021
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.2104
##      2        1.4719             nan     0.1000    0.1431
##      3        1.3778             nan     0.1000    0.1099
##      4        1.3060             nan     0.1000    0.0905
##      5        1.2441             nan     0.1000    0.0763
##      6        1.1922             nan     0.1000    0.0737
##      7        1.1432             nan     0.1000    0.0682
##      8        1.0974             nan     0.1000    0.0692
##      9        1.0548             nan     0.1000    0.0615
##     10        1.0150             nan     0.1000    0.0588
##     20        0.7512             nan     0.1000    0.0416
##     40        0.4640             nan     0.1000    0.0119
##     60        0.3113             nan     0.1000    0.0100
##     80        0.2140             nan     0.1000    0.0030
##    100        0.1517             nan     0.1000    0.0041
##    120        0.1119             nan     0.1000    0.0026
##    140        0.0829             nan     0.1000    0.0007
##    150        0.0736             nan     0.1000    0.0018
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.2674
##      2        1.4381             nan     0.1000    0.1902
##      3        1.3169             nan     0.1000    0.1480
##      4        1.2202             nan     0.1000    0.1196
##      5        1.1427             nan     0.1000    0.1035
##      6        1.0760             nan     0.1000    0.0927
##      7        1.0171             nan     0.1000    0.0946
##      8        0.9573             nan     0.1000    0.0688
##      9        0.9134             nan     0.1000    0.0630
##     10        0.8718             nan     0.1000    0.0599
##     20        0.5723             nan     0.1000    0.0284
##     40        0.2974             nan     0.1000    0.0150
##     60        0.1682             nan     0.1000    0.0060
##     80        0.1049             nan     0.1000    0.0024
##    100        0.0699             nan     0.1000    0.0015
##    120        0.0475             nan     0.1000    0.0014
##    140        0.0336             nan     0.1000    0.0006
##    150        0.0285             nan     0.1000    0.0003
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1218
##      2        1.5226             nan     0.1000    0.0924
##      3        1.4618             nan     0.1000    0.0657
##      4        1.4164             nan     0.1000    0.0540
##      5        1.3805             nan     0.1000    0.0477
##      6        1.3485             nan     0.1000    0.0494
##      7        1.3175             nan     0.1000    0.0439
##      8        1.2897             nan     0.1000    0.0366
##      9        1.2661             nan     0.1000    0.0370
##     10        1.2404             nan     0.1000    0.0293
##     20        1.0556             nan     0.1000    0.0229
##     40        0.8314             nan     0.1000    0.0120
##     60        0.6927             nan     0.1000    0.0069
##     80        0.5857             nan     0.1000    0.0051
##    100        0.5113             nan     0.1000    0.0051
##    120        0.4469             nan     0.1000    0.0039
##    140        0.3949             nan     0.1000    0.0029
##    150        0.3712             nan     0.1000    0.0034
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1812
##      2        1.4872             nan     0.1000    0.1490
##      3        1.3909             nan     0.1000    0.1148
##      4        1.3155             nan     0.1000    0.1051
##      5        1.2495             nan     0.1000    0.0787
##      6        1.1959             nan     0.1000    0.0774
##      7        1.1474             nan     0.1000    0.0700
##      8        1.1016             nan     0.1000    0.0587
##      9        1.0627             nan     0.1000    0.0529
##     10        1.0274             nan     0.1000    0.0621
##     20        0.7543             nan     0.1000    0.0314
##     40        0.4674             nan     0.1000    0.0217
##     60        0.3158             nan     0.1000    0.0113
##     80        0.2195             nan     0.1000    0.0046
##    100        0.1596             nan     0.1000    0.0025
##    120        0.1180             nan     0.1000    0.0019
##    140        0.0903             nan     0.1000    0.0018
##    150        0.0799             nan     0.1000    0.0013
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.2604
##      2        1.4432             nan     0.1000    0.1724
##      3        1.3314             nan     0.1000    0.1397
##      4        1.2385             nan     0.1000    0.1281
##      5        1.1582             nan     0.1000    0.1196
##      6        1.0820             nan     0.1000    0.0893
##      7        1.0241             nan     0.1000    0.0971
##      8        0.9615             nan     0.1000    0.0790
##      9        0.9134             nan     0.1000    0.0699
##     10        0.8682             nan     0.1000    0.0697
##     20        0.5771             nan     0.1000    0.0325
##     40        0.2936             nan     0.1000    0.0101
##     60        0.1706             nan     0.1000    0.0085
##     80        0.1073             nan     0.1000    0.0026
##    100        0.0739             nan     0.1000    0.0016
##    120        0.0526             nan     0.1000    0.0007
##    140        0.0383             nan     0.1000    0.0003
##    150        0.0329             nan     0.1000    0.0006
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1213
##      2        1.5255             nan     0.1000    0.0831
##      3        1.4685             nan     0.1000    0.0652
##      4        1.4245             nan     0.1000    0.0576
##      5        1.3853             nan     0.1000    0.0533
##      6        1.3505             nan     0.1000    0.0446
##      7        1.3208             nan     0.1000    0.0428
##      8        1.2926             nan     0.1000    0.0354
##      9        1.2691             nan     0.1000    0.0352
##     10        1.2446             nan     0.1000    0.0320
##     20        1.0562             nan     0.1000    0.0188
##     40        0.8397             nan     0.1000    0.0136
##     60        0.6939             nan     0.1000    0.0096
##     80        0.5918             nan     0.1000    0.0050
##    100        0.5100             nan     0.1000    0.0040
##    120        0.4477             nan     0.1000    0.0033
##    140        0.3951             nan     0.1000    0.0041
##    150        0.3715             nan     0.1000    0.0030
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1938
##      2        1.4817             nan     0.1000    0.1312
##      3        1.3951             nan     0.1000    0.1200
##      4        1.3179             nan     0.1000    0.1000
##      5        1.2521             nan     0.1000    0.0816
##      6        1.1998             nan     0.1000    0.0696
##      7        1.1537             nan     0.1000    0.0706
##      8        1.1078             nan     0.1000    0.0780
##      9        1.0600             nan     0.1000    0.0541
##     10        1.0245             nan     0.1000    0.0572
##     20        0.7447             nan     0.1000    0.0282
##     40        0.4668             nan     0.1000    0.0134
##     60        0.3123             nan     0.1000    0.0074
##     80        0.2139             nan     0.1000    0.0050
##    100        0.1517             nan     0.1000    0.0042
##    120        0.1128             nan     0.1000    0.0021
##    140        0.0865             nan     0.1000    0.0012
##    150        0.0759             nan     0.1000    0.0017
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.2507
##      2        1.4478             nan     0.1000    0.1833
##      3        1.3298             nan     0.1000    0.1337
##      4        1.2406             nan     0.1000    0.1095
##      5        1.1662             nan     0.1000    0.1063
##      6        1.0979             nan     0.1000    0.0872
##      7        1.0410             nan     0.1000    0.0915
##      8        0.9806             nan     0.1000    0.0797
##      9        0.9304             nan     0.1000    0.0642
##     10        0.8887             nan     0.1000    0.0616
##     20        0.5751             nan     0.1000    0.0359
##     40        0.2900             nan     0.1000    0.0130
##     60        0.1641             nan     0.1000    0.0071
##     80        0.1012             nan     0.1000    0.0032
##    100        0.0663             nan     0.1000    0.0017
##    120        0.0457             nan     0.1000    0.0006
##    140        0.0323             nan     0.1000    0.0006
##    150        0.0276             nan     0.1000    0.0004
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1278
##      2        1.5216             nan     0.1000    0.0894
##      3        1.4608             nan     0.1000    0.0651
##      4        1.4160             nan     0.1000    0.0601
##      5        1.3776             nan     0.1000    0.0451
##      6        1.3454             nan     0.1000    0.0496
##      7        1.3130             nan     0.1000    0.0431
##      8        1.2845             nan     0.1000    0.0400
##      9        1.2568             nan     0.1000    0.0367
##     10        1.2342             nan     0.1000    0.0329
##     20        1.0526             nan     0.1000    0.0208
##     40        0.8317             nan     0.1000    0.0097
##     60        0.6928             nan     0.1000    0.0072
##     80        0.5902             nan     0.1000    0.0055
##    100        0.5127             nan     0.1000    0.0056
##    120        0.4467             nan     0.1000    0.0018
##    140        0.3961             nan     0.1000    0.0041
##    150        0.3723             nan     0.1000    0.0039
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1885
##      2        1.4840             nan     0.1000    0.1425
##      3        1.3905             nan     0.1000    0.1060
##      4        1.3199             nan     0.1000    0.0996
##      5        1.2564             nan     0.1000    0.0785
##      6        1.2053             nan     0.1000    0.0723
##      7        1.1550             nan     0.1000    0.0830
##      8        1.1027             nan     0.1000    0.0622
##      9        1.0630             nan     0.1000    0.0607
##     10        1.0243             nan     0.1000    0.0452
##     20        0.7592             nan     0.1000    0.0299
##     40        0.4702             nan     0.1000    0.0185
##     60        0.3158             nan     0.1000    0.0089
##     80        0.2201             nan     0.1000    0.0055
##    100        0.1595             nan     0.1000    0.0030
##    120        0.1183             nan     0.1000    0.0029
##    140        0.0881             nan     0.1000    0.0019
##    150        0.0773             nan     0.1000    0.0011
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.2488
##      2        1.4470             nan     0.1000    0.1835
##      3        1.3286             nan     0.1000    0.1452
##      4        1.2339             nan     0.1000    0.1196
##      5        1.1560             nan     0.1000    0.1027
##      6        1.0896             nan     0.1000    0.0973
##      7        1.0271             nan     0.1000    0.0762
##      8        0.9765             nan     0.1000    0.0807
##      9        0.9262             nan     0.1000    0.0692
##     10        0.8811             nan     0.1000    0.0626
##     20        0.5801             nan     0.1000    0.0332
##     40        0.2945             nan     0.1000    0.0140
##     60        0.1670             nan     0.1000    0.0051
##     80        0.1060             nan     0.1000    0.0029
##    100        0.0679             nan     0.1000    0.0012
##    120        0.0469             nan     0.1000    0.0010
##    140        0.0338             nan     0.1000    0.0009
##    150        0.0285             nan     0.1000    0.0006
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1289
##      2        1.5199             nan     0.1000    0.0900
##      3        1.4584             nan     0.1000    0.0711
##      4        1.4116             nan     0.1000    0.0568
##      5        1.3724             nan     0.1000    0.0548
##      6        1.3361             nan     0.1000    0.0442
##      7        1.3076             nan     0.1000    0.0385
##      8        1.2819             nan     0.1000    0.0418
##      9        1.2560             nan     0.1000    0.0338
##     10        1.2325             nan     0.1000    0.0329
##     20        1.0412             nan     0.1000    0.0200
##     40        0.8244             nan     0.1000    0.0118
##     60        0.6875             nan     0.1000    0.0092
##     80        0.5843             nan     0.1000    0.0049
##    100        0.5082             nan     0.1000    0.0051
##    120        0.4463             nan     0.1000    0.0053
##    140        0.3964             nan     0.1000    0.0028
##    150        0.3755             nan     0.1000    0.0027
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1959
##      2        1.4777             nan     0.1000    0.1460
##      3        1.3806             nan     0.1000    0.1080
##      4        1.3067             nan     0.1000    0.0996
##      5        1.2429             nan     0.1000    0.0739
##      6        1.1923             nan     0.1000    0.0784
##      7        1.1435             nan     0.1000    0.0661
##      8        1.0989             nan     0.1000    0.0709
##      9        1.0539             nan     0.1000    0.0611
##     10        1.0138             nan     0.1000    0.0466
##     20        0.7522             nan     0.1000    0.0357
##     40        0.4752             nan     0.1000    0.0147
##     60        0.3189             nan     0.1000    0.0058
##     80        0.2232             nan     0.1000    0.0058
##    100        0.1599             nan     0.1000    0.0031
##    120        0.1192             nan     0.1000    0.0029
##    140        0.0908             nan     0.1000    0.0012
##    150        0.0796             nan     0.1000    0.0013
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.2505
##      2        1.4450             nan     0.1000    0.1765
##      3        1.3295             nan     0.1000    0.1375
##      4        1.2388             nan     0.1000    0.1183
##      5        1.1608             nan     0.1000    0.0997
##      6        1.0945             nan     0.1000    0.0907
##      7        1.0350             nan     0.1000    0.0731
##      8        0.9877             nan     0.1000    0.0762
##      9        0.9379             nan     0.1000    0.0847
##     10        0.8867             nan     0.1000    0.0759
##     20        0.5631             nan     0.1000    0.0282
##     40        0.2934             nan     0.1000    0.0130
##     60        0.1680             nan     0.1000    0.0065
##     80        0.1056             nan     0.1000    0.0031
##    100        0.0694             nan     0.1000    0.0018
##    120        0.0485             nan     0.1000    0.0006
##    140        0.0348             nan     0.1000    0.0005
##    150        0.0296             nan     0.1000    0.0004
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1246
##      2        1.5238             nan     0.1000    0.0907
##      3        1.4628             nan     0.1000    0.0672
##      4        1.4179             nan     0.1000    0.0584
##      5        1.3798             nan     0.1000    0.0490
##      6        1.3473             nan     0.1000    0.0470
##      7        1.3158             nan     0.1000    0.0460
##      8        1.2870             nan     0.1000    0.0364
##      9        1.2633             nan     0.1000    0.0430
##     10        1.2345             nan     0.1000    0.0349
##     20        1.0438             nan     0.1000    0.0231
##     40        0.8208             nan     0.1000    0.0138
##     60        0.6851             nan     0.1000    0.0087
##     80        0.5790             nan     0.1000    0.0058
##    100        0.4994             nan     0.1000    0.0047
##    120        0.4386             nan     0.1000    0.0029
##    140        0.3898             nan     0.1000    0.0030
##    150        0.3676             nan     0.1000    0.0035
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1960
##      2        1.4816             nan     0.1000    0.1422
##      3        1.3864             nan     0.1000    0.1151
##      4        1.3105             nan     0.1000    0.1019
##      5        1.2436             nan     0.1000    0.0891
##      6        1.1862             nan     0.1000    0.0744
##      7        1.1360             nan     0.1000    0.0692
##      8        1.0904             nan     0.1000    0.0664
##      9        1.0486             nan     0.1000    0.0513
##     10        1.0138             nan     0.1000    0.0525
##     20        0.7527             nan     0.1000    0.0339
##     40        0.4634             nan     0.1000    0.0134
##     60        0.3118             nan     0.1000    0.0104
##     80        0.2207             nan     0.1000    0.0052
##    100        0.1615             nan     0.1000    0.0033
##    120        0.1200             nan     0.1000    0.0020
##    140        0.0917             nan     0.1000    0.0016
##    150        0.0811             nan     0.1000    0.0012
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.2570
##      2        1.4439             nan     0.1000    0.1899
##      3        1.3205             nan     0.1000    0.1499
##      4        1.2240             nan     0.1000    0.1274
##      5        1.1433             nan     0.1000    0.0997
##      6        1.0790             nan     0.1000    0.0917
##      7        1.0222             nan     0.1000    0.0961
##      8        0.9626             nan     0.1000    0.0762
##      9        0.9145             nan     0.1000    0.0791
##     10        0.8636             nan     0.1000    0.0615
##     20        0.5782             nan     0.1000    0.0358
##     40        0.2960             nan     0.1000    0.0110
##     60        0.1686             nan     0.1000    0.0069
##     80        0.1021             nan     0.1000    0.0032
##    100        0.0685             nan     0.1000    0.0019
##    120        0.0466             nan     0.1000    0.0010
##    140        0.0332             nan     0.1000    0.0005
##    150        0.0284             nan     0.1000    0.0005
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1310
##      2        1.5204             nan     0.1000    0.0931
##      3        1.4594             nan     0.1000    0.0690
##      4        1.4129             nan     0.1000    0.0535
##      5        1.3761             nan     0.1000    0.0514
##      6        1.3422             nan     0.1000    0.0486
##      7        1.3109             nan     0.1000    0.0373
##      8        1.2857             nan     0.1000    0.0374
##      9        1.2614             nan     0.1000    0.0354
##     10        1.2385             nan     0.1000    0.0331
##     20        1.0502             nan     0.1000    0.0186
##     40        0.8340             nan     0.1000    0.0107
##     60        0.6965             nan     0.1000    0.0091
##     80        0.5903             nan     0.1000    0.0052
##    100        0.5128             nan     0.1000    0.0062
##    120        0.4489             nan     0.1000    0.0033
##    140        0.3970             nan     0.1000    0.0020
##    150        0.3749             nan     0.1000    0.0022
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.2022
##      2        1.4772             nan     0.1000    0.1340
##      3        1.3856             nan     0.1000    0.1092
##      4        1.3126             nan     0.1000    0.0890
##      5        1.2545             nan     0.1000    0.0807
##      6        1.2007             nan     0.1000    0.0747
##      7        1.1539             nan     0.1000    0.0659
##      8        1.1096             nan     0.1000    0.0784
##      9        1.0592             nan     0.1000    0.0585
##     10        1.0214             nan     0.1000    0.0594
##     20        0.7627             nan     0.1000    0.0293
##     40        0.4713             nan     0.1000    0.0170
##     60        0.3173             nan     0.1000    0.0095
##     80        0.2210             nan     0.1000    0.0040
##    100        0.1574             nan     0.1000    0.0021
##    120        0.1175             nan     0.1000    0.0021
##    140        0.0892             nan     0.1000    0.0020
##    150        0.0786             nan     0.1000    0.0020
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.2571
##      2        1.4437             nan     0.1000    0.1848
##      3        1.3243             nan     0.1000    0.1449
##      4        1.2302             nan     0.1000    0.1151
##      5        1.1532             nan     0.1000    0.1086
##      6        1.0833             nan     0.1000    0.0811
##      7        1.0299             nan     0.1000    0.0821
##      8        0.9768             nan     0.1000    0.0818
##      9        0.9263             nan     0.1000    0.0664
##     10        0.8824             nan     0.1000    0.0726
##     20        0.5749             nan     0.1000    0.0306
##     40        0.2982             nan     0.1000    0.0136
##     60        0.1700             nan     0.1000    0.0061
##     80        0.1039             nan     0.1000    0.0033
##    100        0.0686             nan     0.1000    0.0007
##    120        0.0485             nan     0.1000    0.0013
##    140        0.0339             nan     0.1000    0.0004
##    150        0.0289             nan     0.1000    0.0006
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1291
##      2        1.5220             nan     0.1000    0.0912
##      3        1.4613             nan     0.1000    0.0697
##      4        1.4147             nan     0.1000    0.0538
##      5        1.3783             nan     0.1000    0.0452
##      6        1.3478             nan     0.1000    0.0512
##      7        1.3156             nan     0.1000    0.0455
##      8        1.2876             nan     0.1000    0.0419
##      9        1.2599             nan     0.1000    0.0422
##     10        1.2325             nan     0.1000    0.0324
##     20        1.0427             nan     0.1000    0.0224
##     40        0.8183             nan     0.1000    0.0114
##     60        0.6778             nan     0.1000    0.0082
##     80        0.5754             nan     0.1000    0.0061
##    100        0.4980             nan     0.1000    0.0050
##    120        0.4354             nan     0.1000    0.0037
##    140        0.3838             nan     0.1000    0.0030
##    150        0.3631             nan     0.1000    0.0030
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1939
##      2        1.4799             nan     0.1000    0.1364
##      3        1.3890             nan     0.1000    0.1183
##      4        1.3110             nan     0.1000    0.0995
##      5        1.2475             nan     0.1000    0.0764
##      6        1.1969             nan     0.1000    0.0887
##      7        1.1420             nan     0.1000    0.0707
##      8        1.0976             nan     0.1000    0.0585
##      9        1.0606             nan     0.1000    0.0560
##     10        1.0237             nan     0.1000    0.0505
##     20        0.7457             nan     0.1000    0.0246
##     40        0.4660             nan     0.1000    0.0131
##     60        0.3124             nan     0.1000    0.0100
##     80        0.2191             nan     0.1000    0.0045
##    100        0.1593             nan     0.1000    0.0018
##    120        0.1192             nan     0.1000    0.0025
##    140        0.0896             nan     0.1000    0.0018
##    150        0.0783             nan     0.1000    0.0011
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.2541
##      2        1.4459             nan     0.1000    0.1874
##      3        1.3274             nan     0.1000    0.1497
##      4        1.2318             nan     0.1000    0.1213
##      5        1.1539             nan     0.1000    0.1124
##      6        1.0820             nan     0.1000    0.0960
##      7        1.0209             nan     0.1000    0.0745
##      8        0.9714             nan     0.1000    0.0894
##      9        0.9172             nan     0.1000    0.0697
##     10        0.8732             nan     0.1000    0.0736
##     20        0.5598             nan     0.1000    0.0376
##     40        0.2863             nan     0.1000    0.0134
##     60        0.1630             nan     0.1000    0.0065
##     80        0.0997             nan     0.1000    0.0035
##    100        0.0664             nan     0.1000    0.0018
##    120        0.0445             nan     0.1000    0.0010
##    140        0.0318             nan     0.1000    0.0010
##    150        0.0266             nan     0.1000    0.0004
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1308
##      2        1.5225             nan     0.1000    0.0896
##      3        1.4627             nan     0.1000    0.0665
##      4        1.4166             nan     0.1000    0.0590
##      5        1.3773             nan     0.1000    0.0502
##      6        1.3442             nan     0.1000    0.0462
##      7        1.3135             nan     0.1000    0.0408
##      8        1.2864             nan     0.1000    0.0396
##      9        1.2595             nan     0.1000    0.0337
##     10        1.2353             nan     0.1000    0.0369
##     20        1.0544             nan     0.1000    0.0218
##     40        0.8326             nan     0.1000    0.0112
##     60        0.6940             nan     0.1000    0.0098
##     80        0.5920             nan     0.1000    0.0058
##    100        0.5116             nan     0.1000    0.0050
##    120        0.4477             nan     0.1000    0.0025
##    140        0.3965             nan     0.1000    0.0018
##    150        0.3762             nan     0.1000    0.0017
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1863
##      2        1.4850             nan     0.1000    0.1503
##      3        1.3882             nan     0.1000    0.1131
##      4        1.3147             nan     0.1000    0.0949
##      5        1.2498             nan     0.1000    0.0991
##      6        1.1880             nan     0.1000    0.0720
##      7        1.1410             nan     0.1000    0.0665
##      8        1.0971             nan     0.1000    0.0692
##      9        1.0530             nan     0.1000    0.0479
##     10        1.0207             nan     0.1000    0.0601
##     20        0.7582             nan     0.1000    0.0265
##     40        0.4759             nan     0.1000    0.0153
##     60        0.3207             nan     0.1000    0.0071
##     80        0.2252             nan     0.1000    0.0048
##    100        0.1618             nan     0.1000    0.0035
##    120        0.1222             nan     0.1000    0.0033
##    140        0.0930             nan     0.1000    0.0019
##    150        0.0818             nan     0.1000    0.0018
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.2597
##      2        1.4397             nan     0.1000    0.1835
##      3        1.3243             nan     0.1000    0.1512
##      4        1.2284             nan     0.1000    0.1095
##      5        1.1573             nan     0.1000    0.1121
##      6        1.0850             nan     0.1000    0.0909
##      7        1.0271             nan     0.1000    0.0835
##      8        0.9736             nan     0.1000    0.0807
##      9        0.9233             nan     0.1000    0.0646
##     10        0.8824             nan     0.1000    0.0641
##     20        0.5775             nan     0.1000    0.0243
##     40        0.2940             nan     0.1000    0.0157
##     60        0.1659             nan     0.1000    0.0070
##     80        0.1045             nan     0.1000    0.0033
##    100        0.0679             nan     0.1000    0.0014
##    120        0.0459             nan     0.1000    0.0008
##    140        0.0325             nan     0.1000    0.0006
##    150        0.0280             nan     0.1000    0.0008
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1337
##      2        1.5197             nan     0.1000    0.0866
##      3        1.4601             nan     0.1000    0.0707
##      4        1.4138             nan     0.1000    0.0569
##      5        1.3760             nan     0.1000    0.0498
##      6        1.3426             nan     0.1000    0.0493
##      7        1.3102             nan     0.1000    0.0350
##      8        1.2865             nan     0.1000    0.0378
##      9        1.2612             nan     0.1000    0.0316
##     10        1.2392             nan     0.1000    0.0366
##     20        1.0467             nan     0.1000    0.0193
##     40        0.8275             nan     0.1000    0.0101
##     60        0.6880             nan     0.1000    0.0065
##     80        0.5834             nan     0.1000    0.0052
##    100        0.5062             nan     0.1000    0.0040
##    120        0.4431             nan     0.1000    0.0039
##    140        0.3941             nan     0.1000    0.0044
##    150        0.3707             nan     0.1000    0.0027
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.2018
##      2        1.4787             nan     0.1000    0.1441
##      3        1.3843             nan     0.1000    0.1074
##      4        1.3141             nan     0.1000    0.0926
##      5        1.2529             nan     0.1000    0.0804
##      6        1.1975             nan     0.1000    0.0825
##      7        1.1455             nan     0.1000    0.0613
##      8        1.1041             nan     0.1000    0.0633
##      9        1.0628             nan     0.1000    0.0556
##     10        1.0263             nan     0.1000    0.0482
##     20        0.7665             nan     0.1000    0.0237
##     40        0.4771             nan     0.1000    0.0185
##     60        0.3102             nan     0.1000    0.0103
##     80        0.2134             nan     0.1000    0.0057
##    100        0.1532             nan     0.1000    0.0040
##    120        0.1113             nan     0.1000    0.0020
##    140        0.0829             nan     0.1000    0.0014
##    150        0.0721             nan     0.1000    0.0013
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.2593
##      2        1.4437             nan     0.1000    0.1794
##      3        1.3266             nan     0.1000    0.1329
##      4        1.2383             nan     0.1000    0.1235
##      5        1.1577             nan     0.1000    0.1065
##      6        1.0900             nan     0.1000    0.0973
##      7        1.0274             nan     0.1000    0.0930
##      8        0.9702             nan     0.1000    0.0892
##      9        0.9153             nan     0.1000    0.0567
##     10        0.8779             nan     0.1000    0.0703
##     20        0.5663             nan     0.1000    0.0215
##     40        0.2986             nan     0.1000    0.0145
##     60        0.1649             nan     0.1000    0.0069
##     80        0.1034             nan     0.1000    0.0039
##    100        0.0670             nan     0.1000    0.0014
##    120        0.0461             nan     0.1000    0.0011
##    140        0.0326             nan     0.1000    0.0004
##    150        0.0280             nan     0.1000    0.0008
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1223
##      2        1.5263             nan     0.1000    0.0804
##      3        1.4692             nan     0.1000    0.0668
##      4        1.4242             nan     0.1000    0.0480
##      5        1.3897             nan     0.1000    0.0590
##      6        1.3538             nan     0.1000    0.0463
##      7        1.3229             nan     0.1000    0.0352
##      8        1.2986             nan     0.1000    0.0439
##      9        1.2690             nan     0.1000    0.0325
##     10        1.2462             nan     0.1000    0.0364
##     20        1.0600             nan     0.1000    0.0194
##     40        0.8363             nan     0.1000    0.0125
##     60        0.6967             nan     0.1000    0.0077
##     80        0.5938             nan     0.1000    0.0059
##    100        0.5167             nan     0.1000    0.0058
##    120        0.4503             nan     0.1000    0.0024
##    140        0.3945             nan     0.1000    0.0027
##    150        0.3734             nan     0.1000    0.0032
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1867
##      2        1.4829             nan     0.1000    0.1429
##      3        1.3896             nan     0.1000    0.1083
##      4        1.3196             nan     0.1000    0.0953
##      5        1.2568             nan     0.1000    0.0795
##      6        1.2030             nan     0.1000    0.0780
##      7        1.1514             nan     0.1000    0.0659
##      8        1.1086             nan     0.1000    0.0628
##      9        1.0681             nan     0.1000    0.0566
##     10        1.0319             nan     0.1000    0.0668
##     20        0.7583             nan     0.1000    0.0240
##     40        0.4720             nan     0.1000    0.0143
##     60        0.3151             nan     0.1000    0.0098
##     80        0.2168             nan     0.1000    0.0051
##    100        0.1579             nan     0.1000    0.0031
##    120        0.1192             nan     0.1000    0.0022
##    140        0.0892             nan     0.1000    0.0014
##    150        0.0782             nan     0.1000    0.0014
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.2558
##      2        1.4455             nan     0.1000    0.1745
##      3        1.3332             nan     0.1000    0.1384
##      4        1.2445             nan     0.1000    0.1102
##      5        1.1718             nan     0.1000    0.1020
##      6        1.1041             nan     0.1000    0.0968
##      7        1.0413             nan     0.1000    0.1018
##      8        0.9767             nan     0.1000    0.0739
##      9        0.9306             nan     0.1000    0.0637
##     10        0.8902             nan     0.1000    0.0694
##     20        0.5726             nan     0.1000    0.0311
##     40        0.2927             nan     0.1000    0.0139
##     60        0.1704             nan     0.1000    0.0057
##     80        0.1075             nan     0.1000    0.0028
##    100        0.0715             nan     0.1000    0.0026
##    120        0.0492             nan     0.1000    0.0013
##    140        0.0346             nan     0.1000    0.0006
##    150        0.0299             nan     0.1000    0.0006
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1283
##      2        1.5203             nan     0.1000    0.0868
##      3        1.4613             nan     0.1000    0.0682
##      4        1.4170             nan     0.1000    0.0530
##      5        1.3797             nan     0.1000    0.0544
##      6        1.3442             nan     0.1000    0.0459
##      7        1.3132             nan     0.1000    0.0436
##      8        1.2845             nan     0.1000    0.0378
##      9        1.2584             nan     0.1000    0.0306
##     10        1.2368             nan     0.1000    0.0381
##     20        1.0460             nan     0.1000    0.0202
##     40        0.8297             nan     0.1000    0.0099
##     60        0.6916             nan     0.1000    0.0080
##     80        0.5874             nan     0.1000    0.0061
##    100        0.5055             nan     0.1000    0.0036
##    120        0.4438             nan     0.1000    0.0037
##    140        0.3941             nan     0.1000    0.0027
##    150        0.3713             nan     0.1000    0.0025
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1976
##      2        1.4796             nan     0.1000    0.1367
##      3        1.3890             nan     0.1000    0.1126
##      4        1.3143             nan     0.1000    0.0978
##      5        1.2516             nan     0.1000    0.0921
##      6        1.1918             nan     0.1000    0.0766
##      7        1.1413             nan     0.1000    0.0620
##      8        1.0991             nan     0.1000    0.0680
##      9        1.0562             nan     0.1000    0.0552
##     10        1.0200             nan     0.1000    0.0544
##     20        0.7463             nan     0.1000    0.0314
##     40        0.4690             nan     0.1000    0.0143
##     60        0.3162             nan     0.1000    0.0082
##     80        0.2205             nan     0.1000    0.0058
##    100        0.1584             nan     0.1000    0.0035
##    120        0.1184             nan     0.1000    0.0027
##    140        0.0888             nan     0.1000    0.0019
##    150        0.0776             nan     0.1000    0.0006
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.2539
##      2        1.4448             nan     0.1000    0.1793
##      3        1.3289             nan     0.1000    0.1445
##      4        1.2353             nan     0.1000    0.1257
##      5        1.1565             nan     0.1000    0.1195
##      6        1.0823             nan     0.1000    0.0914
##      7        1.0220             nan     0.1000    0.0836
##      8        0.9689             nan     0.1000    0.0693
##      9        0.9241             nan     0.1000    0.0770
##     10        0.8757             nan     0.1000    0.0527
##     20        0.5765             nan     0.1000    0.0379
##     40        0.3044             nan     0.1000    0.0119
##     60        0.1737             nan     0.1000    0.0088
##     80        0.1087             nan     0.1000    0.0036
##    100        0.0725             nan     0.1000    0.0019
##    120        0.0505             nan     0.1000    0.0012
##    140        0.0353             nan     0.1000    0.0006
##    150        0.0299             nan     0.1000    0.0007
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1280
##      2        1.5229             nan     0.1000    0.0901
##      3        1.4627             nan     0.1000    0.0661
##      4        1.4177             nan     0.1000    0.0537
##      5        1.3810             nan     0.1000    0.0515
##      6        1.3474             nan     0.1000    0.0512
##      7        1.3146             nan     0.1000    0.0344
##      8        1.2897             nan     0.1000    0.0398
##      9        1.2609             nan     0.1000    0.0349
##     10        1.2372             nan     0.1000    0.0357
##     20        1.0527             nan     0.1000    0.0206
##     40        0.8262             nan     0.1000    0.0112
##     60        0.6848             nan     0.1000    0.0085
##     80        0.5835             nan     0.1000    0.0064
##    100        0.5047             nan     0.1000    0.0035
##    120        0.4430             nan     0.1000    0.0034
##    140        0.3908             nan     0.1000    0.0022
##    150        0.3684             nan     0.1000    0.0023
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.2014
##      2        1.4786             nan     0.1000    0.1354
##      3        1.3881             nan     0.1000    0.1020
##      4        1.3196             nan     0.1000    0.0919
##      5        1.2581             nan     0.1000    0.0951
##      6        1.1965             nan     0.1000    0.0782
##      7        1.1472             nan     0.1000    0.0746
##      8        1.1001             nan     0.1000    0.0648
##      9        1.0572             nan     0.1000    0.0518
##     10        1.0231             nan     0.1000    0.0526
##     20        0.7547             nan     0.1000    0.0256
##     40        0.4756             nan     0.1000    0.0157
##     60        0.3187             nan     0.1000    0.0084
##     80        0.2202             nan     0.1000    0.0053
##    100        0.1591             nan     0.1000    0.0040
##    120        0.1178             nan     0.1000    0.0020
##    140        0.0882             nan     0.1000    0.0016
##    150        0.0767             nan     0.1000    0.0012
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.2511
##      2        1.4442             nan     0.1000    0.1694
##      3        1.3316             nan     0.1000    0.1356
##      4        1.2413             nan     0.1000    0.1246
##      5        1.1622             nan     0.1000    0.1013
##      6        1.0967             nan     0.1000    0.0907
##      7        1.0363             nan     0.1000    0.0880
##      8        0.9802             nan     0.1000    0.0669
##      9        0.9366             nan     0.1000    0.0706
##     10        0.8915             nan     0.1000    0.0790
##     20        0.5681             nan     0.1000    0.0332
##     40        0.3059             nan     0.1000    0.0134
##     60        0.1765             nan     0.1000    0.0070
##     80        0.1091             nan     0.1000    0.0032
##    100        0.0703             nan     0.1000    0.0025
##    120        0.0489             nan     0.1000    0.0007
##    140        0.0346             nan     0.1000    0.0013
##    150        0.0296             nan     0.1000    0.0006
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1317
##      2        1.5231             nan     0.1000    0.0906
##      3        1.4642             nan     0.1000    0.0642
##      4        1.4211             nan     0.1000    0.0562
##      5        1.3841             nan     0.1000    0.0539
##      6        1.3498             nan     0.1000    0.0448
##      7        1.3192             nan     0.1000    0.0456
##      8        1.2897             nan     0.1000    0.0441
##      9        1.2614             nan     0.1000    0.0373
##     10        1.2376             nan     0.1000    0.0322
##     20        1.0521             nan     0.1000    0.0191
##     40        0.8366             nan     0.1000    0.0110
##     60        0.6980             nan     0.1000    0.0088
##     80        0.5931             nan     0.1000    0.0052
##    100        0.5157             nan     0.1000    0.0050
##    120        0.4485             nan     0.1000    0.0029
##    140        0.3966             nan     0.1000    0.0029
##    150        0.3754             nan     0.1000    0.0027
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1944
##      2        1.4840             nan     0.1000    0.1374
##      3        1.3917             nan     0.1000    0.1162
##      4        1.3147             nan     0.1000    0.0910
##      5        1.2553             nan     0.1000    0.0924
##      6        1.1985             nan     0.1000    0.0798
##      7        1.1484             nan     0.1000    0.0732
##      8        1.1007             nan     0.1000    0.0649
##      9        1.0581             nan     0.1000    0.0496
##     10        1.0245             nan     0.1000    0.0451
##     20        0.7505             nan     0.1000    0.0284
##     40        0.4722             nan     0.1000    0.0148
##     60        0.3181             nan     0.1000    0.0096
##     80        0.2224             nan     0.1000    0.0042
##    100        0.1599             nan     0.1000    0.0022
##    120        0.1227             nan     0.1000    0.0022
##    140        0.0940             nan     0.1000    0.0016
##    150        0.0827             nan     0.1000    0.0014
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.2514
##      2        1.4467             nan     0.1000    0.1935
##      3        1.3247             nan     0.1000    0.1592
##      4        1.2245             nan     0.1000    0.1169
##      5        1.1500             nan     0.1000    0.1101
##      6        1.0811             nan     0.1000    0.1013
##      7        1.0172             nan     0.1000    0.0690
##      8        0.9710             nan     0.1000    0.0753
##      9        0.9235             nan     0.1000    0.0769
##     10        0.8752             nan     0.1000    0.0779
##     20        0.5604             nan     0.1000    0.0272
##     40        0.2914             nan     0.1000    0.0110
##     60        0.1671             nan     0.1000    0.0049
##     80        0.1054             nan     0.1000    0.0034
##    100        0.0723             nan     0.1000    0.0016
##    120        0.0519             nan     0.1000    0.0007
##    140        0.0384             nan     0.1000    0.0007
##    150        0.0327             nan     0.1000    0.0006
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1284
##      2        1.5219             nan     0.1000    0.0918
##      3        1.4622             nan     0.1000    0.0668
##      4        1.4172             nan     0.1000    0.0602
##      5        1.3774             nan     0.1000    0.0500
##      6        1.3450             nan     0.1000    0.0414
##      7        1.3157             nan     0.1000    0.0425
##      8        1.2886             nan     0.1000    0.0362
##      9        1.2637             nan     0.1000    0.0364
##     10        1.2400             nan     0.1000    0.0334
##     20        1.0517             nan     0.1000    0.0174
##     40        0.8316             nan     0.1000    0.0100
##     60        0.6907             nan     0.1000    0.0065
##     80        0.5922             nan     0.1000    0.0080
##    100        0.5096             nan     0.1000    0.0045
##    120        0.4459             nan     0.1000    0.0027
##    140        0.3948             nan     0.1000    0.0036
##    150        0.3741             nan     0.1000    0.0025
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1957
##      2        1.4828             nan     0.1000    0.1286
##      3        1.3957             nan     0.1000    0.1100
##      4        1.3205             nan     0.1000    0.0925
##      5        1.2609             nan     0.1000    0.0866
##      6        1.2030             nan     0.1000    0.0730
##      7        1.1561             nan     0.1000    0.0790
##      8        1.1052             nan     0.1000    0.0623
##      9        1.0638             nan     0.1000    0.0580
##     10        1.0272             nan     0.1000    0.0631
##     20        0.7611             nan     0.1000    0.0274
##     40        0.4737             nan     0.1000    0.0132
##     60        0.3212             nan     0.1000    0.0086
##     80        0.2226             nan     0.1000    0.0051
##    100        0.1569             nan     0.1000    0.0029
##    120        0.1157             nan     0.1000    0.0018
##    140        0.0889             nan     0.1000    0.0020
##    150        0.0779             nan     0.1000    0.0017
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.2567
##      2        1.4452             nan     0.1000    0.1860
##      3        1.3261             nan     0.1000    0.1366
##      4        1.2381             nan     0.1000    0.1314
##      5        1.1550             nan     0.1000    0.1046
##      6        1.0874             nan     0.1000    0.0823
##      7        1.0320             nan     0.1000    0.0897
##      8        0.9763             nan     0.1000    0.0802
##      9        0.9251             nan     0.1000    0.0684
##     10        0.8809             nan     0.1000    0.0605
##     20        0.5717             nan     0.1000    0.0367
##     40        0.2949             nan     0.1000    0.0156
##     60        0.1657             nan     0.1000    0.0067
##     80        0.1039             nan     0.1000    0.0024
##    100        0.0695             nan     0.1000    0.0017
##    120        0.0480             nan     0.1000    0.0016
##    140        0.0339             nan     0.1000    0.0009
##    150        0.0290             nan     0.1000    0.0009
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1202
##      2        1.5244             nan     0.1000    0.0853
##      3        1.4662             nan     0.1000    0.0692
##      4        1.4200             nan     0.1000    0.0592
##      5        1.3828             nan     0.1000    0.0421
##      6        1.3534             nan     0.1000    0.0505
##      7        1.3219             nan     0.1000    0.0409
##      8        1.2947             nan     0.1000    0.0332
##      9        1.2714             nan     0.1000    0.0432
##     10        1.2419             nan     0.1000    0.0315
##     20        1.0559             nan     0.1000    0.0190
##     40        0.8365             nan     0.1000    0.0109
##     60        0.6988             nan     0.1000    0.0089
##     80        0.5913             nan     0.1000    0.0046
##    100        0.5140             nan     0.1000    0.0035
##    120        0.4495             nan     0.1000    0.0033
##    140        0.4021             nan     0.1000    0.0025
##    150        0.3795             nan     0.1000    0.0016
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.1997
##      2        1.4814             nan     0.1000    0.1335
##      3        1.3931             nan     0.1000    0.1138
##      4        1.3202             nan     0.1000    0.0912
##      5        1.2594             nan     0.1000    0.0883
##      6        1.2033             nan     0.1000    0.0783
##      7        1.1529             nan     0.1000    0.0820
##      8        1.1014             nan     0.1000    0.0607
##      9        1.0633             nan     0.1000    0.0516
##     10        1.0295             nan     0.1000    0.0670
##     20        0.7579             nan     0.1000    0.0238
##     40        0.4724             nan     0.1000    0.0148
##     60        0.3110             nan     0.1000    0.0073
##     80        0.2162             nan     0.1000    0.0050
##    100        0.1561             nan     0.1000    0.0044
##    120        0.1142             nan     0.1000    0.0014
##    140        0.0870             nan     0.1000    0.0016
##    150        0.0759             nan     0.1000    0.0011
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.2286
##      2        1.4555             nan     0.1000    0.1808
##      3        1.3367             nan     0.1000    0.1507
##      4        1.2391             nan     0.1000    0.1240
##      5        1.1584             nan     0.1000    0.0980
##      6        1.0940             nan     0.1000    0.0893
##      7        1.0364             nan     0.1000    0.1003
##      8        0.9753             nan     0.1000    0.0858
##      9        0.9215             nan     0.1000    0.0647
##     10        0.8812             nan     0.1000    0.0654
##     20        0.5724             nan     0.1000    0.0257
##     40        0.2991             nan     0.1000    0.0125
##     60        0.1715             nan     0.1000    0.0056
##     80        0.1037             nan     0.1000    0.0034
##    100        0.0664             nan     0.1000    0.0018
##    120        0.0447             nan     0.1000    0.0007
##    140        0.0319             nan     0.1000    0.0006
##    150        0.0271             nan     0.1000    0.0006
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        1.6094             nan     0.1000    0.2553
##      2        1.4464             nan     0.1000    0.1707
##      3        1.3333             nan     0.1000    0.1387
##      4        1.2432             nan     0.1000    0.1228
##      5        1.1639             nan     0.1000    0.1139
##      6        1.0932             nan     0.1000    0.0902
##      7        1.0325             nan     0.1000    0.0841
##      8        0.9784             nan     0.1000    0.0782
##      9        0.9273             nan     0.1000    0.0694
##     10        0.8822             nan     0.1000    0.0616
##     20        0.5828             nan     0.1000    0.0274
##     40        0.3025             nan     0.1000    0.0131
##     60        0.1731             nan     0.1000    0.0095
##     80        0.1101             nan     0.1000    0.0039
##    100        0.0748             nan     0.1000    0.0011
##    120        0.0533             nan     0.1000    0.0013
##    140        0.0396             nan     0.1000    0.0007
##    150        0.0339             nan     0.1000    0.0005
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
##          A 3904    6    0    0    0
##          B    1 2643    6    0    0
##          C    0    7 2374   10    0
##          D    1    1   15 2234    2
##          E    0    0    0    7 2522
## 
## Overall Statistics
##                                           
##                Accuracy : 0.9959          
##                  95% CI : (0.9947, 0.9969)
##     No Information Rate : 0.2844          
##     P-Value [Acc > NIR] : < 2.2e-16       
##                                           
##                   Kappa : 0.9948          
##  Mcnemar's Test P-Value : NA              
## 
## Statistics by Class:
## 
##                      Class: A Class: B Class: C Class: D Class: E
## Sensitivity            0.9995   0.9947   0.9912   0.9924   0.9992
## Specificity            0.9994   0.9994   0.9985   0.9983   0.9994
## Pos Pred Value         0.9985   0.9974   0.9929   0.9916   0.9972
## Neg Pred Value         0.9998   0.9987   0.9981   0.9985   0.9998
## Prevalence             0.2844   0.1935   0.1744   0.1639   0.1838
## Detection Rate         0.2843   0.1925   0.1729   0.1627   0.1836
## Detection Prevalence   0.2847   0.1930   0.1741   0.1641   0.1842
## Balanced Accuracy      0.9994   0.9970   0.9949   0.9954   0.9993
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
##          A 3554  300    3    0    0
##          B  312 1972  239    1    0
##          C   39  371 2050  230   10
##          D    0   14  100 1861  231
##          E    1    0    3  159 2283
## 
## Overall Statistics
##                                           
##                Accuracy : 0.8534          
##                  95% CI : (0.8474, 0.8593)
##     No Information Rate : 0.2844          
##     P-Value [Acc > NIR] : < 2.2e-16       
##                                           
##                   Kappa : 0.8147          
##  Mcnemar's Test P-Value : NA              
## 
## Statistics by Class:
## 
##                      Class: A Class: B Class: C Class: D Class: E
## Sensitivity            0.9099   0.7422   0.8559   0.8267   0.9045
## Specificity            0.9692   0.9502   0.9427   0.9700   0.9855
## Pos Pred Value         0.9214   0.7813   0.7593   0.8436   0.9334
## Neg Pred Value         0.9644   0.9389   0.9687   0.9662   0.9786
## Prevalence             0.2844   0.1935   0.1744   0.1639   0.1838
## Detection Rate         0.2588   0.1436   0.1493   0.1355   0.1662
## Detection Prevalence   0.2809   0.1838   0.1966   0.1606   0.1781
## Balanced Accuracy      0.9395   0.8462   0.8993   0.8983   0.9450
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
##          A 3904    6    0    0    0
##          B    1 2643    6    0    0
##          C    0    7 2374   10    0
##          D    1    1   15 2234    2
##          E    0    0    0    7 2522
## 
## Overall Statistics
##                                           
##                Accuracy : 0.9959          
##                  95% CI : (0.9947, 0.9969)
##     No Information Rate : 0.2844          
##     P-Value [Acc > NIR] : < 2.2e-16       
##                                           
##                   Kappa : 0.9948          
##  Mcnemar's Test P-Value : NA              
## 
## Statistics by Class:
## 
##                      Class: A Class: B Class: C Class: D Class: E
## Sensitivity            0.9995   0.9947   0.9912   0.9924   0.9992
## Specificity            0.9994   0.9994   0.9985   0.9983   0.9994
## Pos Pred Value         0.9985   0.9974   0.9929   0.9916   0.9972
## Neg Pred Value         0.9998   0.9987   0.9981   0.9985   0.9998
## Prevalence             0.2844   0.1935   0.1744   0.1639   0.1838
## Detection Rate         0.2843   0.1925   0.1729   0.1627   0.1836
## Detection Prevalence   0.2847   0.1930   0.1741   0.1641   0.1842
## Balanced Accuracy      0.9994   0.9970   0.9949   0.9954   0.9993
```

The accuracy of the combined model is only slightly higher.

## Evaluate the final model (GBM) on the testing dataset


```r
resultGBM  <- predict(modelGBM, testing)
resultGBM
```

```
##  [1] B A B A A E D B A A B C B A E E A B B B
## Levels: A B C D E
```

