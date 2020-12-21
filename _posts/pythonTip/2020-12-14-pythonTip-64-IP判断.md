---
title: pythonTip 64 IP判断
author: gznb
date: 2020-12-13 13:15:24
categories: [ACM, pythonTip]
tags: [简单]
math: true
---

**题目描述：**
互联网上的每台计算机都有一个IP，合法的IP格式为：A.B.C.D。其中A、B、C、D均为位于[0, 255]中的整数。为了简单起见，我们规定这四个整数中不允许有前导零存在，如001这种情况。现在给你一个字符串s(s不含空白符),请你判断s是不是合法IP，若是，输出Yes,否则输出No.如：s="202.115.32.24", 则输出Yes;    s="a.11.11.11", 则输出No.



**示例：**
输入： s = "202.115.32.24"
输出： Yes



**分析:**
题目有问题，我如果添加了前导零的判断，会出错，去掉前导零的判断，反而对了。



**解题思路：**

1. 首先将 ip 这个字符串根据 `"."` 划分 成一个数组。
2. 如果数组不是四个元素，则退出，返回错误。
3. 判断每个元素，如果有一个元素不是数字，则退出，返回错误。
4. 判断每个元素，是否在 [0, 255] 之间，若不是，则退出，返回错误。
5. 若没有返回错误，则返回正确。



**代码:**
```python
flag = "Yes"

nums = s.split('.')

if len(nums) != 4:
    flag = "No"

for num in nums:
    if not num.isdigit():
        flag = "No"
        break
    # 加前导零的判断，反而GG
    # if int(num[::-1]) % 10 == 0:
    #     flag = "No"
    #     break
    if int(num) < 0 or int(num) > 255:
        flag = "No"
        break

print(flag)
```
