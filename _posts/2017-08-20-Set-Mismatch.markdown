---
layout: post
title: Set Mismatch(leetcode 645)
date: 2017-08-20
categories: program
---
# Problem description
This problem comes from leetcode 645. It is a easy problem, but it involves two skills, and it is a typical problem for finding a number from an array within N elements. It's very interesting to analyzing it. Following is the problem description:
> The set S originally contains numbers from 1 to n. But unfortunately, due to the data error, one of the numbers in the set got duplicated to another number in the set, which results in repetition of one number and loss of another number.
>
> Given an array nums representing the data status of this set after the error. Your task is to firstly find the number occurs twice and then find the number that is missing. Return them in the form of an array.
>
> Example:
> ```c++
> Input: nums = [1,2,2,4]
> Output: [2,3]
> ```

# Solution
这个问题实际上是两个问题合在了一起：
1. 一个是在N-1的数组中，找到丢失的那个数。数组取值在1～N，并且没有重复。
2. 另一个是利用O(n)的复杂度，找到数组中重复的那个数。

因此这个问题可以分成两个部分求解。

第一部分，如果假设数组中只是少了一个数，要找到少了的这个数，需要怎么做？这个实际上很简单，只要把所有数相加，然后与期望的值相减就可以了。
```c++
// 计算1～n的所有数之和
int need_sum = n * (n + 1) / 2;

// 计算当前数组中实际所有数的和
int actual_sum = 0;
for (auto a : array) {
    actual_sum += a;
}
// 计算得到数组中少了的那个数
int num = need_sum - actual_sum;
```

第二部分，对于原始的问题来说，实际上不是少了一个数，而是有两个重复的数。

因此这里就要求出实际重复的那个数是多少。求解重复的数有很多方法，比如用std::set，进行排序等等。

这里由于问题的特殊性，整个数组的数都是在1～n之间的，并且只有一个重复。因此我们可以考虑使用计数排序的方式。就是用一块n大小的辅助数组，把对应的值填入到对应的位置，这样如果位置上已经有数了，则认为这个位置上的数重复了。

虽然能够做到O(n)的复杂度，但是这个方式需要使用额外的存储空间。这里要用的是不需要额外辅助空间的方式，也就是做到O(n)的时间复杂度和O(1)的空间复杂度。

为了实现这个目的，我们需要在原始的array上进行操作，把原始array当作辅助空间来用，每次处理一个数的时候，就修改这个数的值对应的array的位置。例如对于i位置的数值是ai，则修改array[ai] += n，这样把值改掉，当出现重复的数时，就可以判断当前值大于n则为重复。同时考虑到已经修改了array 的值，那么如何得到原始值呢，这就需要对array的元素再取%运算，得到原始值。
```c++
for (int i = 0; i < n; ++i) {
    int a = array[i] % (n + 1);
    if (array[a] > n) {
        // found duplicate number
        break;
    }
    array[a] += n + 1;
}
```

经过上述两个步骤，把他们结合在一起就可以解决这个问题了。
```c++
vector<int> findErrorNums(vector<int>& nums) {
	int n = nums.size();
	int sum = 0;
	int dup = 0;
	int total = 0;
	for (int i = 0; i < n; ++i) {
		total += i + 1;
		int num = nums[i] % (n + 1);
		sum += num;
		if (nums[num - 1] > n) {
			dup = num;
		} else {
			nums[num - 1] += (n + 1);
		}
	}
	// std::cout << dup << " " << dup + total - sum << std::endl;
	return std::vector<int>({dup, dup + total - sum});
}
```
这道题我觉得很有意思的点在于结合了两个简单的问题到一起，能够考察到知识的迁移能力。这一点在学习编程的过程中，我觉得非常有用。经常思考一下问题的变形，思考在不同情况下的解法，对提升自身的能力非常有帮助。

