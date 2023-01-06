---
title: "Reproducible Research: Peer Assessment 1"
output: 
  html_document:
    keep_md: true
---

## Loading required packages
Using require() we will load all packages needed for analysis.

```r
require(readr)
```

```
## Loading required package: readr
```

```
## Warning: package 'readr' was built under R version 4.2.2
```

```r
require(ggplot2)
```

```
## Loading required package: ggplot2
```

```
## Warning: package 'ggplot2' was built under R version 4.2.2
```

```r
require(dplyr)
```

```
## Loading required package: dplyr
```

```
## 
## Attaching package: 'dplyr'
```

```
## The following objects are masked from 'package:stats':
## 
##     filter, lag
```

```
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
```

```r
require(lubridate)
```

```
## Loading required package: lubridate
```

```
## Warning: package 'lubridate' was built under R version 4.2.2
```

```
## Loading required package: timechange
```

```
## Warning: package 'timechange' was built under R version 4.2.2
```

```
## 
## Attaching package: 'lubridate'
```

```
## The following objects are masked from 'package:base':
## 
##     date, intersect, setdiff, union
```

## Loading and preprocessing the data
The data is loaded into a data frame. With readr, most of the data is already in an appropriate format.  

```r
stepsdf <- read_csv("activity.zip")
```

```
## Rows: 17568 Columns: 3
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## dbl  (2): steps, interval
## date (1): date
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```
Stepsdf contains three columns:
1. steps (numeric), the number of steps on the given date and time. 
2. date (date), the date on which the measurement was taken.
3. interval (numeric), the time in the format HHMM  
The interval column is used to extract the time, and this is combined with the date to create a datetime value.

```r
stepsdf %>%
    mutate(datetime = date + hours((interval - (interval %% 100)) / 100) + minutes(interval %% 100))
```

```
## # A tibble: 17,568 × 4
##    steps date       interval datetime           
##    <dbl> <date>        <dbl> <dttm>             
##  1    NA 2012-10-01        0 2012-10-01 00:00:00
##  2    NA 2012-10-01        5 2012-10-01 00:05:00
##  3    NA 2012-10-01       10 2012-10-01 00:10:00
##  4    NA 2012-10-01       15 2012-10-01 00:15:00
##  5    NA 2012-10-01       20 2012-10-01 00:20:00
##  6    NA 2012-10-01       25 2012-10-01 00:25:00
##  7    NA 2012-10-01       30 2012-10-01 00:30:00
##  8    NA 2012-10-01       35 2012-10-01 00:35:00
##  9    NA 2012-10-01       40 2012-10-01 00:40:00
## 10    NA 2012-10-01       45 2012-10-01 00:45:00
## # … with 17,558 more rows
```

## What is mean total number of steps taken per day?
First we sum all values of 'steps', grouped by day, using the 'aggregate' function:

```r
steps_per_day <- aggregate(stepsdf$steps, by=list(stepsdf$date), sum)
names(steps_per_day) <- c("Date", "Total_steps")
```
Steps_per_day now contains two columns: a date and the total number of steps taken on that day.  
Now we will use ggplot to create a histogram:  

```r
g <- ggplot(steps_per_day, aes(x = Total_steps))
g + geom_histogram(binwidth = 1000, color = 'black', fill = 'white') + labs(title = 'Total number of steps per day', x = 'Number of steps taken', y = 'Frequency')
```

```
## Warning: Removed 8 rows containing non-finite values (`stat_bin()`).
```

![](PA1_template_files/figure-html/unnamed-chunk-5-1.png)<!-- -->
  
Next, we will calculate the mean and median number of total steps taken per day (so the sum of all steps taken daily). 

```r
cat('Mean:   ', mean(steps_per_day$Total_steps, na.rm = TRUE), "\n")
```

```
## Mean:    10766.19
```

```r
cat('Median: ', median(steps_per_day$Total_steps, na.rm = TRUE))
```

```
## Median:  10765
```

## What is the average daily activity pattern?



## Imputing missing values



## Are there differences in activity patterns between weekdays and weekends?
