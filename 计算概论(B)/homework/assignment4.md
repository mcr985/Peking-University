# Assignment #4: T-primes + 贪心

Updated 0337 GMT+8 Oct 15, 2024

2024 fall, Complied by 



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）课程网站是Canvas平台, https://pku.instructure.com, 学校通知9月19日导入选课名单后启用。**作业写好后，保留在自己手中，待9月20日提交。**

提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### 34B. Sale

greedy, sorting, 900, https://codeforces.com/problemset/problem/34/B



思路：



代码

```python
n, m = map(int, input().split())
earns = []
for price in input().split():
    if int(price) < 0:
        earns.append(-int(price))
final = 0
if len(earns) <= m:
    final = sum(earns)
else:
    earns.sort(reverse=True)
    for i in range(m):
        final += earns[i]
print(final)

```

![image-20241020150021959](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20241020150021959.png)



### 160A. Twins

greedy, sortings, 900, https://codeforces.com/problemset/problem/160/A

思路：



代码

```python
n = int(input())
coins = list(map(int, input().split()))
total = sum(coins)
coins.sort(reverse=True)
get = 0
i = 0
while 2 * get <= total:
    get += coins[i]
    i += 1
print(i)

```

![image-20241020151007638](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20241020151007638.png)



### 1879B. Chips on the Board

constructive algorithms, greedy, 900, https://codeforces.com/problemset/problem/1879/B

思路：



代码

```python
t = int(input())
for i in range(t):
    n = int(input())
    a = [int(x) for x in input().split()]
    b = [int(x) for x in input().split()]
    final1 = sum(b) + min(a) * n
    final2 = sum(a) + min(b) * n
    print(min(final1, final2))

```

![image-20241020150115469](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20241020150115469.png)



### 158B. Taxi

*special problem, greedy, implementation, 1100, https://codeforces.com/problemset/problem/158/B

思路：



代码

```python
n = int(input())
groups = list(map(int, input().split()))
sta = [0, 0, 0, 0]
num = 0
for g in groups:
    if g == 4:
        sta[3] += 1
    elif g == 3:
        sta[2] += 1
    elif g == 2:
        sta[1] += 1
    elif g == 1:
        sta[0] += 1
num += sta[3]
num += sta[2]
sta[0] = max(0, sta[0]-sta[2])
num += sta[1] // 2
if sta[1] % 2 == 1:
    num += 1
    sta[0] = max(0, sta[0]-2)
num += sta[0] // 4
if sta[0] % 4 != 0:
    num += 1
print(num)

```

![image-20241020150154742](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20241020150154742.png)



### *230B. T-primes（选做）

binary search, implementation, math, number theory, 1300, http://codeforces.com/problemset/problem/230/B

思路：



代码

```python
limit = 1000002
def calculate():
    pris = []
    not_p = [False] * (limit + 10)
    for i in range(2, limit):
        if not not_p[i]:
            pris.append(i)
        for pri in pris:
            if i * pri > limit:
                break
            not_p[i * pri] = True
            if i % pri == 0:
                break
    return not_p
 
def check(n):
    if n == 4:
        return True
    if n < 4:
        return False
    sqrt_n = n ** 0.5
    if sqrt_n == int(sqrt_n):
        if not flags[int(sqrt_n)]:
            return True
    return False
 
flags = calculate()
n = int(input())
xs = list(map(int,input().split()))
for x in xs:
    if check(x):
        print("YES")
    else:
        print("NO")

```

![image-20241020165253745](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20241020165253745.png)



### *12559: 最大最小整数 （选做）

greedy, strings, sortings, http://cs101.openjudge.cn/practice/12559

思路：emm……比起字典序排序啥的，可能回归初心，把两个数直接用字符串类型拼一起再转化成整数直接比较更快捷一些，虽然显得有点逃课……但这应该叫利用Python的特性来简化问题



代码

```python
n = int(input())
nums = list(map(str, input().split()))

def compare(a, b):
    c = a + b
    d = b + a
    if int(c) > int(d):
        return True
    else:
        return False

def qsort(nums):
    l = len(nums)
    if l <= 1:
        return nums
    c = []; d = []
    for i in range(l):
        if i != l // 2:
            if compare(nums[l//2], nums[i]):
                c.append(nums[i])
            else:
                d.append(nums[i])
    return qsort(c) + [nums[l//2]] + qsort(d)

so = qsort(nums)
print(''.join(so[::-1]), ''.join(so))

```

![image-20241020155910761](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20241020155910761.png)





## 2. 学习总结和收获

每日选做的跟进越来越花时间了，显然是因为题目难度的上涨。作业里面T-primes花了不少时间，各种方法都试了一遍全部超时，最终是借鉴了other people's code...也不知道为什么原本用欧拉筛都超时，但一把代码封装成函数就直接过了，def的威力简直不可思议。



