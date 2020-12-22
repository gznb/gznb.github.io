---
title: pythonTip 76 QQ音速
author: gznb
date: 2020-12-13 13:27:24
categories: [ACM, pythonTip]
tags: [简单]
math: true
---

**题目描述：**
最近Py开始玩qq音速，这个游戏只需要按4个键，上，下，左，右（分别用u,d,l,r表示）。 Py必须按照游戏规则，依次按下一系列键。问题是Py的手太胖了，他只能把两个手指放在方向键上。 Py把一个手指从键i移动到键j，要耗费`w[i][j]`的体力，而按键不需要耗费体力。 由于Py反应比较慢，所以他每次只能移动一个手指。现在可怜的Py问你，他最少耗费多少体力？ 假设Py一开始就把手指放在左、右两个键上。 

下面是`w[i][j]`数组 

|      | u    | d    | l    | r    |
| ---- | ---- | ---- | ---- | ---- |
| u    | 0    | 1    | 2    | 2    |
| d    | 1    | 0    | 1    | 1    |
| l    | 2    | 1    | 0    | 2    |
| r    | 2    | 1    | 2    | 0    |

现在给你一个仅由0,1,2,3(分别表示u,d,l,r)组成的字符串str,表示Py需要依次按下的键,串的长度不超过10^6.
请你输出Py最少需要耗费的体力值。
如：str="01230123", 则输出8。



**示例：**
输入： str = "01230123"
输出： 8



**分析:**

如果按键上面有手指，那直接按就完事了。

如果当前按键上面没有手指，这就需要一个选择了，到底是移动哪一个手指过来？

有两种可能，要么移动左手，要么移动右手。 于是我们干脆两只手都移动一下，判断一下当前的一个费用和，看看哪一个更优。

假设现在 手指正在 “上下” 两个键上面，那么	左手在上健，和左手在下键会是两种情况，这个时候，我是把两种情况都保存一下。



我最先想使用 多维数组去保存，但是我一直觉得在 python 里面用多维数组怪怪的，就直接用字典了。



移动不同的手指，其结果的一个变化需要好好的分析一下。 如果当前移动的是指 `a = ts`, 那么结果就变成了， (ts, b) 或者是 (b, ts)。而当前移动的费用和就是

`c + nums[ts][a], c 为前一步的费用和`。



最后使用排序的方式找到最小的一个费用。



**代码:**
```python
nums = [
    [0, 1, 2, 2],
    [1, 0, 1, 1],
    [2, 1, 0, 2],
    [2, 1, 2, 0]
]

item_lst = []


item_lst.append((2, 3, 0))

for s in str:
    ts = int(s)
    new_dict = dict()
    while item_lst:
        item = item_lst.pop()
        a, b, c = item

        # 如果手指正在 当前键 上面
        if a == ts or b == ts:
            new_dict[(a, b)] = c
            continue

        # 移动手指a, 最后的结果，就变成了 b, ts
        temp1 = new_dict.get((ts, b), 999999999)
        temp2 = new_dict.get((b, ts), 999999999)
        new_dict[(b, ts)] = min(temp1, temp2, c + nums[ts][a])

        # 移动手指b, 最后的结果就变成了  ts, a  
        temp1 = new_dict.get((a, ts), 999999999)
        temp2 = new_dict.get((ts, a), 999999999)
        new_dict[(ts, a)] = min(temp1, temp2, c + nums[b][ts])

    item_lst = [(key[0], key[1], value) for key, value in new_dict.items()]

item_lst.sort(key=lambda x: x[2])

print(item_lst[0][2])
```
