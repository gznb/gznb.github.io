---
title: pythonTip 96 Py的寻路系统
author: gznb
date: 2020-12-13 13:47:24
categories: [ACM, pythonTip]
tags: [简单]
math: true
---

**题目描述：**
Py是个没有方向感的人，经常在校园内迷路，所以他经常手里拿着一张地图。每天Py都在校园内转来转去，但是Py不是一个喜欢浪费时间的人，每次转悠的时候，他总想找到一条从起点到终点的最短路。现在这个任务就交给了你，希望你给Py设计一个查询系统，Py每次只需要输入起点和终点，你就要告诉Py这两点间的最短路的长度是多少。
现在给你两个整数n和m以及一个三元组列表L，n表示地图上路口的数目，m表示地图上小路的个数，
其中1 <= n <= 100，0 <= m < n(n-1)/2。
列表L由m个三元组构成，每个三元组[a, b, len]表示一条连接路口a和路口b的小路，小路长为len，路口的编号从1开始，即 1 <= a,b <= n,这里的路都是双向的，没有一条路是连接两个相同的路口。
现在Py想从s路口到t路口，请你输出s路口到t路口的最短路径的长度，若不可达，则输出-1.
例如：
n=3,m=3
L=[[1,2,1],[2,3,1],[1,3,1]]
s=1, t=2
则输出1，即从路口1到路口2的最短路径长度为1.

**示例：**
输入：
n = 3
m = 3
s = 1
t = 2
L = [[1, 2, 1], [2, 3, 1], [1, 3, 1]]
输出：
1

**分析:**

此题本质是 **单源最短路径问题**。

给出一个简单的思路,使用 Dijkstra 算法。就是从一个顶点（源点）出发到达另一个顶点(终点)的路径可能不止一条,如何找到一条路径使得沿此路径上各边的权值总和(称为路径长度)达到最小。



定义一个 最小值优先的 优先队列，也就是 值越小就越是排列在前面。

算法步骤：

1. 将所有顶点的价值设置为很大的一个数（假设是无穷大）。
2. 将源点的价值标记为0， 并且将 源点加入到优先队列中。
3. 取优先队列队首顶点。遍历和这个顶点相连的所有顶点。
   1. 如果 从刚刚取到顶点出发，可以更新相邻顶点的价值，则更新顶点的价值，并且将顶点加入到优先队列中
4. 当优先队列为空时，算法结束。







**代码:**

```python
from queue import PriorityQueue

pro_que = PriorityQueue()

flag_lst = [0 for _ in  range(n+1)]

# 初始话所有顶点的价值，以及二维表地图的值。
v_lst = [99999999 if i != s else 0 for i in range(0,n+1)]
nums = [[99999999 for i in range(n+1)] for j in range(n+1)]

# 将 三元组表示的 顶点和边 转换为 二维表。
for item in L:
    x, y, v = item
    nums[x][y]=nums[y][x] = v
flag_lst[s] = 1

# 定义一个最小值优先的 优先队列。
class node:
    def __init__(self, pos, value):
        self.pos = pos
        self.value = value
    
    def __lt__(self, other):
        return self.value < other.value

pro_que.put(node(s, 0))

while not pro_que.empty():
    now = pro_que.get()
    pos, value = now.pos, now.value
    for i in range(1, n+1):
        # 依次遍历，当两个顶点之间不能相连时或者是两个相同的顶点时 跳过。
        if i == pos or nums[i][pos] == 99999999:
            continue
        if v_lst[i] > value + nums[i][pos]:
            v_lst[i] = value + nums[i][pos]
            pro_que.put(node(i, v_lst[i]))
print(v_lst[t] if v_lst[t]!=99999999 else -1)
```
