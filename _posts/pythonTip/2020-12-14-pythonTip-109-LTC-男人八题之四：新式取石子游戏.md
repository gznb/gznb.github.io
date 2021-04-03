---
title: pythonTip 109 LTC-男人八题之四：新式取石子游戏
author: gznb
date: 2020-12-13 14:00:24
categories: [ACM, pythonTip]
tags: [简单]
math: true
---

**题目描述：**
小明和小红参加一种新的取石子游戏。游戏开始时有 n （1 <= n <= 10)堆石子，参与游戏的两个选手轮流取走或移动石子。在每一轮中，选手选择一个石子堆，从该堆石子中拿走至少一个石子。然后，该选手可以多次地从该堆中剩下的石子中把任意多的个石子移动到其它堆中。当然，该选手也可以不移动任何石子。但是，注意，选手必须从选中的堆中取走至少一块石子。因此，随着游戏的进行，石子越来越少, 最后一个取完石子的人获胜。
现在给你一个正整数list L(L的长度不超过10），每个元素代表相应石子堆石子的数目，如L[0]代表第一堆石子的数目，
小明和小红都极其聪明，总能以最优的策略进行游戏。游戏从小明开始，请你判断小明能否在游戏中获胜？
若能获胜，输出：Yes否则输出：No
如：L = [2, 1, 3], 则输出Yes
    L=[1, 1], 则输出No

**示例：**
输入：
L = [2, 1, 3]
输出：
Yes


**分析:**
男人八题系列，我基本上都是 将 C++ 代码变换成了 Python 代码，仅供大家参考。

**代码:**
```python
ans_dict = {}

for item in L:
    temp = ans_dict.get(item, 0)
    ans_dict[item] = temp + 1

ans_set = {key for key, value in ans_dict.items() if value % 2}


print("Yes" if len(ans_set) else "No")
```
