# Assignment #B: Dec Mock Exam大雪前一天

Updated 1649 GMT+8 Dec 5, 2024

2024 fall, Complied by



**说明：**

1）⽉考： AC2 。考试题⽬都在“题库（包括计概、数算题目）”⾥⾯，按照数字题号能找到，可以重新提交。作业中提交⾃⼰最满意版本的代码和截图。

2）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### E22548: 机智的股民老张

http://cs101.openjudge.cn/practice/22548/

思路：



代码：

```python
prices = list(map(int, input().split()))
n = len(prices)

ans = 0
maxes = [-1] * n
for i in range(1, n):
    maxes[i] = max(maxes[i - 1], prices[n - i])
maxes.reverse()
for i in range(n):
    ans = max(ans, maxes[i] - prices[i])
print(ans)
```

![image-20241210190121915](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20241210190121915.png)



### M28701: 炸鸡排

greedy, http://cs101.openjudge.cn/practice/28701/

思路：



代码：

```python
n, k = map(int, input().split())
t = list(map(int, input().split()))
t.sort()

f_sum = [0] * (n + 1)
for i in range(n):
    f_sum[i+1] = f_sum[i] + t[i]

if k == 1:
    T = f_sum[n]
    print(f"{T:.3f}")
    exit(0)

T = float('inf')
max_m = min(k-1, n)
for m in range(max_m + 1):
    if k - m == 0:
        continue
    current = f_sum[n - m]
    temp = current / (k - m)
    if temp < T:
        T = temp

print(f"{T:.3f}")
```





### M20744: 土豪购物

dp, http://cs101.openjudge.cn/practice/20744/

思路：属于做过完全类似但忘记了的题目……一看到双dp就想起来以前做过这种



代码：

```python
values = list(map(int, input().split(',')))
n = len(values)

dp1 = [0] * (n + 1) #all in
dp2 = [0] * (n + 1) #abandon one
for i in range(1, n + 1):
    dp1[i] = max(dp1[i - 1] + values[i - 1], values[i - 1])
    dp2[i] = max(dp1[i - 1], dp2[i - 1] + values[i - 1])
temp = max(max(dp1), max(dp2))
if temp == 0:
    print(max(values))
else:
    print(temp)
```

![image-20241210191047411](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20241210191047411.png)



### T25561: 2022决战双十一

brute force, dfs, http://cs101.openjudge.cn/practice/25561/

思路：oh, gosh……远比想象中的简单，把所有账单枚举出来brute force就好了



代码：

```python
n, m = map(int, input().split())
goods = [[] for _ in range(n)]
for i in range(n):
    ois = list(map(str, input().split()))
    for oi in ois:
        goods[i].append((int(oi[0]), int(oi[2:])))
tickets = [[] for _ in range(m)]
for i in range(m):
    ois = list(map(str, input().split()))
    for oi in ois:
        ind = 0
        for k in range(len(oi)):
            if oi[k] == '-':
                ind = k
                break
        tickets[i].append((int(oi[:ind]), int(oi[ind+1:])))
    tickets[i] = sorted(tickets[i], key=lambda x: x[0])

bills = []
def search(p, bill):
    if p == n:
        bills.append(bill)
        return
    for price in goods[p]:
        n_bill = bill[:]
        n_bill[price[0]-1] += price[1]
        search(p + 1, n_bill)
search(0, [0] * m)

ans = float('inf')
for bill in bills:
    total = sum(bill)
    total -= total//300 * 50
    for i in range(m):
        save = 0
        for ticket in tickets[i]:
            if ticket[0] > bill[i]:
                break
            save = max(save, ticket[1])
        total -= save
    ans = min(ans, total)
print(ans)
```

![image-20241210223750042](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20241210223750042.png)



### T20741: 两座孤岛最短距离

dfs, bfs, http://cs101.openjudge.cn/practice/20741/

思路：卡时间卡的有点太死了这道题……



代码：

```python
import sys
from collections import deque
sys.setrecursionlimit(10**8)

def read_input():
    n = int(sys.stdin.readline())
    grid = []
    for _ in range(n):
        line = sys.stdin.readline().strip()
        grid.append(list(line))
    return n, grid

n, sea = read_input()

directions = [(0, 1), (1, 0), (0, -1), (-1, 0)]
island = []
def find_island(i, j):
    sea[i][j] = '2'
    island.append((i,j))
    for di in directions:
        ni, nj = i + di[0], j + di[1]
        if 0 <= ni < n and 0 <= nj < n and sea[ni][nj] == '1':
            find_island(ni, nj)

flag = False
for i in range(n):
    for j in range(n):
        if sea[i][j] == '1':
            flag = True
            find_island(i, j)
            break
    if flag:
        break

def expand():
    q = deque()
    visited = set()
    for get in island:
        q.append((get[0], get[1], 0))
        visited.add(get)
    while q:
        k, l, s = q.popleft()
        for di in directions:
            nk, nl = k + di[0], l + di[1]
            if 0 <= nk < n and 0 <= nl < n and (nk, nl) not in visited:
                if sea[nk][nl] == '0':
                    visited.add((nk, nl))
                    q.append((nk, nl, s + 1))
                elif sea[nk][nl] == '1':
                    return s

print(expand())
```

![image-20241210200117048](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20241210200117048.png)



### T28776: 国王游戏

greedy, http://cs101.openjudge.cn/practice/28776

思路：



代码：

```python
n = int(input())
a, b = map(int, input().split())
king = (a, b)
nums = []
for _ in range(n):
    a, b = map(int, input().split())
    nums.append((a, b))

product = king[0]
for i in range(n):
    product *= nums[i][0]

ans = float('inf')
for i in range(n):
    ans = min(ans, product // (nums[i][0] * nums[i][1]))
print(ans)
```

![image-20241210190144502](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20241210190144502.png)



## 2. 学习总结和收获

人有点麻。这次暴露出来的问题是练习频率太低，之前做过的题忘了……本来应当是可以AC3的，土豪购物这种题做过后就没有什么难度了，根据不同的状态的数量增加dp数组的个数就可以。

仍然不是很适应考场节奏，很容易慌神（大概是不像数学和物理扫一遍就能知道难度从而稳定心神的原因）（而且计概不像高数线代大把练数分高代就完了，我总不能去做oi题吧）（汗）
