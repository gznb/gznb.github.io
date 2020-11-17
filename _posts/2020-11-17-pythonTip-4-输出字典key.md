---
title: pythonTip 4 输出字典key
author: gznb
date: 2020-11-17 11:17:04
categories: [ACM, pythonTip]
tags: [简单]
math: true
---

# 题目描述：
给你一字典a，如a={1:1,2:2,3:3}，输出字典a的key，以','连接，如‘1,2,3'。要求key按照字典序升序排列(注意key可能是字符串）。

例如：a={1:1,2:2,3:3}, 则输出：1,2,3

# 分析

使用了 **列表推导式** 。 首先是得到 所有字典的key。 然后放到一个列表里面。
最后，使用 `.join()` 将其连接起来。

得到 字典所有 key 的方法是: `dict.keys()`

列表推导式 类似于一个 for 循环。 在创建列表的时候效率比较高。
下面的列表推导式，等价于：

```python3
lst = []
for key in a.keys():
    lst.append(str(key))
```
这里说需要将 key 按照字典序进行排序。但是我没有排序，最后也能通过, 但是我记得，python 的字典key，是 hash 产生的，并且是无序的。（PS：也有一个专门的有序字典）
这就让我产生了一些疑问：
1. 是不是python 字典本身就是有序的，会按照 字典序进行排序？
2. 是不是 dict.keys() 会做一遍排序的操作？
3. 题目测试数据有点问题。

我做了如下代码测试：

```python3
nums = ['1', '2', '3', '5', '4']

di = { key:1 for key in nums}

b = [key for key in di.keys()]

# 最后 di 和 b 的值
di = {'1': 1, '2': 1, '3': 1, '5': 1, '4': 1}
b = ['1', '2', '3', '5', '4']
```
因此，应该是 题目的测试数据存在问题，和题目的描述不一致。 并且，我多次测试，感觉 python 字典的键的顺序，可能和插入顺序有关。
并且一般情况下，我们很少会去关心  字典的顺序。 如果要求有序，可以使用有序字典, 导入代码见下。 使用方式和平常的 dict 差不多。

`from collections import OrderedDict`


```python3
print(",".join([str(key) for key in a.keys()]))
```
