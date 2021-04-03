---
title: pythonTip 108 LTC-男人八题之三：旅游线路问题
author: gznb
date: 2020-12-13 13:59:24
categories: [ACM, pythonTip]
tags: [简单]
math: true
---

**题目描述：**
给你一张基于网格的M x N的城镇地图（1≤M, N≤8），有些格子禁止入内，用“#”表示，其余格子用“.”（一个点）表示。
现在你需要求出从左下角走到右下角，并且经过所有可以入内的格子一次且仅一次的路径的总数。
给你一个二维List L，表示城镇地图, L中的每个元素为'#'或'.',表示含义如上所述（注意N和M没给，可以由L得到)。
如：
L = [
      ['.', '.'],
      ['.', '.']
    ]
此时的地图为：
..
..
输出：1
L = [
      ['#', '.', '.'],
      ['.', '.', '.']
    ]
此时的地图为：
#..
...
输出：1

**示例：**
输入：
L = [[".", "."], [".", "."]]
输出：
1


**分析:**
男人八题系列，我基本上都是 将 C++ 代码变换成了 Python 代码，仅供大家参考。

**代码:**
```python
# L = [[".", "."], [".", "."]]
th = [1,3,9,27,81,243,729,2187,6561,19683]   # th 对应着 三进制数字 进制位

v = [[0 for i in range(9)] for j in range(9)]   # v 是标记这个 地方是通道  还是 阻碍快

dp = [[[0 for i in range(20025)] for j in range(9)] for k in range(9)] # 作为 dp 保存状态的使用， 这里面的  15 是完全没有必要的

plugleft,plugup = 0, 0   # 记录 前一个快的 上插头 还有  左 插头

def checkbyte_three(source, place):   # 检查 三进制对应的位的 数字  是什么
    return (source//th[place])%3

def main():
    N, M = len(L), len(L[0])

    for i in range(N):
        for j in range(M):
            if L[i][j] == '#':
                v[i][j] = True
   

    dp[0][0][0]=1

    for i in range(N):
        for j in range(M+1):
            # 到了 最后一个 就可以退出了
            if i == N-1 and j == M:
                break
            # 这里不是以  j  为转移的，说明就是直接遍历到 了最后一个位置
            for s in range(th[M+1]+1):
                # 因为遍历的原因，这里的话就做了一个 判断，让一些没有的地方就直接的拜拜了。
                if dp[i][j][s] > 0:
                    # 这里是一行的最后一个位置
                    if j == M:
                        if s//th[M] == 0:
                            dp[i+1][0][s*3] += dp[i][j][s]
                        continue

                    plugleft = checkbyte_three(s, j)
                    plugup = checkbyte_three(s, j+1)

                    if v[i][j]:
                        if plugleft == 0 and plugup == 0:
                            dp[i][j+1][s] += dp[i][j][s]
                        continue
                    
                    if plugleft == 0 and plugup == 0:
                        trans = s + th[j] + th[j+1]*2
                        dp[i][j+1][trans] += dp[i][j][s]
                    
                    if (plugleft == 1 and plugup == 0) or (plugleft == 0 and plugup == 1):
                        tmp = s - plugleft * th[j] - plugup * th[j+1]
                        trans = tmp + th[j]
                        dp[i][j+1][trans] += dp[i][j][s]
                        trans = tmp + th[j+1]
                        dp[i][j+1][trans] += dp[i][j][s]
                    
                    if (plugleft == 2 and plugup == 0) or (plugleft == 0 and plugup == 2):
                        tmp = s-plugleft*th[j] - plugup*th[j+1]
                        trans=tmp + th[j] * 2
                        dp[i][j+1][trans] += dp[i][j][s]
                        trans = tmp+th[j+1]*2
                        dp[i][j+1][trans] += dp[i][j][s]
                    
                    if (plugleft==1 and plugup==1):
                    
                        sum=0
                        mat=M+1
                        for k in range(j+1, M+1):
                            dig=checkbyte_three(s,k)
                            if (dig==1):
                                    sum+=1
                            if (dig==2):
                                    sum-=1
                            if sum==0:
                                mat=k
                                break;
                            
                        
                        if (mat==M+1):
                            continue
                        trans=s-th[j]-th[j+1]-th[mat]
                        dp[i][j+1][trans]+=dp[i][j][s]
                    
                    if (plugleft==2 and plugup==2):
                    
                        sum=0
                        mat=-1
                        for k in range(j, -1, -1):
                        
                            dig=checkbyte_three(s,k)
                            if (dig==1):
                                sum -= 1
                            if (dig==2):
                                sum += 1
                            if (sum==0):
                                mat=k
                                break
                            
                
                        if (mat==-1):
                            continue
                        trans=s-th[j]*2-th[j+1]*2+th[mat]
                        dp[i][j+1][trans]+=dp[i][j][s]

                    if (plugleft==1 and plugup==2):
                        continue
                    if (plugleft==2 and plugup==1):
                        trans=s-th[j]*2-th[j+1]
                        dp[i][j+1][trans]+=dp[i][j][s]
                    
    print(dp[N-1][M][1+2*th[M]])

main()

```
