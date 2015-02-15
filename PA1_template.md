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
New dataset with the missing data filled in.

```r
activity_new <- activity
activity_new[is.na(activity$steps), ]$steps <- 0
```
Make a histogram of the total number of steps taken each day and Calculate and report the mean and median total number of steps taken per day. 

```r
t_steps_by_day_new <- tapply(activity_new$steps, activity_new$date, FUN=sum)
hist(t_steps_by_day_new, 
     xlab = "total number of steps taken per day", 
     main = "Histogram of total number of steps taken per day (new)")
```

![](PA1_template_files/figure-html/unnamed-chunk-6-1.png) 

```r
summary(t_steps_by_day_new)
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##       0    6778   10400    9354   12810   21190
```
Do these values differ from the estimates from the first part of the assignment? Yes

What is the impact of imputing missing data on the estimates of the total daily number of steps?


## Are there differences in activity patterns between weekdays and weekends?

```r
Sys.setlocale("LC_TIME", "en_US.UTF-8")
```

```
## [1] "en_US.UTF-8"
```

```r
activity_new$isWeekday <- as.factor(ifelse(weekdays(activity_new$date) %in% c("Saturday", "Sunday"), "Weekday", "Weekends"))
weekday <- activity_new[activity_new$isWeekday=="Weekday", ]
weekends <- activity_new[activity_new$isWeekday=="Weekends", ]
plot(aggregate(weekday$steps, by=list(weekday$interval), FUN=mean, na.rm=TRUE),
     type="l", main = "Weekday average daily activity pattern", ylab = "steps", xlab = "interval")
```

![](PA1_template_files/figure-html/unnamed-chunk-7-1.png) 

```r
plot(aggregate(weekends$steps, by=list(weekends$interval), FUN=mean, na.rm=TRUE),
     type="l", main = "Weekends average daily activity pattern", ylab = "steps", xlab = "interval")
```

![](PA1_template_files/figure-html/unnamed-chunk-7-2.png) 

