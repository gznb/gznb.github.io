---
title: pythonTip 65 RSA密码方程
author: gznb
date: 2020-12-13 13:16:24
categories: [ACM, pythonTip]
tags: [简单]
math: true
---

**题目描述：**
在RSA密码体系中,欧几里得算法是加密或解密运算的重要组成部分。它的基本运算过程就是解 (x*a) % n = 1 这种方程。
其中，x,a,n皆为正整数。现在给你a和n的值(1 < a,n < 140000000)，请你求出最小的满足方程的正整数解x（保证有解）.
如：a = 1001, n = 3837，则输出23。



**示例：**
输入： a = 1001  n = 3837

输出：23



**分析:**
此题使用扩展欧几里得算法求解。

**欧几里得算法：** 欧几里德算法又称辗转相除法，用于计算两个整数a,b的最大公约数 gcd(a,b)。基本算法：设 **a = qb + r**，其中a，b，q，r都是整数，则 

**gcd(a, b) = gcd(b, r)，即 gcd(a, b) = gcd(b, a%b)。**

证明：
**a = qb + r**

**推理1:** 如果 r = 0，那么 a 是 b 的倍数，此时显然 b 是 a 和 b 的最大公约数。

**推理2:** 如果 r ≠ 0，任何整除 a 和 b 的数必定整除 a - qb = r，而且任何同时整除 b 和 r 的数必定整除 qb + r = a，所以 a 和 b 的公约数集合与 b 和r 的公约数集合是相同的。特别的，a 和 b 的最大公约数是相同的。

例如： a = i * k,  b = j * k 

那么 r = i * k - j * q * k = (i - j*q) * k    

a, b, r 都可以整除 k 。



python 参考代码如下：

```python
def gcd(a, b):
	return a if b == 0 else gcd(b, a%b)

def gcd(a, b):
    if b == 0:
        return a
   	else:
        return gcd(b, a%b)
```



**扩展欧几里得算法:** 在求 a , b 的最大公约数 m = g c d ( a , b ) 的同时，求出贝祖等式  ***ax + by = m***的一个解 ( x , y ) 。

对贝祖等式有兴趣的可以百度了解一下。

***ax + by = gcd(a, b)   ------- (1)***

***bx + a%by = gcd(b, a%b)  -----(2)***

**a%b = a - a//b * b -----(3)**

**gcd(a, b) == gcd(b, a%b)   ----(4)**



所以就有:
$$
ax + by = bx + a\%b y = bx + (a-a//b*b)y = bx + ay - a//b*by = ay  + b(x-a//b*y)
$$
于是就有：

x = y

y = x - a // b*y

递归版参考代码：可以和上面的  朴素欧几里得算法对比学习。



```python
# arr[0] 相当于 x,  arr[1] 相当于 y
def exgcd(a, b, arr):
    # 推理1 终止条件
    if b == 0:
        arr[0] = arr[1] = 1, 0
        return a
    r = exgcd(b, a%b, arr[0], arr[1])
    
    t = arr[1]
    arr[1] = arr[0] - (a//b) * arr[1]
    arr[0] = t
    return r
```



真正难的是 **非递归扩展欧几里得算法**，这个才是真的有点点难度。需要消耗一点点的思考吧。

先给出算法步骤：

1. [初始化]:  令 `a' = b = 1, a = b' = 0, c = m, d = n`;
2. [除法]: 令 q 和 r 分别是 d 除 c 所得的商和余数。 `q = d//c,  r = d//r `;
3. [余数为0?] : 如果 r = 0 ，算法终止，此时有 `am + bn = d`  , 正如所求.
4. [循环]: 使 `c=d, d=r, t=a', a'=a, a=t-qa, t=b', b'=b, b=t-qb` ， 然后返回 到第2步。

假定 m = 1769 ， n = 551, 我们相继得到：

| a'   | a    | b'   | b    | c    | d    | q    | r    |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| 1    | 0    | 0    | 1    | 1769 | 551  | 3    | 116  |
| 0    | 1    | 1    | -3   | 551  | 116  | 4    | 87   |
| 1    | -4   | -3   | 13   | 116  | 87   | 1    | 29   |
| -4   | 5    | 13   | -16  | 87   | 29   | 3    | 0    |



我们可以算出 `gcd(1769, 551) = 29`.

现在我们已经了解了算法的步骤，现在再来分析一下，此算法为什么是对的？

```
执行第 1 步后:  a'= b = 1, a = b' = 0, c = m, d = n;

执行 第 4 步以前：
    a'm + b'n = c     -------- 等式(1)
    am + bn = d       ---------等式(2)

a'm + b'n = qd+r     		-----  c = qd + r
a'm + b'n - q(am+bn) = r   	-----  d = am + bn
(a'-qa)m + (b'-qb)n = r  --------等式(3)

执行第 4 步： c=d, d=r, t=a', a'=a, a=t-qa, t=b', b'=b, b=t-qb

等式(2) 变成  a'm + b'n = c
等式(3) 变成  am + bn = d
(PS: 这一步好好的看看，里面各个变量转换的顺序不能错。)

所以最后我们又变成了等式(1)和等式(2)，然后就又可以了新一步的循环了。


为了得到 等式(1) 和 等式 (2) 我们需要当 a' = b = 1, a = b' = 0, c = m, d = n 这样才符合要求。
为什么要得到等式(1) 和 等式(2) 呢？
因为 gcd(m, 0) = m,  gcd(0, n) = n。

也就是说：
根据gcd(m, 0) = m, gcd(0, n) = n 这个原理，
我们初始化  a' = b = 1, a = b' = 0, c = m, d = n
得到 等式(1) 和 等式(2)， 再根据 c = qd+r, d = am+bn 公式，得到等式 (3)。
为了进行下一组循环，使用第4步，又得到等式(1)和等式(2)。

```



非递归扩展欧几里得算法的代码可以看下面这个代码：

```python
def extgcd(m, n):
    # 用  ta, tb 表示  a' 和  b'
    ta = b = 1
    a = tb = 0
    c, d = m, n
    q, r = c//d, c%d
    while r:
        taa = ta - q * a
        tbb = tb - q * b
        ta, tb = a, b
        a, b = taa, tbb
        c, d = d, r
        q, r = c//d, c%d
        # print(ta, tb, a, b, c, d, q, r)
    return d, a, b
```



现在我们可以得到  **ax + by = m**  的一组解了。 但是对于我们的题目 **(x * a) % n = 1** 而言还有一点距离。

又 因为  a%n = a - a//n * n   令 a // n = y 则 a % n = a - y*n,  所以推导出  **(x * a) % n = x * a - y * n = 1** 

又 **k 可以为负数或者是正数**，上面的公式又可以转变为  **x*a + y * n = 1** , 这个时候，就和我们的  **ax+by=m**, 有一点点像了。



所以对于我们这个题目，先通过扩展欧几里得算法得到一组解。

假设现在得到的一组解为  **(x<sub>0</sub>, b<sub>0</sub>)**

因为要求最小正整数解，然而我们求出来的那一组解，可能不是最小的正整数解。于是就需要变化一下。

那么一直一组解如何求另外一组解呢？

公式：



> x = x<sub>0</sub> - b*t  

> y = y<sub>0</sub> + a*t

> 其中  t 为整数。



我们的目的是得到下一组的解，现在我们来简单验证一下，这个公式为啥可以？

```
ax0 + by0 = 1

带入公式：

a(x0-n*t) + b(y0+n*t) = 1

变形：
ax0 - a*b*t + by0 + b*a*t = 1

合并公因式：
ax0 + by0 + (b*a*t - a*b*t) = 1

结果：
ax0 + by0 = 1

这时  x0 和 y0 的值，随着  t 的变化而变化，t可以是负整数，也可以是正整数。
```



**代码:**

```python
def extgcd(m, n):
    # 用  ta, tb 表示  a' 和  b'
    ta = b = 1
    a = tb = 0
    c, d = m, n
    q, r = c//d, c%d
    while r:
        taa = ta - q * a
        tbb = tb - q * b
        ta, tb = a, b
        a, b = taa, tbb
        c, d = d, r
        q, r = c//d, c%d
        # print(ta, tb, a, b, c, d, q, r)
    return d, a, b

_, x0, y0 = extgcd(a, n)


t = 1


while x0 <= 0:
    x0 = x0 + n*t
    y0 = y0 - a*t
    t += 1

t=1
while True:
    if (x0 - n * t) <= 0:
        break
    
    x0 = x0 - n * t
    y0 = y0 + a * t
    t += 1

print(x0)
```
