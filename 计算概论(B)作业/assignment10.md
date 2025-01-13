# Assignment #10: dp & bfs

Updated 2 GMT+8 Nov 25, 2024

2024 fall, Complied by



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

2）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

3）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### LuoguP1255 数楼梯

dp, bfs, https://www.luogu.com.cn/problem/P1255

思路：



代码：

```python
#dp
n = int(input())

dp = [0] * (n + 1)
dp[0] = 1
for i in range(1, n + 1):
    dp[i] = dp[i - 1] + dp[i - 2]
print(dp[n])
```

```python
#bfs
from collections import deque

n = int(input())
count = [0] * (n + 1)
count[0] = 1
def bfs():
    q = deque()
    q.append(0)
    visited = {0}
    while q:
        s = q.popleft()
        if s + 1 <= n:
            s1 = s + 1
            count[s1] += count[s]
            if s1 not in visited:
                visited.add(s1)
                q.append(s1)
        if s + 2 <= n:
            s2 = s + 2
            count[s2] += count[s]
            if s2 not in visited:
                visited.add(s2)
                q.append(s2)
    return count[n]

print(bfs())
```

![image-20241126171854175](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20241126171854175.png)



### 27528: 跳台阶

dp, http://cs101.openjudge.cn/practice/27528/

思路：



代码：

```python
from collections import deque

n = int(input())
count = [0] * (n + 1)
count[0] = 1
def bfs():
    q = deque()
    q.append(0)
    visited = {0}
    while q:
        s = q.popleft()
        for i in range(1, n + 1):
            if s + i <= n:
                ns = s + i
                count[ns] += count[s]
                if ns not in visited:
                    visited.add(ns)
                    q.append(ns)
    return count[n]

print(bfs())
```

![image-20241126172415815](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20241126172415815.png)



### 474D. Flowers

dp, https://codeforces.com/problemset/problem/474/D

思路：



代码：

```python
t, k =map(int, input().split())
dp = [0] * (10**5 + 100)
for i in range(k):
    dp[i] = 1
mod = 10**9 + 7

def dynamic(a):
    for j in range(k, a + 1):
        dp[j] = (dp[j - 1] + dp[j - k]) % mod

def f_sum(a):
    f = [0] * (10**5 + 100)
    for i in range(1, a + 1):
        f[i] = (f[i - 1] + dp[i]) % mod
    return f

ma = 0; sa = []; sb = []
for _ in range(t):
    a, b = map(int, input().split())
    sa.append(a)
    sb.append(b)
    if b > ma:
        ma = b
dynamic(ma)
f = f_sum(ma)
for i in range(t):
    temp = f[sb[i]] - f[sa[i] - 1]
    if temp < 0:
        temp += mod
    print(temp)
```

![image-20241126174639718](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20241126174639718.png)



### LeetCode5.最长回文子串

dp, two pointers, string, https://leetcode.cn/problems/longest-palindromic-substring/

思路：



代码：

```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        n = len(s)
        ans = 0; final = ''
        for i in range(n):
            p = q = i
            m = i; t = i + 1
            temp1 = -1; temp2 = 0
            while p > -1 and q < n:
                if s[p] == s[q]:
                    p -= 1
                    q += 1
                    temp1 += 2
                else:
                    break
            while m > -1 and t < n:
                if s[m] == s[t]:
                    m -= 1
                    t += 1
                    temp2 += 2
                else:
                    break
            ans = max(ans, temp1, temp2)
            if ans == temp1:
                final = s[p + 1:q]
            elif ans == temp2:
                final = s[m + 1:t]
        return final
```

![image-20241126191257343](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20241126191257343.png)



### 12029: 水淹七军

bfs, dfs, http://cs101.openjudge.cn/practice/12029/

思路：



代码：

```python
import sys
from collections import deque

dx, dy = [0, 1, 0, -1], [-1, 0, 1, 0]
input = sys.stdin.read
data = input().split()
idx = 0
k = int(data[idx])
idx += 1
ans = []

def bfs(h, hw, m, n, tx, ty, x, y):
    q = deque([(x, y)])
    if h[x][y] <= h[tx][ty]: 
        return hw
    
    ht = h[x][y]
    hw[x][y] = ht
    
    while q:
        x, y = q.popleft()
        for j in range(4):
            nx, ny = x + dx[j], y + dy[j]
            if 0 <= nx < m and 0 <= ny < n and ht > hw[nx][ny] and ht > h[nx][ny]:
                hw[nx][ny] = ht
                q.append((nx, ny))
    
    return hw

for _ in range(k):
    m, n = map(int, data[idx:idx+2])
    idx += 2
    h = [list(map(int, data[i:i+n])) for i in range(idx, idx + m * n, n)]
    idx += m * n
    tx, ty = map(int, data[idx:idx+2])
    tx, ty = tx - 1, ty - 1
    idx += 2
    p = int(data[idx])
    idx += 1
    hw = [[0] * n for _ in range(m)]
    
    for _ in range(p):
        x, y = map(int, data[idx:idx+2])
        idx += 2
        x, y = x - 1, y - 1
        hw = bfs(h, hw, m, n, tx, ty, x, y)
    
    ans.append('Yes' if hw[tx][ty] > 0 else 'No')

sys.stdout.write('\n'.join(ans) + '\n')
```

![image-20241128230506450](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20241128230506450.png)



### 02802: 小游戏

bfs, http://cs101.openjudge.cn/practice/02802/

思路：我，我，我，我输入输出呀！！！！！！输出把segments打成了segment，直接贡献了我连续八次WA. 彻底怀疑人生，因为我检查不出算法的任何问题. 以及stdin是真的难用.

这道题也不是完全没有收获，主要是让我认识到剪枝的巨大作用.

这绝对是我提交次数最多的一道题，总共23次提交.



代码：

```python
#dfs
import sys
sys.setrecursionlimit(10**8)

directions = [(0, 1), (0, -1), (-1, 0), (1, 0)]
ans = 10 ** 9
def dfs(i, j, ey, ex, step, temp, visited, board):
    global ans
    if temp >= ans:
        return
    for di in directions:
        ni, nj = i + di[0], j + di[1]
        if ni == ey and nj == ex:
            n_temp = temp + 1 if di != step else temp
            ans = min(n_temp, ans)
            return
        if 0 <= ni <= h + 1 and 0 <= nj <= w + 1 and board[ni][nj] == ' ':
            if not visited[ni][nj]:
                visited[ni][nj] = True
                n_temp = temp + 1 if di != step else temp
                dfs(ni, nj, ey, ex, di, n_temp, visited, board)
                visited[ni][nj] = False

input = sys.stdin.read
data = input().splitlines()
idx = 0
final = []
p = 0
while idx < len(data):
    w, h = map(int, data[idx].split())
    idx += 1
    p += 1
    if w == 0 and h == 0:
        break
    final.append('Board ' + '#' + str(p) + ':')
    board = [' ' * (w + 2)]
    for _ in range(h):
        board.append(' ' + data[idx] + ' ')
        idx += 1
    board.append(' ' * (w + 2))
    k = 0
    while True:
        x1, y1, x2, y2 = map(int, data[idx].split())
        idx += 1
        k += 1
        if x1 == 0 and y1 == 0 and x2 == 0 and y2 == 0:
            break
        if board[y1][x1] != 'X' or board[y2][x2] != 'X':
            final.append(f'Pair {k}: impossible.')
            continue
        visited = [[False for _ in range(w + 2)] for _ in range(h + 2)]
        visited[y1][x1] = True
        ans = 10 ** 9
        dfs(y1, x1, y2, x2, (0, 0), 0, visited, board)
        final.append('Pair ' + str(k) + ': ' + str(ans) + ' segments.' if ans < 10 ** 8 else 'Pair ' + str(k) + ': ' + 'impossible.')
    final.append('')
sys.stdout.write('\n'.join(final)+'\n')
```

![image-20241126234419580](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20241126234419580.png)



```python
#bfs
import sys
from collections import deque

directions = [(0, 1), (0, -1), (-1, 0), (1, 0)]
def bfs(w, h, board, start, end):
    q = deque([(start[0], start[1], -1, 0)])
    visited = [[False] * (w + 2) for _ in range(h + 2)]
    visited[start[0]][start[1]] = True
    while q:
        y, x, step, segments = q.popleft()
        for i in range(4):
            ny, nx = y + directions[i][0], x + directions[i][1]
            n_segments = segments + 1 if i != step else segments
            while 0 <= ny < h + 2 and 0 <= nx < w + 2:
                if (ny, nx) == end:
                    return n_segments
                if board[ny][nx] == 'X':
                    break
                if not visited[ny][nx]:
                    visited[ny][nx] = True
                    q.append((ny, nx, i, n_segments))
                ny, nx = ny + directions[i][0], nx + directions[i][1]
    return -1

input = sys.stdin.read
data = input().splitlines()
idx = 0
final = []
p = 0
while idx < len(data):
    w, h = map(int, data[idx].split())
    idx += 1
    p += 1
    if w == 0 and h == 0:
        break
    final.append('Board ' + '#' + str(p) + ':')
    board = [' ' * (w + 2)]
    for _ in range(h):
        board.append(' ' + data[idx] + ' ')
        idx += 1
    board.append(' ' * (w + 2))
    k = 0
    while True:
        x1, y1, x2, y2 = map(int, data[idx].split())
        idx += 1
        k += 1
        if x1 == 0 and y1 == 0 and x2 == 0 and y2 == 0:
            break
        ans = bfs(w, h, board, (y1, x1), (y2, x2))
        final.append('Pair ' + str(k) + ': ' + str(ans) + ' segments.' if ans != -1 else 'Pair ' + str(k) + ': ' + 'impossible.')
    final.append('')
sys.stdout.write('\n'.join(final)+'\n')
```

![image-20241128224401264](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20241128224401264.png)



## 2. 学习总结和收获

最后两道题的输入输出真把人整红温了

（兄弟你敲个代码脸怎么这么红.jpg）

每日选做跟进中（dp感觉越写越顺手了），以及做了下github讲义上的题目。





