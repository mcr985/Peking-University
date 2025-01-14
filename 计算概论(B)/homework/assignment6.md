# Assignment #6: Recursion and DP

Updated 2201 GMT+8 Oct 29, 2024

2024 fall, Complied by



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### sy119: 汉诺塔

recursion, https://sunnywhy.com/sfbj/4/3/119  

思路：



代码：

```python
from copy import deepcopy

def hanoi(n):
    if n == 1:
        return [['A', 'C']]
    else:
        a = hanoi(n-1)
        b = deepcopy(a)
        for i in range(len(a)):
            for j in range(2):
                if a[i][j] == 'B':
                    a[i][j] = 'C'
                elif a[i][j] == 'C':
                    a[i][j] = 'B'
                if b[i][j] == 'A':
                    b[i][j] = 'B'
                elif b[i][j] == 'B':
                    b[i][j] = 'A'
        return a + [['A', 'C']] + b

n = int(input())
t = hanoi(n)
print(len(t))
for o in t:
    print(o[0] + '->' + o[1])
```

![image-20241103213959849](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20241103213959849.png)



### sy132: 全排列I

recursion, https://sunnywhy.com/sfbj/4/3/132

思路：



代码：

```python
judges = [False] * 20

def full_permutation(n, permutation):
    if len(permutation) == n:
        return [permutation]
    results = []
    for i in range(1, n+1):
        if judges[i]:
            continue
        judges[i] = True
        results += full_permutation(n, permutation + [i])
        judges[i] = False
    return results

n = int(input())
results = full_permutation(n, [])
for result in results:
    print(' '.join(map(str, result)))
```

![image-20241103220507677](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20241103220507677.png)



### 02945: 拦截导弹 

dp, http://cs101.openjudge.cn/2024fallroutine/02945

思路：



代码：

```python
k = int(input())
bombs = list(map(int, input().split()))
dp = [1] * k
for i in range(k):
    maximum = 1
    for j in range(i):
        if bombs[i] <= bombs[j]:
            if 1 + dp[j] > maximum:
                maximum = 1 + dp[j]
    dp[i] = maximum
print(max(dp))
```

![image-20241103220629029](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20241103220629029.png)



### 23421: 小偷背包 

dp, http://cs101.openjudge.cn/practice/23421

思路：



代码：

```python
import math as m

n, b = map(int, input().split())
values = list(map(int, input().split()))
weights = list(map(float, input().split()))

unit = min(weights)
l = int(b // unit)
p_weights = [unit * i for i in range(1, l+1)]
dp = [[0 for _ in range(l+1)] for _ in range(n+1)]
for i in range(1, n+1):
    for j in range(1, l+1):
        if weights[i-1] > p_weights[j-1]:
            dp[i][j] = dp[i-1][j]
        elif weights[i-1] == p_weights[j-1]:
            dp[i][j] = max(dp[i-1][j], values[i-1])
        else:
            more = p_weights[j-1] - weights[i-1]
            adds = values[i-1] + dp[i-1][p_weights.index(more)+1]
            dp[i][j] = max(dp[i-1][j], adds)
print(dp[n][l])
```

![image-20241103224426157](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20241103224426157.png)



### 02754: 八皇后

dfs and similar, http://cs101.openjudge.cn/practice/02754

思路：



代码：

```python
cols = [False] * 9
l_d = [False] * 15
r_d = [False] * 15

def queens(k, array):
    if k == 9:
        return [array]
    results = []
    for col in range(1, 9):
        if cols[col] or l_d[col-k+7] or r_d[col+k-2]:
            continue
        cols[col] = True
        l_d[col-k+7] = True
        r_d[col+k-2] = True
        results += queens(k+1, array+[col])
        cols[col] = False
        l_d[col-k+7] = False
        r_d[col+k-2] = False
    return results

results = queens(1, [])
n = int(input())
for _ in range(n):
    t = int(input())
    print(''.join(map(str, results[t-1])))
```

![image-20241103233225164](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20241103233225164.png)



### 189A. Cut Ribbon 

brute force, dp 1300 https://codeforces.com/problemset/problem/189/A

思路：



代码：

```python
n, a, b, c = map(int, input().split())
dp = [[0 for _ in range(n+1)] for _ in range(4)]
ls = sorted([0, a, b, c])
for w in range(1, n+1):
    if w % ls[1] == 0:
        dp[1][w] = w // ls[1]
for j in range(1, n+1):
    k = 0
    while j - ls[2] * k >= 0:
        m = j - ls[2] * k
        if dp[1][m] != 0 or m == 0:
            dp[2][j] = k + dp[1][m]
            break
        k += 1
k = 0
maximum = 0
while n - ls[3] * k >= 0:
    m = n - ls[3] * k
    if dp[2][m] != 0 or m == 0:
        if k + dp[2][m] > maximum:
            maximum = k + dp[2][m]
    k += 1
print(maximum)
```

![image-20241104091649236](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20241104091649236.png)



## 2. 学习总结和收获

dp和recursion不再像之前直接就能上手题目了，需要看教材学习思想和基础题目的代码实现。作业整体较为顺利，因为都是基础题没有出现更多变式。小偷背包在计算子背包步长上纠结了一段时间，但oj数据很弱，直接取步长为最小重量就过了，更强的数据用递归可能更方便。最后一题直接套二维网格递推的模板为 $$O(n^2)$$ 复杂度，会超时，故做了一些简单的优化，代价是代码繁琐了很多。









