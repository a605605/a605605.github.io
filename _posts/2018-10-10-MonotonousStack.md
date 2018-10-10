---
layout: post
title:  "单调栈"
categories: 数据结构和算法
tags:  数据结构 单调栈 
author: a605605
---

* content
{:toc}

## 单调栈的定义

1、首先，显然单调栈是一种栈结构，只能在栈顶对元素进行操作（入栈和出栈）

2、单调栈在栈的基础上，还要求栈中元素保证一定的偏序关系（偏序上升或偏序下降）

## 单调栈的作用

用一个从栈底到栈顶偏序下降（上升）的单调栈，可以在O（n）的时间内找到每个元素的距离第一个比这个元素大（小）的元素。

## 单调栈的入栈操作

设`s`是一个从栈底到栈顶偏序下降的单调栈，`a`是欲入栈的长度为N的数组，则单调栈的入栈算法如下：

```c++
for (int i = 0; i < N; ++i)
{
    if (!s.empty() && a[i] > s.top())
    {
        /*
            如果欲入栈的元素比栈顶元素大
            则将栈顶元素出栈
            直至栈空或欲入栈的元素比栈顶元素为止
            从而保证从栈底到栈顶是偏序下降的
        */
        while (!s.empty() && a[i] > s.top())
        {
            s.pop();
        }
    }
    s.push(a[i]);
}
```

## 单调栈的具体应用

题目链接：[HDU3410:Passing the Message](http://acm.hdu.edu.cn/showproblem.php?pid=3410)

### 题意

给出N个孩子的身高，设每个孩子只能在他和向左看第一个比他高的孩子之间比他矮的孩子以及向右看第一个比他高的孩子之间比他矮的孩子之间传递消息，求每个孩子的传递消息范围中身高最高的孩子的位置。

### 解题思路

利用从栈底到栈顶偏序下降的单调栈，从左到右处理每个孩子，如果这个孩子的身高高于栈顶孩子的身高，则将栈中孩子出栈，在这个过程中维护消息传递左范围内身高最高的孩子，然后从右往左处理每个孩子，类似地找出消息传递右范围内的身高最高的孩子。

### AC代码

```c++
#include <bits/stdc++.h>
using namespace std;
void Solve();
int main(int argc, char *argv[])
{
	#ifndef ONLINE_JUDGE
	freopen("in.txt", "r", stdin);
	#endif
	Solve();
	return 0;
}
const int maxsize = 5e4;
int T, N, kids[maxsize];
stack<int> s;
void Solve()
{
	int left[maxsize], right[maxsize];
	scanf("%d", &T);
	for (int t = 1; t <= T; ++t)
	{
		scanf("%d", &N);
		for (int i = 0; i < N; ++i)
		{
			scanf("%d", kids + i);
			left[i] = right[i] = 0;
		}
		printf("Case %d:\n", t);
		for (int i = 0; i < N; ++i)//处理每个孩子的左消息传递范围
		{
			if (!s.empty() && kids[i] > kids[s.top()])//这个孩子左的孩子比他矮
			{
				int maxShortest = kids[s.top()];
				left[i] = s.top() + 1;
				while (!s.empty() && kids[i] > kids[s.top()])
				{
					if (kids[s.top()] > maxShortest)//左消息传递范围内比他矮的最高孩子
					{
						left[i] = s.top() + 1;
						maxShortest = kids[s.top()];
					}
					s.pop();
				}
			}
			s.push(i);
		}
		while (!s.empty())
		{
			s.pop();
		}
		for (int i = N - 1; ~i; --i)//处理每个孩子的右消息传递范围
		{
			if (!s.empty() && kids[i] > kids[s.top()])
			{
				int maxShortest = kids[s.top()];
				right[i] = s.top() + 1;
				while (!s.empty() && kids[i] > kids[s.top()])//这个孩子右边的孩子比他矮
				{
					if (kids[s.top()] > maxShortest)//右消息传递范围内比他矮的最高的孩子
					{
						right[i] = s.top() + 1;
						maxShortest = kids[s.top()];
					}
					s.pop();
				}
			}
			s.push(i);
		}
		while (!s.empty())
		{
			s.pop();
		}
		for (int i = 0; i < N; ++i)
		{
			printf("%d %d\n", left[i], right[i]);
		}
	}
}
```
