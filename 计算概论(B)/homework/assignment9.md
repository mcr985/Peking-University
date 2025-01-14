# Assignment #9: dfs, bfs, & dp

Updated 2107 GMT+8 Nov 19, 2024

2024 fall, Complied by 



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

2）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

3）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### 18160: 最大连通域面积

dfs similar, http://cs101.openjudge.cn/practice/18160

思路：



代码：

```python
t = int(input())
for _ in range(t):
    n, m = map(int, input().split())
    board = []
    for _ in range(n):
        board.append(list(input()))
    direction = [[0, 1], [-1, 0], [1, 0], [0, -1], [1, 1], [-1, -1], [-1, 1], [1, -1]]

    def dfs(i, j):
        global temp
        board[i][j] = '.'
        for di in direction:
            x, y = i + di[0], j + di[1]
            if 0 <= x < n and 0 <= y < m:
                if board[x][y] == 'W':
                    temp += 1
                    dfs(x, y)

    ans = 0
    for i in range(n):
        for j in range(m):
            if board[i][j] == 'W':
                temp = 1
                dfs(i, j)
                if temp > ans:
                    ans = temp
    print(ans)
```

![image-20241121155121873](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20241121155121873.png)



### 19930: 寻宝

bfs, http://cs101.openjudge.cn/practice/19930

思路：



代码：

```python
from collections import deque

m, n = map(int, input().split())
grid = [list(map(int, input().split())) for _ in range(m)]
directions = [(-1, 0), (1, 0), (0, -1), (0, 1)]

def bfs(m, n, grid):
    if grid[0][0] == 2:
        return 'NO'
    queue = deque([(0, 0, 0)])
    visited = [[False] * n for _ in range(m)]
    visited[0][0] = True
    while queue:
        x, y, dist = queue.popleft()
        if grid[x][y] == 1:
            return dist
        for di in directions:
            nx, ny = x + di[0], y + di[1]
            if 0 <= nx < m and 0 <= ny < n and not visited[nx][ny] and grid[nx][ny] != 2:
                queue.append((nx, ny, dist + 1))
                visited[nx][ny] = True
    return 'NO'

print(bfs(m, n, grid))
```

![image-20241121175525349](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20241121175525349.png)



### 04123: 马走日

dfs, http://cs101.openjudge.cn/practice/04123

思路：



代码：

```python
t = int(input())
for _ in range(t):
    n, m, x, y = map(int, input().split())

    direction = [[2, 1], [1, 2], [2, -1], [-1, 2], [-1, -2], [-2, -1], [-2, 1], [1, -2]]
    visited = [[False for _ in range(m)] for _ in range(n)]
    visited[x][y] = True

    ans = 0
    def dfs(i, j, steps):
        global ans
        if steps == n * m:
            ans += 1
            return
        for di in direction:
            p, q = i + di[0], j + di[1]
            if 0 <= p < n and 0 <= q < m:
                if not visited[p][q]:
                    visited[p][q] = True
                    dfs(p, q, steps + 1)
                    visited[p][q] = False

    dfs(x, y, 1)
    print(ans)
```

![image-20241121160157370](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20241121160157370.png)



### sy316: 矩阵最大权值路径

dfs, https://sunnywhy.com/sfbj/8/1/316

思路：



代码：

```python
n, m = map(int, input().split())
matrix = []
for _ in range(n):
    matrix.append(list(map(int, input().split())))

direction = [[1, 0], [0, 1], [-1, 0], [0, -1]]
visited = [[False for _ in range(m)] for _ in range(n)]

ans = -10 ** 15
final = []
def dfs(i, j, temp, steps):
    global ans, final
    visited[i][j] = True
    new = temp + matrix[i][j]
    n_steps = steps + [str(i + 1) + ' ' + str(j + 1)]
    if i == n - 1 and j == m - 1:
        if new > ans:
            ans = new
            final = n_steps[:]
        visited[i][j] = False
        return
    for di in direction:
        p, q = i + di[0], j + di[1]
        if 0 <= p < n and 0 <= q < m:
            if not visited[p][q]:
                dfs(p, q, new, n_steps)
    visited[i][j] = False

dfs(0, 0, 0, [])
for i in range(len(final)):
    print(final[i])
```

![image-20241121161248083](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20241121161248083.png)





### LeetCode62.不同路径

dp, https://leetcode.cn/problems/unique-paths/

思路：虽然套dfs模板很快，但还是用dp做试一下，弄出不同的做法还是很有益处的.

 dfs竟然会超时.



代码：

```python
#dp
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        dp = [[0 for _ in range(n+1)] for _ in range(m+1)]
        dp[1][0] = 1

        for i in range(1, m+1):
            for j in range(1, n+1):
                dp[i][j] = dp[i - 1][j] + dp[i][j - 1]
        return dp[m][n]
```

```python
#dfs
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        direction = [[1, 0], [0, 1]]
        visited = [[False for _ in range(n)] for _ in range(m)]
        visited[0][0] = True

        global ans
        ans = 0
        def dfs(i, j):
            global ans
            if i == m - 1 and j == n - 1:
                ans += 1
                return
            for di in direction:
                p, q = i + di[0], j + di[1]
                if 0 <= p < m and 0 <= q < n:
                    if not visited[p][q]:
                        visited[p][q] = True
                        dfs(p, q)
                        visited[p][q] = False

        dfs(0, 0)
        return ans
```

![image-20241121162959874](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20241121162959874.png)





### sy358: 受到祝福的平方

dfs, dp, https://sunnywhy.com/sfbj/8/3/539

思路：同样写dfs和dp两种做法

dp讨论极其炸裂。dp[i]指输入数字num中，将前i个数字构成的数，劈裂成可能的最小的（最碎的）完全平方数的列表。对每个dp[i]只有最后一个数可以不是完全平方数。对dp[i+1]，递推公式为从后往前遍历dp[i]，将其后p个（p = 1, 2, 3 ... len(dp[i-1])) 数与num[i+1]接在一起，看是否是完全平方数。若是，则dp[i+1]就是dp[i]剩下的前面的数与这个新完全平方数合并的列表。若遍历完dp[i]也没有一个完全平方数，则把dp[i]最后一个数（若不是完全平方数）与num[i+1]合并起来，与dp[i]前面的数合并成为dp[i+1]；若dp[i]最后一个数是完全平方数，则直接合并dp[i]和num[i]得到dp[i+1]。

这么做就始终维护了每个dp[i]都是最碎最完全的分割，并且每个dp[i]只有列表中最后一个数不是完全平方数的性质。如此从1到l遍历完num，则看dp[l]最后一个数是否是完全平方数。若是，则num的分割存在，输出'Yes';若不是，则输出'No'

这破题不承认0=0^2是有效的分解，就不能直接从p=0开始遍历。因为num[i+1]是0时优先和dp[i-1]拼一起（0不是有效分解）;num[i+1]是非0完全平方数则优先割开，二者需要分开讨论。故多一个elif judge(dp[i-1]【-1】) and judge(num[i]) and int(num[i]) != 0的判断。

用dp做感觉这个题直接变成tough，但也很有可能是我想太复杂了。

代码：

```python
#dfs
import math as m

a = input()
n = len(a)

def cut(k):
    for i in range(k + 1, n + 1):
        num = int(a[k:i])
        if num != 0 and m.sqrt(num) == int(m.sqrt(num)):
            if i == n:
                print('Yes')
                exit(0)
            else:
                cut(i)

cut(0)
print('No')
```

![image-20241121163101825](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20241121163101825.png)

```python
#dp
import math as m

num = input()
l = len(num)
dp = [[] for _ in range(l)]
dp[0] += [num[0]]

def judge(s):
    if m.sqrt(int(s)) == int(m.sqrt(int(s))):
        return True
    else:
        return False

for i in range(1, l):
    flag = False
    for p in range(len(dp[i-1])-1, -1, -1):
        temp = ''.join(dp[i - 1][p:]) + num[i]
        if judge(temp) and int(temp) != 0:
            dp[i] = dp[i - 1][:p] + [temp]
            flag = True
            break
    if judge(dp[i - 1][-1]) and not flag:
        dp[i] = dp[i - 1][:] + [num[i]]
    elif judge(dp[i - 1][-1]) and judge(num[i]) and int(num[i]) != 0:
        dp[i] = dp[i - 1][:] + [num[i]]
    elif not flag:
        t = dp[i - 1].pop()
        dp[i] = dp[i - 1][:] + [t + num[i]]

if judge(dp[-1][-1]) and int(dp[-1][-1]) != 0:
    print('Yes')
else:
    print('No')
```

![image-20241121172155738](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20241121172155738.png)

## 2. 学习总结和收获

这次作业两个多小时解决。一个小时解决所有bfs和dfs，剩下一个多小时想完全平方数的dp。每次看wa的测试数据做修修补补debug，总是算是把dp改成了ac。必须承认dp是这个学期最难的内容。





