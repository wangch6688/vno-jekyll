###NSDate的一些用法---
layout: post
title: Sample Post
date: 2018-04-12 15:32:24.000000000 +09:00
---

1.获取当前的时间

{% highlight ruby %}
- (NSDate *)getCurrentTime {
    NSDateFormatter *formatter=[[NSDateFormatter alloc]init];
    //设置想要的时间格式
    [formatter setDateFormat:@"yyyy-MM-dd"];
    NSString *dateTime=[formatter stringFromDate:[NSDate date]];
    NSDate *date = [formatter dateFromString:dateTime];
    return date;
}
{% endhighlight %}

2.时间转时间戳

{% highlight ruby %}
- (NSString *)timeTapString:(NSString *)timeStr {
    NSDateFormatter *formatter = [[NSDateFormatter alloc] init];
    //设置想要的时区
    NSTimeZone* timeZone = [NSTimeZone timeZoneWithName:@"Asia/Beijing"];
    [formatter setTimeZone:timeZone];
    formatter.dateFormat = @"yyyy-MM-dd";
    NSDate *date = [formatter dateFromString:timeStr];
    NSString *timeSp = [NSString stringWithFormat:@"%ld", (long)[date timeIntervalSince1970]];
    return timeSp;
}
{% endhighlight %}

3.时间戳转时间

{% highlight ruby %}
- (NSString *)getTimeFromTimestamp {
double time = 1504667976;
NSDate * myDate=[NSDate dateWithTimeIntervalSince1970:time];
//设置时间格式
NSDateFormatter * formatter=[[NSDateFormatter alloc]init];
[formatter setDateFormat:@"YYYY-MM-dd HH:mm:ss"];
//将时间转换为字符串
NSString *timeStr=[formatter stringFromDate:myDate];
return timeStr;
}
{% endhighlight %}

4.两个时间的早晚

{% highlight ruby %}
- (int)compareOneDay:(NSDate *)oneDay withAnotherDay:(NSDate *)anotherDay {
    NSDateFormatter *dateFormatter = [[NSDateFormatter alloc] init];
    [dateFormatter setDateFormat:@"yyyy-MM-dd"];
    NSString *oneDayStr = [dateFormatter stringFromDate:oneDay];
    NSString *anotherDayStr = [dateFormatter stringFromDate:anotherDay];
    NSDate *dateA = [dateFormatter dateFromString:oneDayStr];
    NSDate *dateB = [dateFormatter dateFromString:anotherDayStr];
    NSComparisonResult result = [dateA compare:dateB];
    if (result == NSOrderedDescending) {
        //在指定时间前面 过了指定时间 过期
            return 1;
    }
    else if (result == NSOrderedAscending){
        //没过指定时间 没过期
            return -1;
    }
        //刚好时间一样.
        return 0;
}

{% endhighlight %}
