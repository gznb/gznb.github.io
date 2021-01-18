---
title: pythonTip 104 Python之殇
author: gznb
date: 2020-12-13 13:55:24
categories: [ACM, pythonTip]
tags: [简单]
math: true
---

**题目描述：**
今年为什么Python这么火?因为今年是Python（英文意思是蛇）年！可是，可是蛇年就要结束了，但我们的Python会继续进行下去，我们期盼着下一个Python之年的到来。
我们迫切的想要知道，哪年是Python年。现在的任务是，给你一个四位的纯数字字符串year,表示农历的年份，请你判断该年是否是Python年。
若是，输出'Oh yeah,Python Year!'(不包括引号），
否则输出'Oh my god,Not Python Year!'(不包括引号）.
例如：
year = '2013'
则输出：Oh yeah,Python Year!
year = '2014'
则输出：Oh my god,Not Python Year!

**示例：**
输入：
year = "0001"
输出：
Oh my god,Not Python Year!



**分析:**
这个python 之年的含义就是，判断 给定年份是否为  蛇年？ 而蛇年的循环节是 12。换句话说就是 每12 年一个循环。



**代码:**
```python
python_year = 2013

now_year = int(year)

print("Oh yeah,Python Year!" if not abs(now_year-python_year) % 12 else "Oh my god,Not Python Year!")
```
