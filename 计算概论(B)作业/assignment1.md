# Assignment #1: 自主学习

Updated 0110 GMT+8 Sep 10, 2024

2024 fall, Complied by ==同学的姓名、院系==



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）课程网站是Canvas平台, https://pku.instructure.com, 学校通知9月19日导入选课名单后启用。**作业写好后，保留在自己手中，待9月20日提交。**

提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### 02733: 判断闰年

http://cs101.openjudge.cn/practice/02733/



思路：



##### 代码

```python
year = int(input())
if year % 4 == 0:
    if year % 100 == 0 and year % 400 != 0:
        print('N')
    else:
        print('Y')
else:
    print('N')
```

![image-20240919161558061](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20240919161558061.png)



### 02750: 鸡兔同笼

http://cs101.openjudge.cn/practice/02750/



思路：



##### 代码

```python
leg = int(input())
if leg % 2 != 0:
    print(0, 0)
else:
    big = leg // 2
    if leg % 4 == 0:
        small = leg // 4
    else:
        small = leg // 4 + 1
    print(small, big)
```

![image-20240919161727528](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20240919161727528.png)



### 50A. Domino piling

greedy, math, 800, http://codeforces.com/problemset/problem/50/A



思路：



##### 代码

```python
M, N = map(int, input().split())
a = M * N // 2
print(a)
```

![image-20240919161830561](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20240919161830561.png)



### 1A. Theatre Square

math, 1000, https://codeforces.com/problemset/problem/1/A



思路：



##### 代码

```python
n, m, a = map(int, input().split())
if n % a == 0:
    row = n // a
else:
    row = n // a + 1
if m % a == 0:
    line = m // a
else:
    line = m // a + 1
final = row * line
print(final)
```

![image-20240919161925716](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20240919161925716.png)



### 112A. Petya and Strings

implementation, strings, 1000, http://codeforces.com/problemset/problem/112/A



思路：



##### 代码

```python
a = input().lower()
b = input().lower()
length = min(len(a), len(b))
for i in range(0, length):
    if a[i] < b[i]:
        print(-1)
        exit()
    elif a[i] > b[i]:
        print(1)
        exit()
if len(a) == len(b):
    print(0)
elif len(a) > len(b):
    print(1)
else:
    print(-1)
```

![image-20240919162056063](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20240919162056063.png)



### 231A. Team

bruteforce, greedy, 800, http://codeforces.com/problemset/problem/231/A



思路：



##### 代码

```python
n = int(input())
bi = []
score = []
final = 0
for i in range(0, n):
    score = [int(m) for m in input().split()]
    bi.append(score)
for i in range(0, n):
    if sum(bi[i]) >= 2:
        final += 1
print(final)
```

![image-20240919162141763](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20240919162141763.png)



## 2. 学习总结和收获

所有每日选做，除此之外没有额外练习。主要是在熟悉语法，以及纠正细节错误，比如列表指标。





