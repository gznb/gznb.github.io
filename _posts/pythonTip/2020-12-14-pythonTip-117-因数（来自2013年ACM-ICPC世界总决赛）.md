---
title: pythonTip 117 因数（来自2013年ACM-ICPC世界总决赛）
author: gznb
date: 2020-12-13 14:08:24
categories: [ACM, pythonTip]
tags: [简单]
math: true
---

**题目描述：**
数论的基本定理指出，对于任意大于1的整数，总有唯一的质因数分解。但是质因数的组合形式通常不止一种：
如：
10 = 2×5 = 5×2
20 = 2×2×5 = 2×5×2 = 5×2×2
设f(k)为组合的个数，有f(10) = 2, f(20) = 3 .
给定正整数n（0 < n < 1000) ，总存在至少一个k，使得f(k) = n .
求满足条件的k的最小值。
如：n=1,则输出2。 因为2=2 只有一种分解形式,故f(2)=1,且2是满足条件的最小的整数，故输出2。

**示例：**
输入：
n = 1
输出：
2

**分析:**

从数字 2 开始 依次迭代，直到出现满足情况的数字。



如何确定数字 n 的，f(n) 的结果呢？ 这是一个难题。



现在我可以确定 n 的质因数以及对应的个数。



我原本想写回溯递归暴力一波的，然后分析了一下时间复杂度，发现极端情况下 是 阶乘级别的，然后就放弃了。

后来分析可能是一个  数组递推的东西，然后又想了许久，发现推不动，毕竟 不同的质因数的个数都不一样。

然后再猜测这是不是一个排列组合的问题，我发现

 **如果 所有的质因数都不一样并且都只有一个，直接 求阶乘就行。**

**如果其中有两个相同的质因数，就只需要除以2，如果还有另外两个一样的质因素，那就再除以2。**

**如果其中有3个相同的质因数，需要除以几呢？** 发现有点难想，于是直接假设 现在一共只有3个质因数，如果完全不同,结果就是6，如果3个相同，结果就是1。 

那就是 6 / 2 / 3 = 1。 事情好像就变得明了。





**代码:**
```python

def fact(ans, nums_dict):
    nums = 2
    sums = 0
    while ans > 1:
        if ans % nums == 0:
            temp = nums_dict.get(nums, 0)
            nums_dict[nums] = temp + 1
            ans //= nums
            sums += 1
        else:
            nums += 1
    return sums





def get_nums(sums, nums_dict):
    res = 1
    for i in range(1, sums+1):
        res *= i
    
    for key, value in nums_dict.items():
        for i in range(2, value+1):
            res //= i
    return res


ans = 2
while True:
    nums_dict={}
    sums = fact(ans, nums_dict)
    if n == get_nums(sums, nums_dict):
        print(ans)
        break
    
    ans += 1
```
