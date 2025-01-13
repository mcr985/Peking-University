# Assignment #C: 五味杂陈 

Updated 1148 GMT+8 Dec 10, 2024

2024 fall, Complied by



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

2）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

3）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### 1115. 取石子游戏

dfs, https://www.acwing.com/problem/content/description/1117/

思路：



代码：

```python
while True:
    a, b = map(int, input().split())
    if a == b == 0:
        break
    c = max(a, b)
    d = min(a, b)

    step = 1
    def euler(n, m):
        global step
        if n % m == 0:
            return step
        if n // m != 1:
            return step
        step *= -1
        return euler(m, n % m)

    judge = euler(c, d)
    print('win' if judge == 1 else 'lose')
```

![image-20241213160603172](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20241213160603172.png)



### 25570: 洋葱

Matrices, http://cs101.openjudge.cn/practice/25570

思路：



代码：

```python
n = int(input())
matrix = [list(map(int, input().split())) for _ in range(n)]

ans = -float('inf')
for d in range(n // 2):
    temp = 0
    for i in range(d, n - d):
        temp += matrix[d][i] + matrix[n - d - 1][i] + matrix[i][d] + matrix[i][n - d - 1]
    temp -= matrix[d][d] + matrix[n - d - 1][d] + matrix[d][n - d - 1] + matrix[n - d - 1][n - d - 1]
    ans = max(ans, temp)
if n % 2 == 1:
    ans = max(ans, matrix[n // 2][n // 2])
print(ans)
```

![image-20241213161847245](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20241213161847245.png)



### 1526C1. Potions(Easy Version)

greedy, dp, data structures, brute force, *1500, https://codeforces.com/problemset/problem/1526/C1

思路：hard version似乎要用最小堆做……之后学一下吧



代码：

```python
n = int(input())
potions = list(map(int, input().split()))
dp = [-float('inf')] * (n + 1)
dp[0] = 0

for i in range(n):
    for k in range(i + 1, 0, -1):
        if dp[k - 1] + potions[i] >= 0:
            dp[k] = max(dp[k], dp[k - 1] + potions[i])

for k in range(n, -1, -1):
    if dp[k] >= 0:
        print(k)
        break
```

![image-20241213180340651](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20241213180340651.png)



### 22067: 快速堆猪

辅助栈，http://cs101.openjudge.cn/practice/22067/

思路：加一个简单的单调栈即可



代码：

```python
stack = []
min_stack = []
while True:
    try:
        command = input()
        if command == 'pop':
            if len(stack) != 0:
                stack.pop()
                min_stack.pop()
        elif command[:4] == 'push':
            w = int(command[5:])
            stack.append(w)
            if len(min_stack) == 0:
                min_stack.append(w)
            elif w < min_stack[-1]:
                min_stack.append(w)
            else:
                min_stack.append(min_stack[-1])
        elif command == 'min':
            if len(min_stack) != 0:
                print(min_stack[-1])
    except EOFError:
        break
```

![image-20241213181254463](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20241213181254463.png)



### 20106: 走山路

Dijkstra, http://cs101.openjudge.cn/practice/20106/

思路：很标准的dijkstra……没什么好说的



代码：

```python
import heapq

m, n, p = map(int, input().split())
mount = [list(map(str, input().split())) for _ in range(m)]

directions = [(0, 1), (0, -1), (1, 0), (-1, 0)]
def dijkstra(s1, s2, t1, t2):
    dist = [[float('inf')] * n for _ in range(m)]
    dist[s1][s2] = 0
    pq = []
    heapq.heappush(pq, (0, (s1, s2)))
    visited = set()

    while pq:
        c_dist, pos = heapq.heappop(pq)
        if pos in visited:
            continue
        visited.add(pos)
        if pos == (t1, t2):
            return c_dist
        for di in directions:
            nr, nc = pos[0] + di[0], pos[1] + di[1]
            if 0 <= nr < m and 0 <= nc < n:
                if (nr, nc) in visited or mount[nr][nc] == '#':
                    continue
                n_dist = c_dist + abs(int(mount[nr][nc]) - int(mount[pos[0]][pos[1]]))
                if n_dist < dist[nr][nc]:
                    dist[nr][nc] = n_dist
                    heapq.heappush(pq, (n_dist, (nr, nc)))
    return float('inf')

for _ in range(p):
    sr, sc, tr, tc = map(int, input().split())
    if mount[sr][sc] == '#' or mount[tr][tc] == '#':
        print('NO')
        continue
    shortest = dijkstra(sr, sc, tr, tc)
    print(shortest if shortest != float('inf') else 'NO')
```

![image-20241213184911178](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20241213184911178.png)



### 04129: 变换的迷宫

bfs, http://cs101.openjudge.cn/practice/04129/

思路：



代码：

```python
from collections import deque

def find_pole():
    start = target = (0, 0)
    for i in range(r):
        for j in range(s):
            if danger[i][j] == 'S':
                start = (i, j)
                danger[i][j] = '.'
            if danger[i][j] == 'E':
                target = (i, j)
                danger[i][j] = '.'
    return start, target

directions = [(0, 1), (0, -1), (-1, 0), (1, 0)]

def bfs(s1, s2, t1, t2):
    q = deque()
    q.append((0, s1, s2)) 
    visited = set()
    visited.add((s1, s2, 0)) 

    while q:
        time, r_pos, c_pos = q.popleft()
        if (r_pos, c_pos) == (t1, t2):
            return time
        for di in directions:
            nr, nc = r_pos + di[0], c_pos + di[1]
            n_time = time + 1
            mod = n_time % k
            passable = False
            if 0 <= nr < r and 0 <= nc < s and (nr, nc, mod) not in visited:
                if danger[nr][nc] == '.':
                    passable = True
                elif danger[nr][nc] == '#':
                    if n_time % k == 0:
                        passable = True
                    else:
                        passable = False
                if passable:
                    visited.add((nr, nc, mod))
                    q.append((n_time, nr, nc))
    return -1

t = int(input())
for _ in range(t):
    r, s, k = map(int, input().split())
    danger = [list(input().strip()) for _ in range(r)]
    ori, ex = find_pole()
    m_time = bfs(ori[0], ori[1], ex[0], ex[1])
    print(m_time if m_time != -1 else 'Oop!')
```

![image-20241213201220750](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20241213201220750.png)



## 2. 学习总结和收获

这次作业整体并不困难，开始进行dp和搜索的专门刷题





