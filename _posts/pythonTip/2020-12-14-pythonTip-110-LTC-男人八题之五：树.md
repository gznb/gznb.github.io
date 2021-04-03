---
title: pythonTip 110 LTC-男人八题之五：树
author: gznb
date: 2020-12-13 14:01:24
categories: [ACM, pythonTip]
tags: [简单]
math: true
---

**题目描述：**
给你一个含有n个顶点的无向树，树上每条边都有一个长度p(p是不超过1001的正整数).
定义dist(u, v)表示节点u到v的最短距离。给你一个k，请你求出树上满足dist(u, v) <= k的点对数目，注意u不能和v相同。
给你一个正整数k和二维List L, 其中L中的每行是一个三元组[u, v, p],表示一条边,其中u和v表示节点，p表示长度，请你求出满足条件的点对数目。

**示例：**
输入：
k = 4
L = [[1, 2, 3], [1, 3, 1], [1, 4, 2], [3, 5, 1]]
输出：
8


**分析:**
男人八题系列，我基本上都是 将 C++ 代码变换成了 Python 代码，仅供大家参考。

**代码:**
```python
n = 0
tk = k

for item in L:
    a, b, c = item
    n = max(a, b, n)
graph = [[-1 for i in range(n+1)] for j in range(n+1)]

for item in L:
    a, b, c = item
    if graph[a][b] == -1:
        graph[a][b] = graph[b][a] = c
    else:
        graph[a][b] = graph[b][a] = min(graph[a][b], c)

m = 1
while m:
    m = 0
    for i in range(n+1):
        for j in range(n+1):
            for k in range(n+1):
                if graph[i][k] == -1 or graph[k][j] == -1 or i == k or j == k or i == j:
                    continue
                if graph[i][j] == -1 or graph[i][j] > graph[i][k] + graph[k][j]:
                    m += 1
                    graph[i][j] = graph[j][i] =  max(graph[i][k] + graph[k][j], -1)
                    
ans = 0
for i in range(n + 1):
    for j in range(i):
       if graph[i][j] != -1 and graph[i][j] <= tk:
           ans += 1
           
print(ans) 
```
