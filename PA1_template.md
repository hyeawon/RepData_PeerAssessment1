---
title: "repdata_data_activity"
author: "Hyeawon Chae"
date: "12/6/2021"
output: html_document
---

```{r}
library(ggplot2)

setwd("/Users/hyeaw/Desktop/Coursera/Reproducible Research/week 2/repdata_data_activity")
###1. reading in the dataset and/or processing the data

repdata<- read.csv(file = "activity.csv")

###2. Histogram of the total number of steps taken each day steps.

repdata$date <- as.Date(repdata$date)
newdata<-na.omit(repdata) 
groupday<-group_by(newdata, date)
stepsbydays<-summarize(groupday, steps = sum(steps))
hist(stepsbydays$steps, main = "Total number of steps taken per day", xlab = "Total number of steps")

###3. Mean and median number of steps taken each day

summary(stepsbydays$steps)

###4. Time series plot of the average number of steps taken

groupinterval<-group_by(newdata, interval)
stepsbyinterval<-summarize(groupinterval, steps = mean(steps))
plot(stepsbyinterval$interval, stepsbyinterval$steps, type="l", main = "Average number of steps taken per day",xlab = "interval", ylab = "Average number of steps")

###5. The 5-minute interval that, on average, contains the maximum number of steps

groupinterval<-group_by(newdata, interval)
plot(stepsbyinterval$interval, stepsbyinterval$steps, type="l", main = "Average number of steps taken per day",xlab = "interval", ylab = "Average number of steps")

###6. Code to describe and show a strategy for imputing missing data

repdata1 <- repdata %>% replace(is.na(.), 0)
IncludeNAgroupday<-group_by(repdata1, date)
includeNAstepsbydays<-summarize(IncludeNAgroupday, steps = sum(steps))

summary(includeNAstepsbydays$steps)
 
###7. Histogram of the total number of steps taken each day after missing values are imputed

hist(includeNAstepsbydays$steps, main = "Total number of steps taken per day with NA", xlab = "Total number of steps")

###8. Panel plot comparing the average number of steps taken per 5-minute interval across weekdays and weekends

repdata1$days <- weekdays(repdata1$date)
weekdays<-c('Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday')
repdata1$wDay <- factor((weekdays(repdata1$date) %in% weekdays), 
                    levels=c(FALSE, TRUE), labels=c('weekend', 'weekday'))
averagedincludeNAgroupdate <- aggregate(steps ~ interval + wDay, data= includeNAgroupdate, mean)
ggplot(averagedincludeNAgroupdate, aes(interval, steps))+geom_line()+facet_grid(wDay ~ .)+labs(title="Interval vs average steps compare Weekend and Weekdays", x="Interval", y="Average Number of Steps")


```

