###NavigationBar 添加阴影(shadow)---
layout: post
title: Sample Post
date: 2018-06-04 17:42:24.000000000 +09:00
---###

atomic
{% highlight ruby %}
 1. 默认的
 2. 会保证CPU能在别的线程来访问这个属性之前,先执行完当前操作
 3. 速度不快,因为要保证操作整体完成
{% endhighlight %}

nonatomic
{% highlight ruby %}
 1. 不是默认的
 2. 更快
 3. 线程不安全(如有两个线程访问同一个属性,会出现无法预料的结果)
{% endhighlight %}

{% highlight ruby %}
iOS在声明属性时,默认会是atomic,当然如果在创建属性之后选择自己写setter/getter方法,那么atomic和nonatomic关键字将没有意义.
{% endhighlight %}

对于atomic关键字修饰的属性而言,系统生成的gette/setter方法会保证在进行get/set操作时的完整性,不会受到其余线程的影响,可以保证读写操作的安全性.例如:线程A的getter方法运行到一半,线程B调用了setter方法,此时线程A的getter方法还是能获取到一个完好无损的对象,这是一个串行的操作.但是如果是nonatomic修饰的属性,当多个线程同时对属性进行读写操作,会进行并发访问,可能会产生不可预知的问题,例如crash.

atomic可以保证属性的读写是安全的,但是atomic无法保证值的时效性,例如:线程A调用了getter,与此同时线程B和C都调用的setter,此时A get到的值可能是B  set的值,也可能是C set的或者最初的值,因为无法确定是哪个线程先执行完成,但是会保证一定会有值.


{% highlight ruby %}
保证数据的完整性,这是多线程变成最大的挑战之一,往往需要借助其余的手段
{% endhighlight %}

代码:
nonatomic修饰属性生成的getter/setter方法
{% highlight ruby %}
- (UITextField *) userName {
	return userName;
}

- (void)setUserName:(UITextField *)userName_ {
	[userName_ retain];
	[userName release];
	userName = userName_;
}
{% endhighlight %}

atomic修饰属性生成的getter/setter方法
{% highlight ruby %}
- (UITextField *)userName {
	UITextField * retval = nil;
	@sychronized(self) {
		retval = [[userName retain] autorelease];
	}
	return retval;
}

- (void)setUserName:(UITextField *)userName_ {
	@sychronized(self) {
		if(userName != userName_) {
			[userName release];
			userName = [userName_ retain];
		}
	}
}
{% endhighlight %}

关于@sychronized同步锁，在atomic关键字修饰后创建的setter/getter方法中并不是使用的@sychronized同步锁，而是使用的互斥锁.关于其中的区别,会在下面的章节中具体说明.
