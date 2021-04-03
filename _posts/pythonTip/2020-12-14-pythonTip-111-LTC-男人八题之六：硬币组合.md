---
title: pythonTip 111 LTC-男人八题之六：硬币组合
author: gznb
date: 2020-12-13 14:02:24
categories: [ACM, pythonTip]
tags: [简单]
math: true
---

**题目描述：**
小Py手里有面值为A1,A2,A3...的硬币若干枚，现在他想知道，用手里的硬币能够拼凑出多少种不超过m的不同钱数的数目。
给你一个正整数m(m <= 100000)和两个等长的正整数List L1和L2(L2[i] <= 1000), L1中的元素表示硬币的面值， L2中的元素表示对应面值硬币的数目，即L2[i]表示小Py拥有的面值为L1[i]的硬币个数。
请你输出小Py利用手中的硬币能够拼凑出的不同钱数的数目。
如：
L1 = [1, 4], L2 = [2, 1], m = 100, 则输出：4

**示例：**
输入：
m = 10
L1 = [1, 2, 4]
L2 = [2, 1, 1]
输出：
8


男人八题系列，我基本上都是 将 C++ 代码变换成了 Python 代码，仅供大家参考。

**代码:**
```python
v_lst = [0 for i in range(m+1)]
v_lst[0] = 1

tn = len(L1)

for i in range(tn):
     p_lst = [0 for i in range(m+1)]

     for j in range(1, m+1):
        if v_lst[j] or j < L1[i]:
            continue
        
        if v_lst[j-L1[i]] and p_lst[j-L1[i]] < L2[i]:
            v_lst[j] = 1

            p_lst[j] = p_lst[j-L1[i]] + 1
print(sum(v_lst)-1)
```
