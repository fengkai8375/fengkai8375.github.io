---
layout: post
title: "Linux Shell 日期和时间操作"
date: 2017-12-13 09:00:00 +0800 
categories: Shell
tag: Shell
---
* content
{:toc}

### 日期和时间

```
#date and time
today=`date +"%y%m%d"`
echo $today
today_big=`date +"%Y-%m-%d %H:%M:%S"`
echo "Today is ${today_big}"
yesterday=`date -d"1 day ago" +"%Y%m%d"` # `date -d"-1 day" +"%Y%m%d"`
echo "Yestoday is ${yesterday}"
last_month=`date -d"1 month ago" +"%Y%m%d"`
echo "Last Month of today is ${last_month}"
tomorrow=`date -d"+1 day" +"%Y%m%d"`  # `date -d"-1 day ago" +"%Y%m%d"`
echo "Tomorrow is ${tomorrow}"
next_month=`date -d"+1 month" +"%Y%m%d"`
echo "Next Month of today is ${next_month}"
```


<!-- more -->

### 时间戳

```
#timestamp
time=`date +"%s"`
echo $time
time_yesterday=`date -d"1 day ago" +"%s"`
echo $time_yesterday
```