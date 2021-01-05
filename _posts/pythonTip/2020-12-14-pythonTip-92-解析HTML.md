---
title: pythonTip 92 解析HTML
author: gznb
date: 2020-12-13 13:43:24
categories: [ACM, pythonTip]
tags: [简单]
math: true
---

**题目描述：**
我们每天都在浏览网页，HTML是网页的基础，现在请你模拟解析简单的HTML代码。
我们这里定义的HTML只包括两个特殊标记 `<hr>` 和 `-------`

具体的解析规则如下： 

rule1：你从输入中读入的一个单词，如果这个单词和当前行已有的长度加起来不超过80，则将该单词输出到当前行，否则另起一行，将该单词输出到下一行的开头； 

rule2：如果你从输入中读到的是`<hr>`,则换行

 rule3：如果你从输入中读到的是`------`,则另起一行输出80个'-'（如果当前正好在新行的开始，则直接输出80个'-'），并再次换行到新行的开始。

 rule4：单词之间以一个空格分开。 给你一个HTML字符串html,请你输出解析之后的结果。

**示例：**

**分析:**



这个实际上是一个简单的题目，但是因为 题目中的描述不是很清楚，所以就造成思考的一定困难。

只要按照题目给定的一个 规则进行转化，这个题目还是比较简单的。



**代码:**
```python
el_lst = html.replace('\n', ' ').split(' ')

el_lst = [el for el in el_lst if len(el)]

now_len = 0
now_str = ""
for el in el_lst:
    if el == '<br>':
        if now_str:
            print(now_str.strip())
        now_str = ""
        print("")
    elif el == '<hr>':
        print(now_str.strip())
        now_str = ""
        print("-"*80)
    else:
        if len(now_str) + len(el) >= 80:
            print(now_str.strip())
            now_str = el + " "
        else:
            now_str += el + " "
if now_str:
    print(now_str.strip())
```
