# Assign #3: Oct Mock Exam暨选做题目满百

Updated 1537 GMT+8 Oct 10, 2024

2024 fall, Complied by Hongfei Yan==（请改为同学的姓名、院系）==



**说明：**

1）Oct⽉考： AC6==（请改为同学的通过数）== 。考试题⽬都在“题库（包括计概、数算题目）”⾥⾯，按照数字题号能找到，可以重新提交。作业中提交⾃⼰最满意版本的代码和截图。

2）请把每个题目解题思路（可选），源码Python, 或者C++/C（已经在Codeforces/Openjudge上AC），截图（包含Accepted, 学号），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、作业评论有md或者doc。

4）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### E28674:《黑神话：悟空》之加密

http://cs101.openjudge.cn/practice/28674/



思路：



代码

```python
low = 'abcdefghijklmnopqrstuvwxyz'
up = low.upper()
k = int(input())
s = input()
final = ''
for i in range(len(s)):
    if s[i] in low:
        temp = low.index(s[i])
        temp -= k % 26
        if temp < 0:
            temp += 26
        final += low[temp]
    else:
        temp = up.index(s[i])
        temp -= k % 26
        if temp < 0:
            temp += 26
        final += up[temp]
print(final)

```

![image-20241015140702937](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20241015140702937.png)



### E28691: 字符串中的整数求和

http://cs101.openjudge.cn/practice/28691/



思路：



代码

```python
a, b = map(str, input().split())
a = int(a[:2])
b = int(b[:2])
print(a+b)

```

![image-20241015141113893](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20241015141113893.png)



### M28664: 验证身份证号

http://cs101.openjudge.cn/practice/28664/



思路：



代码

```python
co = [7, 9, 10, 5, 8, 4, 2, 1, 6, 3, 7, 9, 10, 5, 8, 4, 2]
maps = [1, 0, 'X', 9, 8, 7, 6, 5, 4, 3, 2]
n = int(input())
for i in range(n):
    s = input()
    ex = 0
    for i in range(17):
        ex += int(s[i]) * co[i]
    ex = ex % 11
    if str(maps[ex]) == s[-1]:
        print("YES")
    else:
        print('NO')

```

![image-20241015141535236](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20241015141535236.png)



### M28678: 角谷猜想

http://cs101.openjudge.cn/practice/28678/



思路：



代码

```python
n = int(input())
while n != 1:
    if n % 2 == 0:
        print(str(n) + '/2=' + str(n//2))
        n = n // 2
    else:
        print(str(n) + '*3+1=' + str(n*3+1))
        n = n * 3 + 1
print('End')

```

![image-20241015141617933](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20241015141617933.png)



### M28700: 罗马数字与整数的转换

http://cs101.openjudge.cn/practice/28700/



思路：



##### 代码

```python
trans = {'I': 1, 'IV': 4, 'V': 5, 'IX': 9, 'X': 10, 'XL': 40, 'L': 50,
         'XC': 90, 'C': 100, 'CD':400, 'D': 500, 'CM': 900,  'M': 1000}


def r_to_a(s):
    a = 0
    i = 0
    while i < len(s):
        if s[i:i+2] == 'IV':
            a += 4
            i += 2
            continue
        if s[i:i+2] == 'IX':
            a += 9
            i += 2
            continue
        if s[i:i+2] == 'XL':
            a += 40
            i += 2
            continue
        if s[i:i+2] == 'XC':
            a += 90
            i += 2
            continue
        if s[i:i+2] == 'CD':
            a += 400
            i += 2
            continue
        if s[i:i+2] == 'CM':
            a += 900
            i += 2
            continue
        a += trans[s[i]]
        i += 1
    return a


def a_to_r(a):
    s = ''
    nums = list(trans.values())
    alphas = list(trans.keys())
    while a > 0:
        for i in range(12, -1, -1):
            while a >= nums[i]:
                a -= nums[i]
                s += alphas[i]
    return s


r = input()
if r[0] in trans.keys():
    print(r_to_a(r))
else:
    print(a_to_r(int(r)))

```

![image-20241015141944675](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20241015141944675.png)



### *T25353: 排队 （选做）

http://cs101.openjudge.cn/practice/25353/



思路：



代码

```python
n, d = map(int, input().split())
h = []
for i in range(n):
    h.append(int(input()))
ans = []
while len(h) > 0:
    max1 = 0
    min1 = 10 ** 10
    temp = []
    copy = []
    i = 0
    for i in range(len(h)):
        if h[i] > max1:
            max1 = h[i]
        if h[i] < min1:
            min1 = h[i]
        if abs(h[i]-max1) <= d and abs(h[i]-min1) <= d:
            temp.append(h[i])
        else:
            copy.append(h[i])
    temp.sort()
    h = copy[::]
    for i in range(len(temp)):
        print(temp[i])

```

![image-20241015142145961](C:\Users\Mark Lee\AppData\Roaming\Typora\typora-user-images\image-20241015142145961.png)



## 2. 学习总结和收获

每日选做每日跟进，问题不大。

月考罗马数字选了不太熟悉的字典来做，浪费了一些时间。排队写出来后一看就会超时（O(n^2)），但考试时没有优化出来。下来思考了一会才给出了初步的优化。进一步优化需要线段树等没有学过的东西，就暂且搁置。









