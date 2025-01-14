# Assignment #7: Nov Mock Exam立冬

Updated 1646 GMT+8 Nov 7, 2024

2024 fall, Complied by



**说明：**

1）⽉考： AC5 。考试题⽬都在“题库（包括计概、数算题目）”⾥⾯，按照数字题号能找到，可以重新提交。作业中提交⾃⼰最满意版本的代码和截图。

2）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### E07618: 病人排队

sorttings, http://cs101.openjudge.cn/practice/07618/

思路：



代码：

```python
n = int(input())
elders = []
young = []
for i in range(n):
    Id, age = map(str, input().split())
    age = int(age)
    if age < 60:
        young.append([Id, i])
    else:
        elders.append([Id, -1 * age, i])
elders = sorted(elders, key=lambda x: (x[1], x[2]))
young = sorted(young, key=lambda x: x[1])
for patient in elders:
    print(patient[0])
for patient in young:
    print(patient[0])
```

![image-20241108141637472](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20241108141637472.png)



### E23555: 节省存储的矩阵乘法

implementation, matrices, http://cs101.openjudge.cn/practice/23555/

思路：



代码：

```python
n, m1, m2 = map(int, input().split())
A = [[0 for _ in range(n)] for _ in range(n)]
B = [[0 for _ in range(n)] for _ in range(n)]
for _ in range(m1):
    r, c, v = map(int, input().split())
    A[r][c] = v
for _ in range(m2):
    r, c, v = map(int, input().split())
    B[r][c] = v
ans = []
for i in range(n):
    for j in range(n):
        temp = 0
        for k in range(n):
            temp += A[i][k] * B[k][j]
        if temp != 0:
            ans.append([i, j, temp])
ans = sorted(ans, key=lambda x: (x[0], x[1]))
for a in ans:
    print(' '.join(map(str, a)))
```

![image-20241108141716079](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20241108141716079.png)



### M18182: 打怪兽 

implementation/sortings/data structures, http://cs101.openjudge.cn/practice/18182/

思路：



代码：

```python
t = int(input())
for _ in range(t):
    n, m, h = map(int, input().split())
    skills = []
    for _ in range(n):
        t, x = map(int, input().split())
        skills.append([t, -1 * x])
    skills = sorted(skills, key=lambda p: (p[0], p[1]))
    flag = True
    time = skills[0][0]
    left = m
    for i in range(len(skills)):
        if skills[i][0] != time:
            time = skills[i][0]
            left = m
        if left != 0:
            h += skills[i][1]
            left -= 1
        if h <= 0:
            print(time)
            flag = False
            break
    if flag:
        print('alive')
```

![image-20241108141739766](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20241108141739766.png)



### M28780: 零钱兑换3

dp, http://cs101.openjudge.cn/practice/28780/

思路：



代码：

```python
n, target = map(int, input().split())
coins = [0] + list(map(int, input().split()))
dp = [0] + [10 ** 8 for _ in range(1, target + 1)]

for j in range(1, n + 1):
    for t in range(1, target + 1):
        dp[t] = min(dp[t], dp[t - coins[j]] + 1)

if dp[target] > 10 ** 6:
    print(-1)
else:
    print(dp[target])
```

![image-20241108141807442](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20241108141807442.png)



### T12757: 阿尔法星人翻译官

implementation, http://cs101.openjudge.cn/practice/12757

思路：



代码：

```python
dic = {'zero': 0, 'one': 1, 'two': 2, 'three': 3, 'four': 4, 'five': 5, 'six': 6, 'seven': 7, 'eight': 8,
       'nine': 9, 'ten': 10, 'eleven': 11, 'twelve': 12, 'thirteen': 13, 'fourteen': 14, 'fifteen': 15, 'sixteen': 16,
       'seventeen': 17, 'eighteen': 18,'nineteen': 19, 'twenty': 20, 'thirty': 30, 'forty': 40, 'fifty': 50,
       'sixty': 60, 'seventy': 70, 'eighty': 80, 'ninety': 90, 'hundred': 100, 'thousand': 1000, 'million': 1000000}

words = list(map(str, input().split()))
sgn = 1
if words[0] == 'negative':
    sgn = -1
    words.pop(0)
temp = 0
ans = 0
for word in words:
    t = dic[word]
    if t == 1000000:
        temp *= 1000000
        ans += temp
        temp = 0
    elif t == 1000:
        temp *= 1000
        ans += temp
        temp = 0
    elif t == 100:
        temp *= 100
    else:
        temp += t
ans += temp
print(sgn * ans)
```

![image-20241108141833378](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20241108141833378.png)



### T16528: 充实的寒假生活

greedy/dp, cs10117 Final Exam, http://cs101.openjudge.cn/practice/16528/

思路：



代码：

```python
n = int(input())
intervals = []
for _ in range(n):
    intervals.append(list(map(int, input().split())))
intervals = sorted(intervals, key=lambda x: x[1])
ed = intervals[0][1]
num = 1
for i in range(1, n):
    if intervals[i][0] > ed:
        num += 1
        ed = intervals[i][1]
print(num)
```

![image-20241108141901853](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20241108141901853.png)



## 2. 学习总结和收获

考试的时候老是在纠结代码是否会超时，写代码写的畏手畏脚，耽误了不少时间。不管怎样先把朴素代码交上去，真超时了再考虑优化的事，可以很大的提高效率。（oj的数据感觉基本都比codeforces要弱很多）





