# Reproducible Research: Peer Assessment 1


### Loading and preprocessing the data

### get the data from the activity.csv file and set to data frame

```r
#assumes in working directory
setwd("C://Users//Jim//Documents//Coursera//Reproducible_Research//RepData_PeerAssessment1")
DataActTemp <- read.csv("activity.csv",as.is=TRUE)
DataAct <- na.omit(DataActTemp)
```

## What is mean total number of steps taken per day?


```r
## 1. Calculate the total number of steps taken per day

#split the file by Date
DataActSplit <- split(DataAct,DataAct$date)

# initialize vectors for use in the loop below
d <- character()  #used for total steps
tS <- numeric()
mnS <- numeric()
mdS <- numeric()
Tdf <- data.frame()

# now loop through each date to get the total number of steps taken per day
# now loop through all the separate data frames produces by split
for (i in 1:length(DataActSplit)) {
        
        #remove all na's
        GoDAS <- DataActSplit[[i]]
        Tsteps <- sum(GoDAS[,1]) #sum first column (i.e. steps)
        MnSteps <- mean(GoDAS[,1]) #mean steps
        MdSteps <- median(GoDAS[,1]) #median steps
        
        # put these in the variables set previously
        d[i] <- GoDAS[1,2]
        tS[i] <- Tsteps #ts is a vector containing the total steps for each day              
        mnS[i] <- MnSteps
        mdS[i] <- MdSteps
        Tdf[i,1] <- c(d[i])
        Tdf[i,2] <- c(mnS[i])
        Tdf[i,3] <- c(mdS[i])
}

##2. Make a histogram of the total number of steps taken each day

# producing the histogram
hist(tS,col="red",xlab="steps",ylab="days",main="Steps per Day",breaks=20)
```

![](./PA1_template_files/figure-html/unnamed-chunk-2-1.png) 

```r
# get 5-min internal 
# to be used 
DataInts <- split(DataAct,DataAct$interval)
mnI <- numeric()
Ints <- numeric()
for (i in 1:length(DataInts)) {
        TempInt <- DataInts[[i]]
        IntMean <- mean(TempInt[,1])
        Ints[i] <- TempInt[1,3]
        mnI[i] <- IntMean
}

##3. Calculate and report the mean and median of the total number of steps taken per day

# create a data frame with all the average steps across all days for each time interval
newInts <- data.frame(interval=Ints,avg_steps=mnI)
```

Histogram of steps taken each day

```r
hist(tS,col="red",xlab="steps",ylab="days",main="Steps per Day",breaks=20)
```

![](./PA1_template_files/figure-html/histogram-1.png) 

The mean and median of the total number of steps taken per day 

```r
print(Tdf,type="html")
```

           V1         V2 V3
1  2012-10-02  0.4375000  0
2  2012-10-03 39.4166667  0
3  2012-10-04 42.0694444  0
4  2012-10-05 46.1597222  0
5  2012-10-06 53.5416667  0
6  2012-10-07 38.2465278  0
7  2012-10-09 44.4826389  0
8  2012-10-10 34.3750000  0
9  2012-10-11 35.7777778  0
10 2012-10-12 60.3541667  0
11 2012-10-13 43.1458333  0
12 2012-10-14 52.4236111  0
13 2012-10-15 35.2048611  0
14 2012-10-16 52.3750000  0
15 2012-10-17 46.7083333  0
16 2012-10-18 34.9166667  0
17 2012-10-19 41.0729167  0
18 2012-10-20 36.0937500  0
19 2012-10-21 30.6284722  0
20 2012-10-22 46.7361111  0
21 2012-10-23 30.9652778  0
22 2012-10-24 29.0104167  0
23 2012-10-25  8.6527778  0
24 2012-10-26 23.5347222  0
25 2012-10-27 35.1354167  0
26 2012-10-28 39.7847222  0
27 2012-10-29 17.4236111  0
28 2012-10-30 34.0937500  0
29 2012-10-31 53.5208333  0
30 2012-11-02 36.8055556  0
31 2012-11-03 36.7048611  0
32 2012-11-05 36.2465278  0
33 2012-11-06 28.9375000  0
34 2012-11-07 44.7326389  0
35 2012-11-08 11.1770833  0
36 2012-11-11 43.7777778  0
37 2012-11-12 37.3784722  0
38 2012-11-13 25.4722222  0
39 2012-11-15  0.1423611  0
40 2012-11-16 18.8923611  0
41 2012-11-17 49.7881944  0
42 2012-11-18 52.4652778  0
43 2012-11-19 30.6979167  0
44 2012-11-20 15.5277778  0
45 2012-11-21 44.3993056  0
46 2012-11-22 70.9270833  0
47 2012-11-23 73.5902778  0
48 2012-11-24 50.2708333  0
49 2012-11-25 41.0902778  0
50 2012-11-26 38.7569444  0
51 2012-11-27 47.3819444  0
52 2012-11-28 35.3576389  0
53 2012-11-29 24.4687500  0


## What is the average daily activity pattern?


```r
##1. Make a time series plot (i.e. type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all days (y-axis)

## built time series plot using ggplot library
## load the library
library("ggplot2")
```

```
## Warning: package 'ggplot2' was built under R version 3.2.2
```

```r
# create the plot
g <- ggplot(newInts,aes(interval,avg_steps)) + geom_line() + labs(x="5-minute interval",y="avg steps taken",title="Average Daily Activity Pattern")

#print it out
#print(g)

## 2. Which 5-minute interval, on average across all the days in the dataset, contains the maximum number of steps?

# determine do this by ordering the newInts dataframe in descending order of the average steps taken

yyy <- newInts[order(-mnI),]
# the first row will contain the max avg_steps taken
# get the interval containing the max value
maxInt <- yyy[1,1]
```

Time series plot of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all days (y-axis)

```r
print(g)
```

![](./PA1_template_files/figure-html/timeseries-1.png) 

The 5-minute interval, on average across all the days in the dataset, containing the maximum number of steps is `maxInt`

## Imputing missing values

```r
#1. Calculate and report the total number of missing values in the dataset (i.e. the total number of rows with NAs)
# calculate this using complete.cases

ok <- complete.cases(DataActTemp)
numNArows <- sum(!ok)

##2. Devise a strategy for filling in all of the missing values in the dataset. The strategy does not need to be sophisticated. For example, you could use the mean/median for that day, or the mean for that 5-minute interval, etc.

# loop through the intitial data set and set temp variables
newDataSteps <- numeric()
for (i in 1:nrow(DataActTemp)) {
        tSteps <- DataActTemp[i,1]
        tInt <- DataActTemp[i,3]
        if (is.na(tSteps)) {
                IntRow <- subset(newInts,newInts[,1]==tInt)
                #newDataSteps[i] <- newInts[IntRow,2]
                newDataSteps[i] <- IntRow[,2]
        } else {
        newDataSteps[i] <- DataActTemp[i,1]
        }
}

##3. Create a new dataset that is equal to the original dataset but with the missing data filled in.
NewDataAct <- data.frame(steps=NewDataSteps,date=DataActTemp[,2],interval=DataActTemp[,3])


##4. Make a histogram of the total number of steps taken each day and Calculate and report the mean and median total number of steps taken per day. Do these values differ from the estimates from the first part of the assignment? What is the impact of imputing missing data on the estimates of the total daily number of steps?
```

## Are there differences in activity patterns between weekdays and weekends?


