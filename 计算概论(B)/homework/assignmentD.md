# Assignment #D: 十全十美 

Updated 1254 GMT+8 Dec 17, 2024

2024 fall, Complied by



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

2）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

3）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### 02692: 假币问题

brute force, http://cs101.openjudge.cn/practice/02692

思路：



代码：

```python
dic = 'ABCDEFGHIJKL'

m = int(input())
for _ in range(m):
    left = []; right = []; flag = []
    for _ in range(3):
        c = input()
        left.append(c[:4])
        right.append(c[5:9])
        if 'even' in c[10:]:
            flag.append(0)
        elif 'up' in c[10:]:
            flag.append(1)
        elif 'down' in c[10:]:
            flag.append(-1)
    ind = 12; w = 0
    for i in range(12):
        for weight in [-1, 1]:
            fla = True
            for j in range(3):
                if dic[i] in left[j] and flag[j] != weight:
                    fla = False
                if dic[i] in right[j] and flag[j] != -1 * weight:
                    fla = False
                if dic[i] not in left[j] and dic[i] not in right[j] and flag[j] != 0:
                    fla = False
            if fla:
                ind = i; w = weight
                break
        if fla:
            break
    if w == 1:
        print(str(dic[ind]) + ' is the counterfeit coin and it is heavy.')
    elif w == -1:
        print(str(dic[ind]) + ' is the counterfeit coin and it is light.')
```

![image-20241220161054897](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20241220161054897.png)



### 01088: 滑雪

dp, dfs similar, http://cs101.openjudge.cn/practice/01088

思路：并不困难，做一个记忆化搜索就可以，只是一开始用dp倒腾了一会发现确实搞不出来，耽误了会时间。用时较长可能是优化做的不够好？不是很懂这种。



代码：

```python
n, m = map(int, input().split())
heights = [list(map(int, input().split())) for _ in range(n)]

maxes = [[1] * m for _ in range(n)]
directions = [(0, 1), (0, -1), (-1, 0), (1, 0)]
def longest(s, t, i, j, step):
    if (i < s) or (i == s and j < t):
        maxes[s][t] = max(maxes[s][t], step - 1  + maxes[i][j])
        return
    for di in directions:
        ni, nj = i + di[0], j + di[1]
        if 0 <= ni < n and 0 <= nj < m and heights[ni][nj] < heights[i][j]:
            longest(s, t, ni, nj, step + 1)
    maxes[s][t] = max(maxes[s][t], step)
    return

ans = 0
for i in range(n):
    for j in range(m):
        longest(i, j, i, j, 1)
        ans = max(ans, maxes[i][j])
print(ans)
```

![image-20241220160851070](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20241220160851070.png)



### 25572: 螃蟹采蘑菇

bfs, dfs, http://cs101.openjudge.cn/practice/25572/

思路：



代码：

```python
from collections import deque

n = int(input())
grid = [list(map(int, input().split())) for _ in range(n)]

crab = []
target = ()
for i in range(n):
    for j in range(n):
        if grid[i][j] == 9:
            target = (i, j)
        elif grid[i][j] == 5:
            crab.append((i, j))


def bfs():
    directions = [(0, 1), (0, -1), (1, 0), (-1, 0)]
    q = deque()
    visited = set()
    q.append(crab)
    while q:
        pos1, pos2 = q.popleft()
        if pos1 == target or pos2 == target:
            return True
        visited.add((pos1, pos2))
        for di in directions:
            nx1, ny1, nx2, ny2 = pos1[0] + di[0], pos1[1] + di[1], pos2[0] + di[0], pos2[1] + di[1]
            if 0 <= nx1 < n and 0 <= ny1 < n and 0 <= nx2 < n and 0 <= ny2 < n:
                if grid[nx1][ny1] == 1 or grid[nx2][ny2] == 1 or ((nx1, ny1), (nx2, ny2)) in visited:
                    continue
                q.append([(nx1, ny1), (nx2, ny2)])
    return False


print('yes' if bfs() else 'no')
```

![image-20241220161143152](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20241220161143152.png)



### 27373: 最大整数

dp, http://cs101.openjudge.cn/practice/27373/

思路：这个与普通01背包的差别仅在于只有把数排好序后这些数的“价值”才是有定义的。



代码：

```python
#method 1
m = int(input())
n = int(input())
nums = list(map(str, input().split()))
nums = sorted(nums, key=lambda x: int(x), reverse=True)
dp = [[] for _ in range(m+1)]
get = [[False for _ in range(n)] for _ in range(m+1)]

for l in range(1,m + 1):
    temp = int(''.join(dp[l])) if ''.join(dp[l]) else -1
    ind = -1
    nu = -1
    got = -1
    for p in range(n):
        t = len(nums[p])
        if t > l or t == got or get[l - t][p]:
            continue
        got = t
        b = dp[l - t][:]
        for i in range(len(b) + 1):
            b.insert(i, nums[p])
            new = int(''.join(b))
            if new > temp:
                temp = new
                ind = i
                nu = p
            b = dp[l - t][:]
    get[l] = get[l - len(nums[nu])][:]
    if ind != -1:
        b = dp[l - len(nums[nu])][:]
        b.insert(ind, nums[nu])
        dp[l] = b[:]
        get[l][nu] = True

for q in range(m, -1, -1):
    if dp[q]:
        print(int(''.join(dp[q])))
        break
```

```python
#method 2
from functools import cmp_to_key

m = int(input())
n = int(input())
nums = list(map(str, input().split()))

def compare(a, b):
    t1 = int(a + b)
    t2 = int(b + a)
    if t1 > t2:
        return -1
    elif t1 < t2:
        return 1
    else:
        return 0

nums.sort(key=cmp_to_key(compare))
dp = [''] * (m + 1)
for num in nums:
    for l in range(m, len(num)-1, -1):
        a = int(dp[l]) if dp[l] != '' else 0
        b = int(dp[l-len(num)] + num)
        dp[l] = dp[l] if a > b else dp[l-len(num)] + num
print(dp[m])
```

![image-20241220164549512](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20241220164549512.png)



### 02811: 熄灯问题

brute force, http://cs101.openjudge.cn/practice/02811

思路：



代码：

```python
from copy import deepcopy

directions = [(0, 0), (1, 0), (-1, 0), (0, 1), (0, -1)]
def toggle(board, r, c, rows=5, cols=6):
    for dr, dc in directions:
        nr, nc = r + dr, c + dc
        if 0 <= nr < rows and 0 <= nc < cols:
            board[nr][nc] = 1 if board[nr][nc] == 0 else 0

def solve_lights_out(initial):
    rows = len(initial)
    cols = len(initial[0])
    for first_row in range(1 << cols):
        board = deepcopy(initial)
        solution = [[0] * cols for _ in range(rows)]
        for c in range(cols):
            if (first_row >> c) & 1:
                solution[0][c] = 1
                toggle(board, 0, c)
        for r in range(1, rows):
            for c in range(cols):
                if board[r - 1][c] == 1:
                    solution[r][c] = 1
                    toggle(board, r, c)
        if all(light == 0 for light in board[rows - 1]):
            return solution
    return None


initial = [list(map(int, input().split())) for _ in range(5)]
sol = solve_lights_out(initial)
if sol is not None:
    for row in sol:
        print(" ".join(map(str, row)))
```

![image-20241220171723371](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20241220171723371.png)



### 08210: 河中跳房子

binary search, greedy, http://cs101.openjudge.cn/practice/08210/

思路：和agressive cows完全一致。



代码：

```python
L, n, m = map(int, input().split())
stones = [int(input()) for _ in range(n)]

def judge(d):
    count = 0
    last = 0
    for i in range(n):
        if stones[i] - last < d:
            count += 1
        else:
            last = stones[i]
        if count > m:
            return False
    if L - last < d:
        count += 1
    return True if count <= m else False

def search(l, r):
    if l == r:
        return l
    elif l == r - 1:
        return r if judge(r) else l
    mid = (l + r) // 2
    if judge(mid):
        return search(mid, r)
    else:
        return search(l, mid)

print(search(0, L))
```

![image-20241220161221730](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20241220161221730.png)



## 2. 学习总结和收获

这次作业有之前的基础，除了熄灯问题之外做的都很顺利。熄灯问题的代码实在是……繁琐。

在练习leetcode上的题目，巩固基础。



