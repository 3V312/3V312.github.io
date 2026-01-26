---
title: python学习记录1
weight: 13
draft: false
description: "刚好今天白天在notion写的这个,用作第一条博客吧"
slug: "advanced-customisation"
tags: ["python", "学习记录", "文档"]
series: ["python"]
series_order: 1
---
# 260124

今天写PAT发现，好简单啊，本来想写到难的就停止的，但是好简单。跟OJ的中等难度一样啊，最多题目形式多一些。

于是我决定用python写，我需要去看看python的库

def read_data():
"""读取输入数据"""
n = int(input())
nums = list(map(int, input().split()))
return n, nums

def process(nums):
"""处理数据"""
return sum(nums)

def output(result):
"""输出结果"""
print(result)

def main():
n, nums = read_data()
result = process(nums)
output(result)

if **name** == "**main**":
main()

# 输入输出

对于输入：

3 6

错误写法：

```
max_a=int (input())
max_b=int (input())

```

正确写法：
`max_a, max_b = map(int, input().split())`

*

其中`input().split()`是一个函数，返回了多个元素（没有char类型，那么就是长度为1的字符串意思）。int 是它们俩的数据类型（这里应该看作是一个函数），map也是一个函数，

`input().split()`，split的原理似乎是把这一行全输入作为字符串，然后分割，然后转类型。

*

首先给出map的定义

- **map**(*function*, *iterable*,/, **iterables*, *strict=False*)
    
    **map**(*function*, *iterable*,/, **iterables*, *strict=False*) 返回将 *function* 应用于每个 *iterable* 项目的迭代器。若传递多个 *iterables*,*function* 必须接受相同数量的参数并并行应用。迭代器在最短的 iterable 用尽后停止。若 *strict* 为 True 且某个 iterable 提前耗尽,则引发 [ValueError](https://docs.python.org/zh-cn/3/library/exceptions.html#ValueError)。
    

简单来说，就是把function依次对iterable作用。

- *iterable意思是可以接受**任意数量的可迭代对象**。

```
list1 = [1, 2, 3]
list2 = [4, 5, 6]
result = map(add, list1, list2)  # [5, 7, 9]
```

- *strict=False：*

默认 strict=False：长度不同时，按最短的截断

```jsx
list1 = [1, 2, 3, 4]
list2 = [5, 6, 7]
result = map(lambda x, y: x + y, list1, list2, strict=False)、
只计算前3对：[6, 8, 10]，忽略多余的
```

strict=True：长度不同时报错

```jsx
result = map(lambda x, y: x + y, list1, list2, strict=True)
ValueError: zip() argument 2 is shorter than argument 1
```

- / 表示它前面的参数必须是位置参数，不能使用关键字参数形式调用。

位置参数：第一个给name，第二个给age

say_hello("Alice", 25)  # Alice is 25 years old

                  ↓           ↓

             name        age

关键字参数：明确指定哪个值给哪个参数

say_hello(age=25, name="Alice")  # 顺序无所谓

- input().split()是一个链式调用，就是前者调用后的结果直接传给后者，并非是成员关系

**分解步骤：**

1. `input()` 执行 → 返回一个**字符串**
2. 对这个字符串调用 `.split()` 方法
3. 返回分割后的列表

最后，`max_a, max_b = map(int, input().split())`的意思就是，先把一行输入为字符串，传递给split分割成多个元素（长度为1的字符串），用int函数依次作用，按顺序传给max_a,max_b;

# 循环输入

对于：

8 10 9 12
5 10 5 10
3 8 5 12

我们想要循环3次，每次输入一行，然后进行判断；

此时应该：

```jsx
for _ in range(n): 
    a_say, a_do, b_say, b_do = map(int, input().split())
```

其中_表示一个不关心的值，range是一个函数，n是循环次数

现在给出for的介绍：

Python 的 [`for`](https://docs.python.org/zh-cn/3/reference/compound_stmts.html#for) 语句与 C 或 Pascal 中的不同。Python 的 `for` 语句不迭代算术递增数值（如 Pascal），或是给予用户定义迭代步骤和结束条件的能力（如 C），而是在列表或字符串等任意序列的元素上迭代，按它们在序列中出现的顺序。 例如（这不是有意要暗指什么）：

```jsx

# 度量一些字符串：
words = ['cat', 'window', 'defenestrate']
for w in words:
    print(w, len(w))

cat 3
window 6
defenestrate 12
```

简单来说，新建变量w,指向words当前元素，每次循环中自动走向下一个元素，当words的元素没有了，迭代自然结束。

需要注意的是，操作中对words的删除或增添实时的改变原本的列表，所以会影响w的移动，使某些元素被错误的掠过。所以：

```jsx
难正确地在迭代多项集的同时修改多项集的内容。更简单的方法是迭代多项集的副本或者创建新的多项集：

Copy
# 创建示例多项集
users = {'Hans': 'active', 'Éléonore': 'inactive', '景太郎': 'active'}

# 策略：迭代一个副本
for user, status in users.copy().items():
    if status == 'inactive':
        del users[user]

# 策略：创建一个新多项集
active_users = {}
for user, status in users.items():
    if status == 'active':
        active_users[user] = status
```

在我们的循环里，并没有对已有的列表操作，而是起到一个计数器的作用

- _是我们新建的变量，它表示我们不关心它。
- range(n)是一个近似于列表的东西，它可以生成等差数列：
- 关于range的解释
    
    内置函数 [`range()`](https://docs.python.org/zh-cn/3/library/stdtypes.html#range) 用于生成等差数列：
    
    `for i in range(5):
        print(i)
    
    0
    1
    2
    3
    4`
    
    生成的序列绝不会包括给定的终止值；`range(10)` 生成 10 个值——长度为 10 的序列的所有合法索引。range 可以不从 0 开始，且可以按给定的步长递增（即使是负数步长）：
    
    `list(range(5, 10))
    [5, 6, 7, 8, 9]
    
    list(range(0, 10, 3))
    [0, 3, 6, 9]
    
    list(range(-10, -100, -30))
    [-10, -40, -70]`
    
    要按索引迭代序列，可以组合使用 [`range()`](https://docs.python.org/zh-cn/3/library/stdtypes.html#range) 和 [`len()`](https://docs.python.org/zh-cn/3/library/functions.html#len)：
    
    Copy
    
    `a = ['Mary', 'had', 'a', 'little', 'lamb']
    for i in range(len(a)):
        print(i, a[i])
    
    0 Mary
    1 had
    2 a
    3 little
    4 lamb`
    

另外：

```jsx
range() 返回的对象在很多方面和列表的行为一样，但其实它和列表不一样。
该对象只有在被迭代时才一个一个地返回所期望的列表项，并没有真正生成过一个含有全部项的列表，
从而节省了空间。
这种对象称为可迭代对象 iterable，适合作为需要获取一系列值的函数或程序构件的参数。

```

# 判断

```jsx
				a_lose = (a_do == total)
        b_lose = (b_do == total)
        

        if a_lose and not b_lose:
            count_a += 1
        elif b_lose and not a_lose:
            count_b += 1
```

a_lose自动成为一个布尔类型。

另外python没有++，要用+=1；