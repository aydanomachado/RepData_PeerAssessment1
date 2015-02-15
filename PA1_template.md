# Reproducible Research: Peer Assessment 1



## Loading and preprocessing the data

```r
activity <- read.csv("activity.csv", colClasses = c("integer","Date","integer"))
```


## What is mean total number of steps taken per day?

```r
t_steps_by_day <- tapply(activity$steps, activity$date, FUN=sum)
hist(t_steps_by_day, 
     xlab = "total number of steps taken per day", 
     main = "Histogram of total number of steps taken per day")
```

![](PA1_template_files/figure-html/unnamed-chunk-2-1.png) 

```r
summary(t_steps_by_day)
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
##      41    8841   10760   10770   13290   21190       8
```



## What is the average daily activity pattern?

```r
plot(aggregate(activity$steps, by=list(activity$interval), FUN=mean, na.rm=TRUE),
     type="l", main = "Average daily activity pattern", ylab = "steps", xlab = "interval")
```

![](PA1_template_files/figure-html/unnamed-chunk-3-1.png) 

## Imputing missing values
Total number of rows with NAs

```r
sum(is.na(activity$steps))
```

```
## [1] 2304
```


## Are there differences in activity patterns between weekdays and weekends?
