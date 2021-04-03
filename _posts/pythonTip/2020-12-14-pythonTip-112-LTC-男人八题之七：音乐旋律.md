---
title: pythonTip 112 LTC-男人八题之七：音乐旋律
author: gznb
date: 2020-12-13 14:03:24
categories: [ACM, pythonTip]
tags: [简单]
math: true
---

**题目描述：**
给定一个正整数序列L（L的长度不超过20000，1<=L[i]<=88)，问这个序列中存在的最长一个符合下列三个条件的子序列长度是多少：
条件1：子序列A的长度不小于5
条件2：存在另一个子序列B，且A和B不重叠
条件3：A和B的长度一样，且A[0]-B[0] = A[1]-B[1] = ... = A[k]-B[k],及两个子序列对应项的差相等。
请你输出符合条件的最长的子序列A的长度。
如：L = [25, 27, 30, 34, 39, 45, 52, 60, 69, 79, 69, 60, 52, 45, 39, 34, 30, 26, 22, 18, 82, 78, 74, 70, 66, 67, 64, 60, 65, 80]，则输出：5



**示例：**
输入：
L = [25, 27, 30, 34, 39, 45, 52, 60, 69, 79, 69, 60, 52, 45, 39, 34, 30, 26, 22, 18, 82, 78, 74, 70, 66, 67, 64, 60, 65, 80]
输出：
5


**分析:**
男人八题系列，我基本上都是 将 C++ 代码变换成了 Python 代码，仅供大家参考。

**代码:**
```python
def get_suff(strr):
    suff_lst = []
    s_len = len(strr)
    for i in range(s_len):
        suff_lst.append(strr[i:])
    return suff_lst

def get_sa_and_rank(suff_lst):
    suff_dict = dict()
    step = 0
    for item in suff_lst:
        suff_dict[item] = step
        step += 1

    sa = []
    rank = []

    suff_order_lst =  sorted(suff_lst)

    # 构造 sa 数组
    for item in suff_order_lst:
        sa.append(suff_dict[item])

    # 构造  rank 数组
    for item in suff_lst:
        rank.append(suff_order_lst.index(item))

    return sa, rank

def get_height(sa_lst, rank_lst, strr):
    s_len = len(sa_lst)
    height_lst = [0 for i in range(s_len)]
    strr += "0"
    
    k = 0
    for i in range(s_len):
        k =  k-1 if k else 0

        j = sa_lst[rank_lst[i] - 1]

        while strr[i+k] == strr[j+k]:
            k += 1
        height_lst[rank_lst[i]] = k
    return height_lst


def judge(sa_lst, height_lst, k):
    L = 1
    while L <= N:
        R = L
        mx = mn = sa_lst[L]

        while  R < N and height_lst[R+1] >= k :
            R += 1
            mx = max(mx, sa_lst[R])
            mn = min(mn, sa_lst[R])
        
        L = R+1
        if mx - mn >= k:
            return True
    return False

N = len(L)

# if N < 10:
#     print(0)

strr = "".join([chr(L[i+1] - L[i] + 90) for i in range(N-1)])
N -= 1
strr += "X"

suff_lst = get_suff(strr)
sa_lst, rank_lst = get_sa_and_rank(suff_lst)
height_lst = get_height(sa_lst, rank_lst, strr)

L, R = 1, N

while L + 1 < R:
    mid = (L+R) >> 1
    if judge(sa_lst, height_lst, mid):
        L = mid
    else:
        R = mid

if L < 4:
    print(0)
else:
    print(L+1)
```
