# Assignment #2: 语法练习

Updated 0126 GMT+8 Sep 24, 2024

2024 fall, Complied by ==同学的姓名、院系==



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）课程网站是Canvas平台, https://pku.instructure.com, 学校通知9月19日导入选课名单后启用。**作业写好后，保留在自己手中，待9月20日提交。**

提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### 263A. Beautiful Matrix

https://codeforces.com/problemset/problem/263/A



思路：



##### 代码

```python
row = []
f1 = 0
f2 = 0
for i in range(5):
    num = input().split()
    for j in range(5):
        row.append(int(num[j]))
        if int(num[j]) == 1:
            f1 = i
            f2 = j
final = abs(2-f1) + abs(2-f2)
print(final)
```

![image-20240926154603912](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20240926154603912.png)



### 1328A. Divisibility Problem

https://codeforces.com/problemset/problem/1328/A



思路：



##### 代码

```python
t = int(input())
a = []
b = []
for i in range(0, t):
    c = input().split()
    a.append(int(c[0]))
    b.append(int(c[1]))
final = 0
for i in range(0, t):
    if a[i] % b[i] == 0:
        c = a[i] // b[i]
    else:
        c = a[i] // b[i] +1
    final = b[i] * c - a[i]
    print(final)
```

![image-20240926154649295](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20240926154649295.png)



### 427A. Police Recruits

https://codeforces.com/problemset/problem/427/A



思路：



##### 代码

```python
n = input()
events = [int(x) for x in input().split()]
num = 0
final = 0
for event in events:
    if event > 0:
        num += event
    else:
        if num != 0:
            num -= 1
        else:
            final += 1
print(final)
```

![image-20240926154740109](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20240926154740109.png)



### 02808: 校门外的树

http://cs101.openjudge.cn/practice/02808/



思路：



##### 代码

```python
l, m = map(int, input().split())
areas = []
for i in range(m):
    areas.append(list(map(int, input().split())))
areas.sort(key=lambda x: x[0])
i = 0
while i < len(areas) - 1:
    if areas[i + 1][1] >= areas[i][1] >= areas[i + 1][0]:
        areas[i][1] = areas[i + 1][1]
        del areas[i + 1]
        continue
    elif areas[i][1] >= areas[i + 1][1]:
        del areas[i + 1]
        continue
    i += 1
final = 0
for k in range(len(areas)):
    final += areas[k][1] - areas[k][0] + 1
print(l + 1 - final)
```

![image-20240926155147659](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20240926155147659.png)



### sy60: 水仙花数II

https://sunnywhy.com/sfbj/3/1/60



思路：



##### 代码

```python
def judge(n):
    c = n % 10
    b = n//10 % 10
    a = n//100
    if n == a**3+b**3+c**3:
        return True
    else:
        return False


a, b = map(int, input().split())
final = ''
for i in range(a, b+1):
    if judge(i):
        final += str(i) + ' '
if final == '':
    final = 'NO'
print(final.rstrip())
```

![image-20240926160134345](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20240926160134345.png)



### 01922: Ride to School

http://cs101.openjudge.cn/practice/01922/



思路：这其实是一道数学题，我们可以证明Charley一定是和最早到达学校且是t=0后出发的学生一起的。首先显然Charley第一个跟上的学生是t>=0时出发的。假设Charley一直跟着t>=0时出发的学生，若他和一个t<0时出发的学生相遇，由于行驶路程一致，t<0时出发的学生速度显然更慢，故Charley还是会跟着t>=0时出发的学生，而其他速度更快的t<0时出发的学生无法与Charley相遇。下一步说明Charley跟着的学生一定是t>=0出发的学生中最早到的。假设Charley并非跟着这个最早到的学生，而是跟着学生P。则最早到的学生要么比学生P出发的早，要么比P出发的晚且路途上必定相遇。若出发的早，则Charley无论如何应先遇到最早到学生而不是学生P；若出发的晚，则学生P与最早到学生相遇时Charley应转而跟着最早到学生，均矛盾。故我们证明了Charley一定是跟着最早到的学生到的学校。



##### 代码

```python
import math

while True:
    n = int(input())
    if n == 0:
        break
    times = []
    for i in range(n):
        v, t = map(int, input().split())
        if t >= 0:
            temp = 16200 / v + t
            times.append(temp)
    print(math.ceil(min(times)))
```

![image-20240926190814472](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20240926190814472.png)



## 2. 学习总结和收获

做完了所有每日选做。这次作业中Ride to school给了我明显的收获。一开始由于模拟思路直接，且代码也并不困难，没有仔细考虑就将代码写了出来，喜提超时。卡了半个多小时，看到同学代码时间很短，发现一定存在数学方法，拿纸笔分析了一下就得到了结果。总而言之能用手解的不用代码。
