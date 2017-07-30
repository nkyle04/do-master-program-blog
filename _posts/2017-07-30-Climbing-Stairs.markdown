---
layout: post
title: Climbing stairs
date: 2017-07-30
categories: dynamic programming
---
# Problem description

> You are climbing a stair case. It takes n steps to reach to the top.
> 
> Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?
> 
> Note: Given n will be a positive integer.

# Resolution

这是一个简单的斐波那契数列问题。可以用f(n) = f(n-1) + f(n-2)解决
代码如下：
```c++
int climbStairs(int n) {
	int f0 = 1;
	int f1 = 1;
	int x = f1;
	for (int i = 2; i <= n; ++i) {
	    x = f0 + f1;
	    f0 = f1;
	    f1 = x;
	}

	return x;
}
```
