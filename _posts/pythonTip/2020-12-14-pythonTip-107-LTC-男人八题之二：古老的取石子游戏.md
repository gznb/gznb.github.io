---
title: pythonTip 107 LTC-男人八题之二：古老的取石子游戏
author: gznb
date: 2020-12-13 13:58:24
categories: [ACM, pythonTip]
tags: [简单]
math: true
---

**题目描述：**
游戏的开始有n个排成一条线的石子堆，游戏的目标是将这些堆石子合并成一堆，合并的规则如下：
Rule:每一步，游戏玩家只能合并相邻的两堆石子，得分是被合并的两堆石子数目之和。
你的目标是求出合并所有石子能够得到的最小得分之和。
给你一个正整数列表L,L中每个数字表示对应石子堆的石子数目，如L[0]表示第一个石子堆的石子数目，请你输出合并的最小得分之和。
如：
L=[100],则输出0
L=[3, 4, 3], 则输出17.

**示例：**
输入：
L = [100]
输出：
0


**分析:**
男人八题系列，我基本上都是 将 C++ 代码变换成了 Python 代码，仅供大家参考。

**代码:**
```python
len_l = len(L)

nums = [[999999 for i in range(len_l+1)] for j in range(len_l+1)]

for i in range(len_l):
    nums[i][i] = 0

for i in range(len_l-1, -1, -1):
    dev = len_l - i
    for j in range(i+1):
        x, y = j, j+dev
        for k in range(x, y):
            nums[x][y] = min(nums[x][y], nums[x][k]+nums[k+1][y]+sum(L[x:y+1]))

print(nums[0][len_l-1])
```
