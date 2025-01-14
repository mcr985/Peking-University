# Assignment #8: 田忌赛马来了

Updated 1021 GMT+8 Nov 12, 2024

2024 fall, Complied by



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

2）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

3）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### 12558: 岛屿周⻓

matices, http://cs101.openjudge.cn/practice/12558/ 

思路：



代码：

```python
n, m = map(int, input().split())
matrix = []
for _ in range(n):
    oi = list(map(int, input().split()))
    matrix.append(oi + [0])

island = []
for i in range(n):
    l, r = -1, -1
    for j in range(m + 1):
        if matrix[i][j] == 1 and l == -1:
            l = j
        if matrix[i][j] == 0 and l != -1:
            r = j - 1
            break
    if l != -1 and r != -1:
        island.append([l, r])
length = len(island)
c = island[0][1] - island[0][0] + island[length - 1][1] - island[length - 1][0] + 2 + 2 * length
for r in range(1, length):
    c += abs(island[r][0] - island[r - 1][0]) + abs(island[r][1] - island[r - 1][1])
print(c)
```

![image-20241112163317202](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20241112163317202.png)



### LeetCode54.螺旋矩阵

matrice, https://leetcode.cn/problems/spiral-matrix/

与OJ这个题目一样的 18106: 螺旋矩阵，http://cs101.openjudge.cn/practice/18106

思路：



代码：

```python
n = int(input())
matrix = [[0 for _ in range(n)] for _ in range(n)]

k = 1
i = 0; j = -1
p = 0; q = 1
while k <= n ** 2:
    if i + p == -1 or j + q == -1 or i + p == n or j + q == n or matrix[i + p][j + q] != 0:
        if (p, q) == (0, 1):
            p, q = 1, 0
        elif (p, q) == (1, 0):
            p, q = 0, -1
        elif (p, q) == (0, -1):
            p, q = -1, 0
        elif (p, q) == (-1, 0):
            p, q = 0, 1
    i += p; j += q
    matrix[i][j] = k
    k += 1
for l in range(n):
    print(' '.join(map(str, matrix[l])))
```

![image-20241112164400910](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20241112164400910.png)



### 04133:垃圾炸弹

matrices, http://cs101.openjudge.cn/practice/04133/

思路：



代码：

```python
d = int(input())
n = int(input())
garbage_x = []; garbage_y = []; garbage_n = []
for i in range(n):
    x, y, i = map(int, input().split())
    garbage_x.append(x)
    garbage_y.append(y)
    garbage_n.append(i)

def clear(i, j):
    num = 0
    for k in range(n):
        if abs(garbage_x[k] - i) <= d and abs(garbage_y[k] - j) <= d:
            num += garbage_n[k]
    return num

def scan():
    x_max = max(garbage_x) + d
    y_max = max(garbage_y) + d
    x_min = min(garbage_x) - d
    y_min = min(garbage_y) - d
    n_max = 0
    num = 0
    for i in range(max(0, x_min), min(1025, x_max+1)):
        for j in range(max(0, y_min), min(1025, y_max+1)):
            if clear(i, j) > n_max:
                num = 1
                n_max = clear(i, j)
            elif clear(i, j) == n_max:
                num += 1
    return num, n_max

print(' '.join(map(str, scan())))
```

![image-20241112164917534](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20241112164917534.png)



### LeetCode376.摆动序列

greedy, dp, https://leetcode.cn/problems/wiggle-subsequence/

与OJ这个题目一样的，26976:摆动序列, http://cs101.openjudge.cn/routine/26976/

思路：虽然tags是greedy，但还是直接dp更快一些……oj数据比较弱也就直接过了



代码：

```python
n = int(input())
nums = list(map(int, input().split()))

dp = [[1, 0] for _ in range(n)]
for i in range(n):
    for j in range(i):
        minus = nums[i] - nums[j]
        if minus != 0 and minus * dp[j][1] <= 0:
            if dp[j][0] + 1 > dp[i][0]:
                dp[i] = [dp[j][0] + 1, minus]
print(dp[n - 1][0])
```

![image-20241112172815786](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20241112172815786.png)

```
#greedy
def longest_wiggle_subsequence(n, nums):
    if n < 2:
        return n
    
    up = down = 1
    for i in range(1, n):
        if nums[i] > nums[i - 1]:
            up = down + 1
        elif nums[i] < nums[i - 1]:
            down = up + 1
            
    return max(up, down)

n = int(input())
nums = list(map(int, input().split()))

print(longest_wiggle_subsequence(n, nums))
```

这个题可以直接使用贪心解决的关键是数列的交替性质是被自动维护了的，不需要记忆化搜索来手动维护。比如nums[i]-nums[i-1] > 0，如果前面的nums[i - 1]……nums[i - j + 1]都不是交替的，那么它就一定是递增的，必有nums[i] - nums[i - j] > 0，因此去掉nums[i - 1]到nums[i - j + 1]后数列的交替性质依然不变。



### CF455A: Boredom

dp, 1500, https://codeforces.com/contest/455/problem/A

思路：



代码：

```python
n = int(input())
array = list(map(int, input().split()))

sta = [0] * 10**6
nums = []
for num in array:
    if sta[num] == 0:
        nums.append(num)
    sta[num] += num

m = max(nums)
dp = [0] * (m + 1)
for i in range(1, m + 1):
    dp[i] = max(dp[i - 1], dp[i - 2] + sta[i])
print(dp[m])
```

![image-20241112202641449](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20241112202641449.png)



### 02287: Tian Ji -- The Horse Racing

greedy, dfs http://cs101.openjudge.cn/practice/02287

思路：相当的tricky，不知道有没有更加general的分析方式？



代码：

```python
while True:
    n = int(input())
    if n == 0:
        break
    tian = list(map(int, input().split()))
    king = list(map(int, input().split()))
    tian.sort(reverse=True)
    king.sort(reverse=True)
    final = -10 ** 9
    for j in range(n):
        temp = 0
        for i in range(n):
            if tian[i] > king[(i + j) % n]:
                temp += 200
            elif tian[i] < king[(i + j) % n]:
                temp -= 200
        if temp > final:
            final = temp
    print(final)
```

![image-20241112191830332](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20241112191830332.png)



## 2. 学习总结和收获

最后一题借助了浏览器的帮助……相当tricky的贪心，权当积累经验了，对于这种binary match的题。



