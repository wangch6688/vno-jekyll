###NavigationBar 添加阴影(shadow)---
layout: post
title: Sample Post
date: 2016-02-15 15:32:24.000000000 +09:00
---

1.设置阴影的颜色

{% highlight ruby %}
    self.navgiationController.navigationBar.layer.shadowColor = [UIColor blackColor].CGColor;
{% endhighlight %}

2.设置阴影偏移范围

{% highlight ruby %}
    self.navigationController.navigationBar.layer.shadowOffset = CGSizeMake(0, 10);
{% endhighlight %}

3.设置阴影颜色的透明度

{% highlight ruby %}
    self.navigationController.navigationBar.layer.shadowOpacity = 0.2;
{% endhighlight %}

4.设置阴影半径

{% highlight ruby %}
    self.navigationController.navigationBar.layer.shadowRadius = 16;
{% endhighlight %}

5.设置阴影路径

{% highlight ruby %}
    self.navigationController.navigationBar.layer.shadowPath = [UIBezierPath bezierPathWithRect:self.navigationController.navigationBar.bounds].CGPath;
{% endhighlight %}