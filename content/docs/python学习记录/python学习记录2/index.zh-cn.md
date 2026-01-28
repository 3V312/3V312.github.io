---
title: python学习记录2
weight: 13
draft: false
description: "一些补充"
slug: "python2"
tags: ["python", "学习记录", "文档"]
series: ["python"]
series_order: 2
---
# 260127

python的输入输出比较随意也比较复杂

# 输入

第一是输入单行中的单个值

`d = int(input().strip())`

其中input返回字符串，strip切割字符串前后的无用空值，int进行类型转换

第二是输入单行中的多个值，见记录1

`max_a, max_b = map(int, input().split())`

map返回的是一个迭代器，=发生了解包。

如果对一行中的多个值要分开操作。就是先读取再操作。

如果一行多个数字

`nums = list(map(int, input().split()))`

第三是输入多行

就是加上一个循环。

- 多行，每行一个数

```
# 已知有n行数据
n = int(input())  # 第一行：告诉你有多少数据

# 方法1：用列表推导式
numbers = [int(input()) for _ in range(n

# 方法2：普通循环
numbers = []
for i in range(n):
    num = int(input())
    numbers.append(num)
```

- 多行每行多个数

```
# 情况1：每行有固定数量的数（比如2个）
n = int(input())  # n行数据

data = []
for i in range(n):
    # 每行输入："1 2" → 转为 [1, 2]
    row = list(map(int, input().split()))
    data.append(row)
```

- 每行数量不固定

```
n = int(input())  # n行

data = []
for i in range(n):
    # 每行可能有多个数，如："1 2 3 4"
    row = list(map(int, input().split()))
    data.append(row)
```

- 多少行不固定

```
# 方法1：读取到空行结束
lines = []
while True:
    line = input().strip()  # 读一行
    if line == "":  # 空行停止
        break
    lines.append(line)

# 方法2：读取EOF（Ctrl+D或文件结束）
import sys
for line in sys.stdin:
    line = line.strip()
    if not line:  # 可选的停止条件
        break
    # 处理line
```

# 输出

1. printf

有两种方式，一种是print(f"{a} + {b} = {a + b}")

另一种是print("{} + {} = {}".format(a, b, a + b))

进行详细配置的方式，是在花括号内

```
num = 123.456

# 数字格式
print(f"整数: {num:.0f}")    # 123
print(f"两位小数: {num:.2f}") # 123.46
print(f"宽度10: {num:10.2f}") # '    123.46'

# 对齐
print(f"左对齐: {text:<10}")  # 'hello     '
print(f"右对齐: {text:>10}")  # '     hello'
print(f"居中对齐: {text:^10}") # '  hello   '
```

特别对于format

```
print('The story of {0}, {1}, and {other}.'.format('Bill', 'Manfred',
                                                   other='Georg'))
      同时使用位置参数和关键字参数  
```

format的高级用法

```
第一
table = {'Sjoerd': 4127, 'Jack': 4098, 'Dcab': 8637678}
print('Jack: {0[Jack]:d}; Sjoerd: {0[Sjoerd]:d}; Dcab: {0[Dcab]:d}'.format(table))
# 理解 {0[Jack]:d} 的每个部分：
#   0      → format()的第0个参数（从0开始数）
#   [Jack] → 访问第0个参数的'Jack'键（必须是字典或支持[]操作）
#   :d     → 格式化为十进制整数

# 实际执行步骤：
# 1. format(table) → 把table作为第一个参数
# 2. {0[Jack]} → 相当于 table['Jack'] → 4098
# 3. {0[Sjoerd]} → table['Sjoerd'] → 4127
# 4. {0[Dcab]} → table['Dcab'] → 8637678

第二
table = {'Sjoerd': 4127, 'Jack': 4098, 'Dcab': 8637678}
print('Jack: {Jack:d}; Sjoerd: {Sjoerd:d}; Dcab: {Dcab:d}'.format(**table))

# **table 是"解包"操作：
# 把字典 {'a': 1, 'b': 2} 变成 a=1, b=2

# 所以：
print('Jack: {Jack:d}'.format(**table))
# 等价于：
print('Jack: {Jack:d}'.format(Jack=4127, Sjoerd=4098, Dcab=8637678))
第三
table = {k: str(v) for k, v in vars().items()}
message = " ".join([f'{k}: ' + '{' + k +'};' for k in table.keys()])
print(message.format(**table))
# vars() 返回当前作用域的变量字典
x = 10
y = "hello"

# vars() 会返回类似：
# {'x': 10, 'y': 'hello', '__name__': '__main__', ...}

# 第一步：{k: str(v) for k, v in vars().items()}
# → 生成器表达式，把每个值转为字符串

# 第二步：[f'{k}: ' + '{' + k +'};' for k in table.keys()]
# → 为每个变量名生成模板字符串
# 比如 'x: ' + '{' + 'x' + '};' → 'x: {x};'

# 第三步：join拼接 → 'x: {x}; y: {y}; __name__: {__name__}; ...'

# 第四步：format(**table) 用实际值填充
```

2.sys

```
import sys

# 标准流
sys.stdin          # 标准输入（键盘）
sys.stdout         # 标准输出（屏幕）
sys.stderr         # 标准错误（屏幕）

# 使用示例
sys.stdout.write("Hello\n")
sys.stderr.write("Error\n")
data = sys.stdin.read()  # 读取所有输入

```

sys.stdout.write就是print的原理，通常它用于以下场景

`sys.stdout.write()` 使用场景

1. **无换行连续输出** - 需要在一行内多次输出
2. **动态更新显示** - 进度条、倒计时等需要覆盖上一行的场景
3. **二进制数据输出** - 输出原始字节而非文本
4. **精确格式控制** - 避免print的自动空格和换行
5. **性能敏感场景** - 大量输出时略快于print
6. **低层I/O操作** - 需要直接操作文件描述符时
7. **自定义输出缓冲** - 需要手动控制flush时机
8. **模拟终端效果** - 打字机效果、动画等
9. **保持光标位置** - 使用`\\r`、`\\b`等控制字符时
10. **兼容性要求** - 需要在不同Python版本间保持行为一致

`sys.stderr` 使用场景

1. **错误信息报告** - 程序错误、异常信息
2. **警告信息输出** - 非致命但需要注意的情况
3. **调试信息打印** - 开发时的临时调试输出
4. **进度状态显示** - 长时间操作的状态更新
5. **日志信息记录** - 操作日志、访问日志等
6. **用户交互提示** - 需要用户注意的提示信息
7. **输入验证反馈** - 格式错误、范围越界等提示
8. **资源使用报告** - 内存、时间等统计信息
9. **权限相关通知** - 权限不足、访问被拒等信息
10. **配置信息输出** - 配置加载、环境检测结果

两者区分使用的核心理由

1. **输出分离** - stdout用于结果，stderr用于过程信息
2. **重定向控制** - 用户可分别重定向结果和日志
3. **错误隔离** - 错误信息不影响正常输出管道
4. **调试便利** - 可关闭stderr只看纯净结果
5. **日志管理** - 错误日志与业务数据分开存储
6. **管道处理** - stdout可被其他程序继续处理
7. **权限区分** - 不同级别信息输出到不同位置
8. **性能考虑** - stderr通常无缓冲或行缓冲
9. **标准化** - 遵循Unix/Linux标准I/O设计
10. **兼容性** - 与其他命令行工具行为一致

# 其他

isinstance(a, int)，检查a是不是int