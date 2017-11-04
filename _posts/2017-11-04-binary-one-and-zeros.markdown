---
layout: post
title: Binary one and zeros
date: 2017-11-04
categories: dynamic-programming
---
## About this topic
这里描述几个跟0/1相关的问题，都是求整数中满足条件的0/1 bit位。

## Problem description
# 问题一
求一个整数n的二进制表示中，1的个数。
# 问题二
对于一个给定的n，我们可以求出n中1的个数。那么对于给定[0~n]的连续数字，求返回一个数组，数组每个元素表示i中1的个数。
# 问题三
对于在[0~n]之间的数，求他们的二进制表示中，满足没有连续两个1的数字的个数。这个题有两种问法，一个是n为2的整数幂；另一个是n为随机数。
# 问题四
对于在[0~n]之间的数，求他们的二进制表示中，满足有连续k个1的元素个数。

## 解法
# 问题一
这是一个比较老的题了，解决方法大家都知道。a = a & (a - 1);
# 问题二
这个题跟问题一类似，但是如果用问题一的方式来解，就是O(32n)的复杂度，有没有满足O(n)的复杂度呢？考虑a = a & (a-1)的原理是什么？然后就可以用动态规划的方式来解了dp[a] = dp[a & (a-1)] + 1
# 问题三
对于n是2的整数次幂，相当于是给定一个logn大小的数组，求满足不连续1的条件的组合个数。这个用动态规划就可以解了。
对于n不是2的整数次幂的时候，就比较复杂了。这个问题来自leetcode 600
```c++
int findIntegers(int num) {
	//计算一个32的数组，保存0～31个1的情况下，对应的满足条件的数字个数
	int mp[32];
	int pre1 = 0;
	int pre0 = 1;
	mp[0] = 1;
	for (int i = 1; i < 32; ++i){
		int tmp = pre1;
		pre1 = pre0;
		pre0 = pre0 + tmp;
		mp[i] = pre1 + pre0;
		// std::cout << mp[i] << " ";
	}
	// std::cout << std::endl;

	int index = 32;
	unsigned int mask = 0x1 << 31;
	int total = 0;
	int pre1_index = INT_MAX;
	while (index > 0) {
		// 处理num的末尾时111或者0的情况，这时可以直接加上mp的值。
		if (num == (mask - 1)) {
			total += mp[index - 1];
			break;
		}
		if (num & mask) {
			if ((pre1_index - index) == 1) {
				// 处理11000中的连续两个1的情况，当出现连续两个1时，不再继续。但是要加上10xxxx的情况
				total += mp[index - 1];
				break;
			}
			pre1_index = index;
			total += mp[index - 1];
		}
		num = num & (~mask);
		mask = mask >> 1;
		index--;
	}
	
	return total;
}
```
# 问题四
这个问题还没想好，只是偶然想到了会有这样的变种。


