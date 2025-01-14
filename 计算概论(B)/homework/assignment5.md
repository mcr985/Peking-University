# Assignment #5: Greedy穷举Implementation

Updated 1939 GMT+8 Oct 21, 2024

2024 fall, Complied by



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### 04148: 生理周期

brute force, http://cs101.openjudge.cn/practice/04148

思路：



代码：

```python
k = 1
while True:
    a, b, c, d = map(int, input().split())
    if a == -1:
        exit(0)
    i = 0
    dat = c
    judge = True
    while True:
        if (dat-a)%23==0 and (dat-b)%28==0 and dat > d:
            print('Case ' + str(k) + ': the next triple peak occurs in ' + str(dat-d) + ' days.')
            judge = False
            break
        dat = c + 33 * i
        i += 1
    k += 1
```

![image-20241022165037508](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20241022165037508.png)



### 18211: 军备竞赛

greedy, two pointers, http://cs101.openjudge.cn/practice/18211

思路：



代码：

```python
p = int(input())
costs = list(map(int, input().split()))
costs.sort()
i1 = 0
i2 = len(costs) - 1
num = 0
rival = 0
while i1 <= i2:
    if i1 == i2:
        if p < costs[i1]:
            break
    if p >= costs[i1]:
        p -= costs[i1]
        num += 1
        i1 += 1
    else:
        if rival + 1 <= num:
            p += costs[i2]
            rival += 1
            i2 -= 1
        else:
            print(0)
            exit(0)
print(num-rival)
```

![image-20241022165140099](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20241022165140099.png)



### 21554: 排队做实验

greedy, http://cs101.openjudge.cn/practice/21554

思路：



代码：

```python
n = int(input())
times = list(map(int, input().split()))
for i in range(n):
    times[i] = [times[i], i + 1]
times = sorted(times, key=lambda x: (x[0], x[1]))
nums = [time[1] for time in times]
print(' '.join(map(str, nums)))
total = 0
for i in range(n):
    total += (n - i - 1) * times[i][0]
average = total / n
print('%.2f' % average)
```

![image-20241024151108211](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20241024151108211.png)



### 01008: Maya Calendar

implementation, http://cs101.openjudge.cn/practice/01008/

思路：



代码：

```python
months = ['pop', 'no', 'zip', 'zotz', 'tzec', 'xul', 'yoxkin', 'mol', 'chen', 'yax', 'zac',
          'ceh', 'mac', 'kankin', 'muan', 'pax', 'koyab', 'cumhu', 'uayet']
weeks = ['imix', 'ik', 'akbal', 'kan', 'chicchan', 'cimi', 'manik', 'lamat', 'muluk', 'ok',
         'chuen', 'eb', 'ben', 'ix', 'mem', 'cib', 'caban', 'eznab', 'canac', 'ahau']
n = int(input())
print(n)
for i in range(n):
    tmp = input().split()
    day, month, year = tmp[0][:-1], tmp[1], tmp[2]
    d = int(day)+1+months.index(month)*20+365*int(year)
    nd, y = d % 260, d // 260
    x, m = nd % 13, nd % 20
    if x == 0:
        x = 13
        if m == 0:
            y -= 1
    print(x, weeks[m-1], y)
```

![image-20241022165314663](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20241022165314663.png)



### 545C. Woodcutters

dp, greedy, 1500, https://codeforces.com/problemset/problem/545/C

思路：



代码：

```python

n = int(input())
trees = []
for i in range(n):
    trees.append(list(map(int, input().split())))
if n == 1:
    print(1)
    exit(0)
num = 2
occupy = False
for i in range(1, n-1):
    dl = trees[i][0] - trees[i-1][0]
    dr = trees[i+1][0] - trees[i][0]
    if occupy:
        dl -= trees[i-1][1]
    if dl > trees[i][1]:
        num += 1
        occupy = False
    elif dr > trees[i][1]:
        num += 1
        occupy = True
    else:
        occupy = False
print(num)
```

![image-20241024154325303](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20241024154325303.png)



### 01328: Radar Installation

greedy, http://cs101.openjudge.cn/practice/01328/

思路：



代码：

```python
def find_intervals(islands):
    intervals = []
    for island in islands:
        if island[1] > d:
            return False
        else:
            x = (d**2-island[1]**2) ** 0.5
            l = island[0] - x
            r = island[0] + x
            intervals.append([l, r])
    return intervals

def overlaps(islands):
    intervals = find_intervals(islands)
    if not intervals:
        return -1
    else:
        intervals = sorted(intervals, key=lambda x: x[1])
        ed = intervals[0][1]
        num = 1
        for i in range(n):
            if intervals[i][0] > ed:
                num += 1
                ed = intervals[i][1]
        return num

k = 0
while True:
    k += 1
    n, d = map(int, input().split())
    if (n, d) == (0, 0):
        exit(0)
    islands = []
    for i in range(n):
        islands.append(list(map(int, input().split())))
    print('Case', str(k) + ':', overlaps(islands))
    input()
```

![image-20241024145533449](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20241024145533449.png)



## 2. 学习总结和收获

跟进每日选做中，逐渐熟悉贪心题目的做法，慢慢有了一点对贪心策略的直觉





