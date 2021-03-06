---
title: pythonTip 41 Py数
author: gznb
date: 2020-12-13 12:54:24
categories: [ACM, pythonTip]
tags: [简单]
math: true
---

**题目描述：**
Py从小喜欢奇特的东西，而且天生对数字特别敏感，一次偶然的机会，他发现了一个有趣的四位数2992，这个数，它的十进制数表示，其四位数字之和2+9+9+2=22，它的十六进制数BB0，其四位数字之和也为22，同时它的十二进制数表示1894，其四位数字之和也为22，啊哈，真是巧啊。Py非常喜欢这种四位数，由于他的发现，所以这里我们命名其为Py数。现在给你一个十进制4位数n，你来判断n是不是Py数，若是，则输出Yes，否则输出No。如n=2992，则输出Yes； n = 9999，则输出No。



**示例：**

> 输入：n = 1234
> 输出：No



**分析:**
按照我们一般的思路而言，就是先将给定数转化为  16， 12进制，然后判断对应进制下面相加在一起的和，是不是等于22。

如果我们为 10转16进制，10转12进制都分别编写代码的话，就比较复杂了，其实我们根本就不需要这样。

我们可以写一个函数，然后将需要转化的进制作为参数传递给函数就可以了，

并且我们也不用找到不同进制下面一一对应的字母。（Ps: 想想为啥不用找到对应的字母？）



**代码:**
```python
def get_system(num, s):
    sums = 0
    
    while num:
        sums += num % s
        num //= s
    return sums
if get_system(n, 10) == 22 and get_system(n, 16) == 22 and get_system(n, 12) == 22:
    print("Yes")
else:
    print("No")
```

