# Reproducible Research: Peer Assessment 1


## Loading and preprocessing the data
This code chunk loads the required libraries, reads the data from the .csv file and converts the date variable to class "Date".

```r
library(dplyr,quietly = T)
library(lubridate,quietly = T)
library(zoo,quietly = T)
library(lattice,quietly = T)
data <- read.csv(file="activity.csv")
data <- mutate(data,date=ymd(date))
```
## What is mean total number of steps taken per day?
First the data is grouped by date in order to use the summarise function to calculate the mean for each unique date. This data is used for the histogram. Subsequently the total mean and median are calculated. The data is plotted with the "plotly" package.

```r
by_date <- data %>% group_by(date)
steps_per_day <- by_date %>% summarise(Steps=sum(steps,na.rm=T))
plotly::plot_ly(steps_per_day,x=~Steps,type = "histogram",nbinsx=length(steps_per_day$date))
```

<!--html_preserve--><div id="htmlwidget-f152a717caf3d34acfd3" style="width:672px;height:480px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-f152a717caf3d34acfd3">{"x":{"layout":{"margin":{"b":40,"l":60,"t":25,"r":10},"xaxis":{"domain":[0,1],"title":"Steps"},"yaxis":{"domain":[0,1]}},"source":"A","config":{"modeBarButtonsToAdd":[{"name":"Collaborate","icon":{"width":1000,"ascent":500,"descent":-50,"path":"M487 375c7-10 9-23 5-36l-79-259c-3-12-11-23-22-31-11-8-22-12-35-12l-263 0c-15 0-29 5-43 15-13 10-23 23-28 37-5 13-5 25-1 37 0 0 0 3 1 7 1 5 1 8 1 11 0 2 0 4-1 6 0 3-1 5-1 6 1 2 2 4 3 6 1 2 2 4 4 6 2 3 4 5 5 7 5 7 9 16 13 26 4 10 7 19 9 26 0 2 0 5 0 9-1 4-1 6 0 8 0 2 2 5 4 8 3 3 5 5 5 7 4 6 8 15 12 26 4 11 7 19 7 26 1 1 0 4 0 9-1 4-1 7 0 8 1 2 3 5 6 8 4 4 6 6 6 7 4 5 8 13 13 24 4 11 7 20 7 28 1 1 0 4 0 7-1 3-1 6-1 7 0 2 1 4 3 6 1 1 3 4 5 6 2 3 3 5 5 6 1 2 3 5 4 9 2 3 3 7 5 10 1 3 2 6 4 10 2 4 4 7 6 9 2 3 4 5 7 7 3 2 7 3 11 3 3 0 8 0 13-1l0-1c7 2 12 2 14 2l218 0c14 0 25-5 32-16 8-10 10-23 6-37l-79-259c-7-22-13-37-20-43-7-7-19-10-37-10l-248 0c-5 0-9-2-11-5-2-3-2-7 0-12 4-13 18-20 41-20l264 0c5 0 10 2 16 5 5 3 8 6 10 11l85 282c2 5 2 10 2 17 7-3 13-7 17-13z m-304 0c-1-3-1-5 0-7 1-1 3-2 6-2l174 0c2 0 4 1 7 2 2 2 4 4 5 7l6 18c0 3 0 5-1 7-1 1-3 2-6 2l-173 0c-3 0-5-1-8-2-2-2-4-4-4-7z m-24-73c-1-3-1-5 0-7 2-2 3-2 6-2l174 0c2 0 5 0 7 2 3 2 4 4 5 7l6 18c1 2 0 5-1 6-1 2-3 3-5 3l-174 0c-3 0-5-1-7-3-3-1-4-4-5-6z"},"click":"function(gd) { \n        // is this being viewed in RStudio?\n        if (location.search == '?viewer_pane=1') {\n          alert('To learn about plotly for collaboration, visit:\\n https://cpsievert.github.io/plotly_book/plot-ly-for-collaboration.html');\n        } else {\n          window.open('https://cpsievert.github.io/plotly_book/plot-ly-for-collaboration.html', '_blank');\n        }\n      }"}],"modeBarButtonsToRemove":["sendDataToCloud"]},"data":[{"x":[0,126,11352,12116,13294,15420,11015,0,12811,9900,10304,17382,12426,15098,10139,15084,13452,10056,11829,10395,8821,13460,8918,8355,2492,6778,10119,11458,5018,9819,15414,0,10600,10571,0,10439,8334,12883,3219,0,0,12608,10765,7336,0,41,5441,14339,15110,8841,4472,12787,20427,21194,14478,11834,11162,13646,10183,7047,0],"nbinsx":61,"type":"histogram","marker":{"fillcolor":"rgba(31,119,180,1)","color":"rgba(31,119,180,1)","line":{"color":"transparent"}},"xaxis":"x","yaxis":"y"}],"base_url":"https://plot.ly"},"evals":["config.modeBarButtonsToAdd.0.click"],"jsHooks":[]}</script><!--/html_preserve-->

```r
mean_steps_per_day <- mean(steps_per_day$Steps,na.rm = T)
median_steps_per_day <- median(steps_per_day$Steps,na.rm = T)
cat(" The mean number of steps per day= ",mean_steps_per_day,"\n","The median number of steps per day= ",median_steps_per_day,"\n")
```

```
##  The mean number of steps per day=  9354.23 
##  The median number of steps per day=  10395
```
## What is the average daily activity pattern?
Similar to the previous code chunk, first the data is grouped by interval, then the mean for each unique interval value is calculated. The data is plotted with the "base" package.

```r
by_interval <- data %>% group_by(interval)
steps_per_int <- by_interval %>% summarise(Steps=sum(steps,na.rm=T))
plot(steps_per_int$interval,steps_per_int$Steps, type = "l",xlab = "Interval", ylab = "Steps")
```

![](PA1_template_files/figure-html/unnamed-chunk-3-1.png)<!-- -->

```r
max_interval <- steps_per_int$interval[which.max(steps_per_int$Steps)]
cat("The interval with the highest average step count= interval #",max_interval)
```

```
## The interval with the highest average step count= interval # 835
```
## Imputing missing values
the strategy used for imupting the missing data is to use linear interpolation. The approx function provides for linear interpolation and the "na.approx" function is a wrapper around "approx" from the package "zoo". Each missing value (or sequence of missing values) is interpolated between the previous valid sample and the next valid sample. Consequently, the strategy does not work for leading and trailing NA values in the dataset. These values are droped for the purpose of the analysis, since we are only reporting summary statistics. Replacing these values with the mean, median, etc. would not substantially change these summary statistics, but would reduce the variance (error of each statistic) slightly. The data is plotted with the "plotly" package.

```r
sum(!complete.cases(data))
```

```
## [1] 2304
```

```r
filled <-  na.approx(data$steps,1:17568, na.rm = F)
data2 <- mutate(data,steps=filled)
by_date2 <- data2 %>% group_by(date)
steps_per_day2 <- by_date2 %>% summarise(Steps=sum(steps,na.rm=T))
plotly::plot_ly(steps_per_day2,x=~Steps,type = "histogram",nbinsx=length(steps_per_day2$date))
```

<!--html_preserve--><div id="htmlwidget-2b2a7e2f43d0fa1dfa16" style="width:672px;height:480px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-2b2a7e2f43d0fa1dfa16">{"x":{"layout":{"margin":{"b":40,"l":60,"t":25,"r":10},"xaxis":{"domain":[0,1],"title":"Steps"},"yaxis":{"domain":[0,1]}},"source":"A","config":{"modeBarButtonsToAdd":[{"name":"Collaborate","icon":{"width":1000,"ascent":500,"descent":-50,"path":"M487 375c7-10 9-23 5-36l-79-259c-3-12-11-23-22-31-11-8-22-12-35-12l-263 0c-15 0-29 5-43 15-13 10-23 23-28 37-5 13-5 25-1 37 0 0 0 3 1 7 1 5 1 8 1 11 0 2 0 4-1 6 0 3-1 5-1 6 1 2 2 4 3 6 1 2 2 4 4 6 2 3 4 5 5 7 5 7 9 16 13 26 4 10 7 19 9 26 0 2 0 5 0 9-1 4-1 6 0 8 0 2 2 5 4 8 3 3 5 5 5 7 4 6 8 15 12 26 4 11 7 19 7 26 1 1 0 4 0 9-1 4-1 7 0 8 1 2 3 5 6 8 4 4 6 6 6 7 4 5 8 13 13 24 4 11 7 20 7 28 1 1 0 4 0 7-1 3-1 6-1 7 0 2 1 4 3 6 1 1 3 4 5 6 2 3 3 5 5 6 1 2 3 5 4 9 2 3 3 7 5 10 1 3 2 6 4 10 2 4 4 7 6 9 2 3 4 5 7 7 3 2 7 3 11 3 3 0 8 0 13-1l0-1c7 2 12 2 14 2l218 0c14 0 25-5 32-16 8-10 10-23 6-37l-79-259c-7-22-13-37-20-43-7-7-19-10-37-10l-248 0c-5 0-9-2-11-5-2-3-2-7 0-12 4-13 18-20 41-20l264 0c5 0 10 2 16 5 5 3 8 6 10 11l85 282c2 5 2 10 2 17 7-3 13-7 17-13z m-304 0c-1-3-1-5 0-7 1-1 3-2 6-2l174 0c2 0 4 1 7 2 2 2 4 4 5 7l6 18c0 3 0 5-1 7-1 1-3 2-6 2l-173 0c-3 0-5-1-8-2-2-2-4-4-4-7z m-24-73c-1-3-1-5 0-7 2-2 3-2 6-2l174 0c2 0 5 0 7 2 3 2 4 4 5 7l6 18c1 2 0 5-1 6-1 2-3 3-5 3l-174 0c-3 0-5-1-7-3-3-1-4-4-5-6z"},"click":"function(gd) { \n        // is this being viewed in RStudio?\n        if (location.search == '?viewer_pane=1') {\n          alert('To learn about plotly for collaboration, visit:\\n https://cpsievert.github.io/plotly_book/plot-ly-for-collaboration.html');\n        } else {\n          window.open('https://cpsievert.github.io/plotly_book/plot-ly-for-collaboration.html', '_blank');\n        }\n      }"}],"modeBarButtonsToRemove":["sendDataToCloud"]},"data":[{"x":[0,126,11352,12116,13294,15420,11015,0,12811,9900,10304,17382,12426,15098,10139,15084,13452,10056,11829,10395,8821,13460,8918,8355,2492,6778,10119,11458,5018,9819,15414,0,10600,10571,0,10439,8334,12883,3219,0,0,12608,10765,7336,0,41,5441,14339,15110,8841,4472,12787,20427,21194,14478,11834,11162,13646,10183,7047,0],"nbinsx":61,"type":"histogram","marker":{"fillcolor":"rgba(31,119,180,1)","color":"rgba(31,119,180,1)","line":{"color":"transparent"}},"xaxis":"x","yaxis":"y"}],"base_url":"https://plot.ly"},"evals":["config.modeBarButtonsToAdd.0.click"],"jsHooks":[]}</script><!--/html_preserve-->

```r
mean_steps_per_day<- mean(steps_per_day2$Steps,na.rm = T)
median_steps_per_day<- median(steps_per_day2$Steps,na.rm = T)
cat(" The mean number of steps per day of the imputed dataset= ",mean_steps_per_day,"\n","The median number of steps per day of the imputed dataset= ",median_steps_per_day,"\n")
```

```
##  The mean number of steps per day of the imputed dataset=  9354.23 
##  The median number of steps per day of the imputed dataset=  10395
```

## Are there differences in activity patterns between weekdays and weekends?
Weekdays vs weekends are identified and stored as a factor variable. The imputed data is grouped by both interval and the factor "wkday". Finally the mean is calculated as above and the data is plotted using the lattice package.

```r
wkday <- (weekdays(data2$date)=="Saturday" | weekdays(data2$date)=="Sunday")
data2 <- data2 %>% mutate(wkday=factor(wkday,labels=c("weekday","weekend")))
by_interval_wkday <- data2 %>% group_by(interval,wkday)
steps_per_int2 <- by_interval_wkday %>% summarise(Steps=sum(steps,na.rm=T))
xyplot(Steps ~ interval | wkday, data = steps_per_int2, type="l")
```

![](PA1_template_files/figure-html/unnamed-chunk-5-1.png)<!-- -->
