---
title: pythonTip 113 LTC-男人八题之八：电梯调度
author: gznb
date: 2020-12-13 14:04:24
categories: [ACM, pythonTip]
tags: [简单]
math: true
---

**题目描述：**
有一栋楼, 里面只有一架电梯. 电梯上一层需 4s,停下是瞬时的，再次启动（不含从1楼的那次启动）要10秒, 人上下一层楼都要20秒.
现在电梯初始在一楼，所有人也都在一楼，告诉你人群需要到达的楼层情况, 求一个安排计划,使最后到达自己目的地的人的用时最短(1 层和最后一层的等待不计时).
现在给你一个正整数List L，L[i]表示有人的目的地在第L[i]层，L的长度不超过300000， 2 <= L[i] <= 30000 ,请你输出最后一个到达目的地的人所用的最短时间.
如：L = [4, 5, 10], 则输出：46
    说明：此时的安排：需要在四楼和五楼下的人都在四楼下，10楼的人在10楼下。
    此时，各层人用时：4楼：12s  5楼：12 + 20 = 32s， 10楼：9 * 4 + 10 = 46s， 最后一个到达目的地的人用时为46s
    L = [2], 则输出：4

**示例：**
输入：
L = [4, 5, 10]
输出：
46


**分析:**
男人八题系列，我基本上都是 将 C++ 代码变换成了 Python 代码，仅供大家参考。

**代码:**
```python
def judge(T):
    curt, curf = 0, 1
    
    for i in L:
        
        # 直接从第一层可以走到目标楼层
        if (i-1) * 20 <= T:
            continue
        
        # 可以 直接从当前楼层 走到目标楼层
        if curt + abs(curf-i) * 20 <= T:
            continue
        # 这个是我们的一个 假定楼层
        tr = i-1
        while True:
            nums = curt + (0 if curf == 1 else 10) + abs(tr-i+1) * 20 + abs(tr-curf+1)*4
            
            if nums <= T:
                tr += 1
            else:
                break
            
            
        if tr == i - 1:
            return False
        
        curt = curt + (0 if curf == 1 else 10) + abs(tr-curf) * 4
        
        curf = tr
    return True
left, right = 0, (L[-1] - 1) * 20

while left  < right:
    mid = (left+right) >> 1
    if judge(mid):
        right = mid
    else:
        left = mid + 1
    

print(right)
```
