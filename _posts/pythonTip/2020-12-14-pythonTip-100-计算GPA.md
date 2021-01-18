---
title: pythonTip 100 计算GPA
author: gznb
date: 2020-12-13 13:51:24
categories: [ACM, pythonTip]
tags: [简单]
math: true
---

**题目描述：**
每门课程的成绩分为五个等级：A,B,C,D,F(注意没有E),它们分别代表可以获得4,3,2,1,0个绩点.
现在给你一个由大写字母构成的列表L，请你计算平均绩点，保留小数点后两位。
若L中包含非法成绩等级，则输出-1.
如：
L = ["A", "B", "C", "D", "F"]
则输出2.00

**示例：**
输入：
L = ["A", "B", "C", "D", "F"]
输出：
2.00

**分析:**



直接贴代码，仅供参考。

将表示等级的字符串转换成具体的绩点，然后求和取平均值即可。

**代码:**
```python
gpa_map = {
    'A': 4,
    'B': 3,
    'C': 2,
    'D': 1,
    'F': 0
}

ans = 0

for item in L:
    ans += gpa_map.get(item, -99999999)

print("{:.2f}".format(ans/len(L)) if ans >= 0 else -1)
```
