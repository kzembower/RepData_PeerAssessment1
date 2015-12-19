---
title: "Reproducible Research: Peer Assessment 1"
author: "Kevin Zembower"
output: 
  html_document:
    keep_md: true
---



## Loading and preprocessing the data

The data on personally-recorded activity was loaded from a
comma-delimited file which was stored on the local drive. This file
was downloaded from the forked project from the instructor, and
decompressed using 'unzip' on the command line. The file was loaded
into R with the commands:


```r
df <- read.csv("activity.csv")
```

No further processing was needed to produce the subsequent results.

## What is mean total number of steps taken per day?

The median and mean number of steps taken per day can be produced with these R commands:


```r
tapply(df$steps, df$date, mean)
```

```
## 2012-10-01 2012-10-02 2012-10-03 2012-10-04 2012-10-05 2012-10-06 
##         NA     0.4375    39.4167    42.0694    46.1597    53.5417 
## 2012-10-07 2012-10-08 2012-10-09 2012-10-10 2012-10-11 2012-10-12 
##    38.2465         NA    44.4826    34.3750    35.7778    60.3542 
## 2012-10-13 2012-10-14 2012-10-15 2012-10-16 2012-10-17 2012-10-18 
##    43.1458    52.4236    35.2049    52.3750    46.7083    34.9167 
## 2012-10-19 2012-10-20 2012-10-21 2012-10-22 2012-10-23 2012-10-24 
##    41.0729    36.0938    30.6285    46.7361    30.9653    29.0104 
## 2012-10-25 2012-10-26 2012-10-27 2012-10-28 2012-10-29 2012-10-30 
##     8.6528    23.5347    35.1354    39.7847    17.4236    34.0938 
## 2012-10-31 2012-11-01 2012-11-02 2012-11-03 2012-11-04 2012-11-05 
##    53.5208         NA    36.8056    36.7049         NA    36.2465 
## 2012-11-06 2012-11-07 2012-11-08 2012-11-09 2012-11-10 2012-11-11 
##    28.9375    44.7326    11.1771         NA         NA    43.7778 
## 2012-11-12 2012-11-13 2012-11-14 2012-11-15 2012-11-16 2012-11-17 
##    37.3785    25.4722         NA     0.1424    18.8924    49.7882 
## 2012-11-18 2012-11-19 2012-11-20 2012-11-21 2012-11-22 2012-11-23 
##    52.4653    30.6979    15.5278    44.3993    70.9271    73.5903 
## 2012-11-24 2012-11-25 2012-11-26 2012-11-27 2012-11-28 2012-11-29 
##    50.2708    41.0903    38.7569    47.3819    35.3576    24.4688 
## 2012-11-30 
##         NA
```

```r
tapply(df$steps, df$date, median)
```

```
## 2012-10-01 2012-10-02 2012-10-03 2012-10-04 2012-10-05 2012-10-06 
##         NA          0          0          0          0          0 
## 2012-10-07 2012-10-08 2012-10-09 2012-10-10 2012-10-11 2012-10-12 
##          0         NA          0          0          0          0 
## 2012-10-13 2012-10-14 2012-10-15 2012-10-16 2012-10-17 2012-10-18 
##          0          0          0          0          0          0 
## 2012-10-19 2012-10-20 2012-10-21 2012-10-22 2012-10-23 2012-10-24 
##          0          0          0          0          0          0 
## 2012-10-25 2012-10-26 2012-10-27 2012-10-28 2012-10-29 2012-10-30 
##          0          0          0          0          0          0 
## 2012-10-31 2012-11-01 2012-11-02 2012-11-03 2012-11-04 2012-11-05 
##          0         NA          0          0         NA          0 
## 2012-11-06 2012-11-07 2012-11-08 2012-11-09 2012-11-10 2012-11-11 
##          0          0          0         NA         NA          0 
## 2012-11-12 2012-11-13 2012-11-14 2012-11-15 2012-11-16 2012-11-17 
##          0          0         NA          0          0          0 
## 2012-11-18 2012-11-19 2012-11-20 2012-11-21 2012-11-22 2012-11-23 
##          0          0          0          0          0          0 
## 2012-11-24 2012-11-25 2012-11-26 2012-11-27 2012-11-28 2012-11-29 
##          0          0          0          0          0          0 
## 2012-11-30 
##         NA
```

To draw a histogram of the total number of steps taken per day, we can
execute these R commands:


```r
tot.steps <- tapply(df$steps, df$date, FUN=sum)
barplot(tot.steps)
```

![plot of chunk Barplot_of_Total_Steps_per_Day](figure/Barplot_of_Total_Steps_per_Day-1.png) 

The mean number of steps taken per day is
1.0766 &times; 10<sup>4</sup>
, while the median number of steps taken per day is
10765.

## What is the average daily activity pattern?

A histogram that illustrates the average daily activity pattern can be
generated with:


```r
int.steps <- tapply(df$steps,
                    df$interval,
                    FUN=mean,
                    na.rm=TRUE
                    )

plot(y=int.steps,
     x=names(int.steps),
     type='l',
     main="Average steps per 5-minute interval",
     xlab='Time at beginning of interval',
     ylab='Total steps',
     )
```

![plot of chunk Total_Steps_per_5-minute_interval](figure/Total_Steps_per_5-minute_interval-1.png) 

NOTE: This chunk would normally be hidden (not echoed), as it is just
used to calculate a pleasing display of the maximum interval:


```r
max.int.steps <- max(int.steps)
hours <- round( as.numeric(names(int.steps[int.steps==max.int.steps])) / 100 )
minutes <- as.numeric(names(int.steps[int.steps==max.int.steps])) %% 100
max.int <- sprintf("%02d:%02d", hours, minutes)
```

The maximum number of steps, 206.1698, occurred during the
interval starting at 08:35.


## Imputing missing values

### Calculate the total number of missing values

The total number of missing values of 'steps' in the dataset is
2304.

### Strategy for imputing missing values

My strategy to impute the number of steps is to replace NA's with the
median number of steps for all the days in this interval. I create a
named array of the median of the steps, and replace all NS's with the
median value.


```r
steps.median <- tapply(df$steps, df$interval, FUN=median, na.rm=TRUE)
```

### Create an imputed dataset

These R commands will create a new dataset, `df.impute`, with the same
characteristics, but with missing `step` values replace with the
median number of steps for that interval across all days:


```r
df.impute <- df
df.impute$steps <- ifelse(is.na(df.imput$steps), steps.median, df.impute$steps)
```

```
## Error in ifelse(is.na(df.imput$steps), steps.median, df.impute$steps): object 'df.imput' not found
```

### Create a histogram of the imputed values

This R command will plot a histogram of the imputed value of the
total steps taken per day:


```r
tot.steps.imput <- tapply(df.impute$steps, df.impute$date, FUN=sum)
barplot(tot.steps.imput)
```

![plot of chunk Total_Steps_per_5-minute_interval_imputed](figure/Total_Steps_per_5-minute_interval_imputed-1.png) 
The mean number of _imputed_ steps taken per day is
1.0766 &times; 10<sup>4</sup>
while the mean number of steps taken per day (_not_ imputed) is
1.0766 &times; 10<sup>4</sup>
.

The median number of _imputed_ steps taken per day is
10765,
, while the median number of steps taken per day (_not_ imputed) is
10765.

The impact of imputing data on the mean and median number of steps
taken per day is to _decrease_ both values for the imputed compared to
the non-imputed data.


## Are there differences in activity patterns between weekdays and weekends?
