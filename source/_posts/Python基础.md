---
title: Python基础
date: 2025-09-03 23:29:02
tags:
---
### 输入和输出

* `print()` 函数可以向屏幕上输出指定的文字。

  ```python
  >>> print('hello, world')
  ```

* `print()` 函数也可以接受多个字符串，用逗号 `,` 隔开，就可以连成一串输出。

  ```python
  >>> print('The quick brown fox', 'jumps over', 'the lazy dog')
  The quick brown fox jumps over the lazy dog
  ```

  ```python
  >>> print('100 + 200 =', 100 + 200)
  100 + 200 = 300
  ```

* `input()` 函数可以让用户输入字符串，并返回到一个变量中。

  ```python
  >>> name = input()
  Jio
  >>> name
  'Jio'
  ```

  ```python
  # input 函数可以显示一个字符串用作提示
  name = input('Please enter your name: ')
  print('Hello,' name)
  ```



### 数据类型和变量

#### ① 整数

* Python 可以处理任意大小的整数，例如：`1` , `100` , `-8080` ,`0` 等。



#### ② 浮点数

* 浮点数也就是小数，例如：`1.23` , `-9.01`, `1.23e9` , `1.2e-5` 。



#### ③ 字符串

* 字符串是以单引号 `'` 或双引号 `"` 括起来的任意文本，例如：`'abc'`  `"efg"`  `'I\'m \"OK\"!'` 。

* 为了简化，Python 还允许用 `r''` 表示 `''` 内部的字符串默认不转义。

  ```python
  >>> print('\\\t\\')
  \	\
  >>>print(r'\\\t\\')
  \\\t\\
  ```

* 如果字符串内部有很多换行，Python 提供了`'''...'''` 的格式表示很多行内容。

  ```python
  >>> print('''line1
  		 line2
           line3''')
  line1
  line2
  line3
  ```



#### ④ 布尔值

* 布尔值只有 `True` 和 `False` 两种值。

* 布尔值可以用 `and` `or` `not` 运算。

  ```python
  >>> True and True
  True
  >>> True or False
  True
  >>> not True
  False
  ```



#### ⑤ 空值

* 空值是 Python 里一个特殊的值，用 `None` 表示。`None` 不能理解为 `0` ，因为 `0` 是有意义的，而 `None` 是一个特殊的空值。



#### ⑥ 变量

* 在计算机程序中，变量不仅可以是数字，还可以是任意数据类型。

* 变量在程序中就是用变量名表示了，变量名必须是大小写英文、数字和下划线的组合，且不能用数字开头。

  ```python
  a = 123 # a是整数
  print(a)
  a = 'ABC' # a是字符串
  print(a)
  ```



#### ⑦ 常量

* 所谓常量就是不能变的变量。

  ```python
  PI = 3.14159265359
  ```

* 但事实上 `PI` 仍然是一个变量，但是全部大写的变量名表示常量是一个习惯上的用法。



#### ⑧ 其他

* 在 Python 中有两种除法，一种除法是 `/`，这种除法结果时浮点数，即使两个整数恰好整除，结果也仍然是浮点数。

  ```python
  >>> 10 / 3
  3.3333333333333335
  ```

* 另外一种除法是 `//`，称为地板除，两个整数的除法仍然是整数，即使除不尽，也仍然是整数。

  ```python
  >>> 10 // 3
  3
  ```



### 编码

字符串也是一种数据类型，但是字符串比较特殊的是会有编码问题。因为计算机只能处理数字，如果要处理文本，就必须先把文本转换为数字才能处理。最早的计算机在设计时采用 `8` 个比特(`bit`)作为一个字节(`byte`)，所以，一个字节能表示的最大的整数就是 `255`，如果要表示更大的整数，就必须用更多的字节。比如两个字节可以表示最大整数 `65535`，`4` 个字节表示的最大整数是`4294967295`。

由于计算机是美国人发明的，所以最早只有`127`个字符被编码到计算机里，也就是大小写字母、数字和一些符号，这个编码表被称为 `ASCII` 编码。但是要处理中文一个字节显然是不够的，至少需要两个字节，并且不能与 `ASCII` 编码冲突，所以中国制定了 `GB2312` 编码。

但是由于全世界有上百种语言，日本把日文编到 `Shift_JIS` 里，韩国把韩文编到 `Euc-kr` 里，各国都有各国的标准。因此，`Unicode` 应运而生，`Unicode` 把所有语言都统一到一套编码里，这样就不会有乱码问题了。

 `Unicode` 标准也在不断发展，但最常用的是用两个字节表示一个字符，<u>如果要用到非常偏僻的字符，就需要用 `4` 个字节</u>，现代操作系统和大多数编程语言都直接支持 `Unicode`。

字母 `A` 用 `ASCII` 编码是十进制 `65`，二进制 `01000001`。

字符 `0` 用 `ASCII` 编码是十进制 `48`，二进制 `00110000`，注意字符 `'0'` 和整数 `0` 是不同的。

汉字 `中` 已经超出了 `ASCII` 编码的范围，用 `Unicode` 编码是十进制的 `20013`，二进制的 `00000000 01000001`。

所以 `ASCII` 编码的 `A` 用 `Unicode` 编码，只需要在前面补 `0` 就可以，因此，`A` 的 `Unicode` 编码是 `00000000 01000001`。

可是这样新的问题又出现了，如果统一成 `Unicode` 编码，乱码问题解决了。但是，如果写的文本基本都是英文的话，用 `Unicode` 编码比 `ASCII` 编码需要多一倍的存储空间，在存储和传输上就十分不划算。所以，本着解决的精神，又出现了把 `Unicode` 编码转化为“可变长编码”的 `UTF-8` 编码。`UTF-8` 编码把一个 `Unicode` 字符根据不同的数字大小编码成1-6个字节，常用的英文字母被编码成1个字节，汉字通常是3个字节，只有很生僻的字符才会被编码成4-6个字节。如果你要传输的文本包含大量英文字符，用 `UTF-8` 编码就能节省空间。

| 字符  |  ASCII   |      Unicode      |           UTF-8            |
| :---: | :------: | :---------------: | :------------------------: |
|   A   | 01000001 | 00000000 01000001 |          01000001          |
|  中   |    -     | 01001110 00101101 | 11100100 10111000 10101101 |

在计算机内存中，统一使用 `Unicode` 编码，当需要保存到硬盘或者需要传输的时候，就转换为 `UTF-8` 编码。

用记事本编辑的时候，从文件读取的 `UTF-8` 字符被转换为 `Unicode` 字符到内存里，编辑完成后，保存的时候再把 `Unicode` 转换为 `UTF-8` 保存到文件。



​                                                          ![rw-file-utf-8](https://gitee.com/jiochen/typora/raw/master/img/file.png)

浏览网页的试试，服务器会把动态生成的 `Unicode` 内容转换为 `UTF-8` 再传输到浏览器，所以你看到很多网页的源码上会有类似 `<meta charset="UTF-8" />` 的信息，表示该网页正是用的 `UTF-8` 编码。

![web-utf-8](https://gitee.com/jiochen/typora/raw/master/img/webpage.png)



### Python 的字符串

在 Python3 版本中，字符串是以 `Unicode` 编码的，就是说，Python 的字符串支持多语言。

```python
>>> print('包含中文的str')
包含中文的str
```

对于单个字符的编码，Python 提供了 `ord()` 函数获取字符的整数表示，`chr()` 函数把编码转换为对应的字符。

```python
>>> ord('中')
20013
>>> ord('A')
65
>>> chr(66)
'B'
>>> chr(25991)
'文'
```

如果知道字符的整数编码，还可以用十六进制这么写 `str`：

```python
>>> '\u4e2d\u6587'
'中文'
```

这两种写法完全是等价的。

由于 Python 的字符串类型是 `str`，在内存中以 `Unicode` 表示，一个字符对应若干个字节。如果要再网络上传输或者保存在硬盘上，就需要把 `str` 变为以字节为单位的 `bytes`。

Python 中对 `bytes` 类型的数据用带 `b` 前缀的单引号或双引号表示：

```python
x = b'ABC'
```

注意区分 `'ABC'` 和 `b'ABC'`，前者是 `str`，后者虽然内容显示和前者一样，但 `bytes` 的每个字符都只占用一个字节。

以 `Unicode` 表示的 `str` 通过 `encode()` 方法可以编码为指定的 `bytes`，例如：

```python
>>> 'ABC'.encode('ASCII')
b'ABC'
>>> '中文'.encode('UTF-8')
b'\xe4\xb8\xad\xe6\x96\x87'
>>> '中文'.encode('ASCII')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
UnicodeEncodeError: 'ascii' codec can't encode characters in position 0-1: ordinal not in range(128)
```

纯英文的 `str` 可以用 `ASCII` 编码为 `bytes`，含有中文的 `str` 可以用 `UTF-8` 编码为 `bytes`，含有中文的 `str` 无法用 `ASCII` 编码，因为中文编码范围超过了 `ASCII` 编码的范围，所以会报错。在 `bytes` 中，无法显示为 `ASCII` 字符的字节，用 `\x##` 显示。

反过来，如果我们从网络或硬盘上读取了字节流，那么读到的数据就是 `bytes`。要把 `bytes` 变为 `str`，就需要用 `decode()` 方法：

```python
>>> b'ABC'.decode('ASCII')
'ABC'
>>> b'\xe4\xb8\xad\xe6\x96\x87'.decode('UTF-8')
'中文'
```

如果 `bytes` 中包含无法解码的字节，`decode()` 方法会报错：

```python
>>> b'\xe4\xb8\xad\xff'.decode('utf-8')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
UnicodeDecodeError: 'utf-8' codec can't decode byte 0xff in position 3: invalid start byte
```

如果 `bytes` 中只有一小部分无效的字节，可以传入 `error='ignore'` 忽略错误的字节：

```python
>>> b'\xe4\xb8\xad\xff'.decode('utf-8', errors='ignore')
'中'
```

要计算 `str` 包含多少个字符，可以用 `len()` 函数：

```python
>>> len('ABC')
3
>>> len('中文')
2
```

`len()` 函数计算的是 `str` 的字符数，如果换成 `bytes`，`len()` 函数就计算字节数：

```python
>>> len(b'ABC')
3
>>> len(b'\xe4\xb8\xad\xe6\x96\x87')
6
>>> len('中文'.encode('UTF-8'))
6
>>> len('中文'.encode('GB2312'))
4
```

可见，1个中文字符经过 `UTF-8` 编码后通常会占用3个字节，而通过 `GB2312` 编码后通常占用2个字节，1个英文字符只占用1个字节。在操作字符串时，我们经常遇到 `str` 和 `bytes` 的互相转换，为了避免乱码问题，应当始终坚持使用 `UTF-8` 编码对 `str` 和 `bytes` 进行转换。

由于 Python 源代码也是一个文本文件，所以当你的源代码包含中文的时候，就需要指定保存为 `UTF-8` 编码，当 Python 解释器读取源代码时，为了让它按 `UTF-8` 编码读取，我们通常在文件开头写上这两行：

```python
#!/usr/local/bin python3
# -*- coding: utf-8 -*-
```

第一行注释告诉 Linux/OS X 系统，这是一个 Python 可执行程序，Windows 系统会忽略这个注释。第二行注释告诉 Python 解释器，按照 `UTF-8` 编码读取源代码，否则，你在源代码写的中文输出可能会有乱码。申明了 `UTF-8` 编码并不意味着你的 `.py` 文件就是 `UTF-8` 编码的，必须并且要确保文本编辑器正在使用 `UTF-8 without BOM` 编码。如果 `.py` 文件本身使用 `UTF-8` 编码，并且也申明了 `# -*- coding: utf-8 -*-`，打开命令提示符测试就可以正常显示中文。



### 格式化

还有一个常见的问题就是如何输出格式化的字符串，我们经常会输出类似 `'亲爱的xxx你好！你xx月的话费是xx，余额是xx'` 之类的字符串，而 `xxx` 的内容都是根据变量变化的，所以需要一个简便的格式化字符串的方式。

![](/Users/chenhao/Documents/python/3.png)

在 Python 中，采用的格式化方式和 C 语言是一致的，用`%` 实现，举例：

```python
>>> 'Hello, %s' % 'world'
'Hello, world'
>>> 'Hi, %s, you have $%d.' % ('Michael', 1000000)
'Hi, Michael, you have $1000000.'
```

运算符 `%` 用来格式化字符串，`%s` 表示用字符串替换，`%d` 表示用整数替换，有几个 `%?` 占位符，后面就跟几个变量或者值，顺序对应。如果只有一个 `%?`，括号可以省略。

| 占位符 |   替换内容   |
| :----: | :----------: |
|  `%d`  |     整数     |
|  `%f`  |    浮点数    |
|  `%s`  |    字符串    |
|  `%x`  | 十六进制整数 |

```python
>>> print('%2d-%02d' % (3, 1))
 3-01
>>> print('%.2f' % 3.1412926)
3.14
>>> 'Age: %s. Gender: %s' % (25, True)
'Age: 25. Gender: True'
>>> 'growth rate: %d%%' % 7
'growth rate: 7%'
```

如果不太确定应该用什么，`%s` 永远起作用，它会把任何数据类型转换为字符串。而有些时候，字符串里面的 `%` 是一个普通字符的话，这个时候可以用 `%%` 来进行转义。

另外一种格式化字符串的方式用使用字符串的 `format()` 方法，它会用传入的参数一次替换字符串内的占位符 `{0}`、`{1}`...，不过这种方式写起来比 `%` 要麻烦的多：

```python
>>> 'Hello，{0}，成绩提升了{1:.1f}%'.format('小明', 17.125)
'Hello，小明，成绩提升了17.1%'
```



### List 和 Tuple

#### ① 列表 list

list 是 Python 内置的一种数据类型，一种有序的集合，可以随时添加和删除其中的元素。

```python
>>> classmates = ['Michael', 'Bob', 'Tracy']
>>> classmates
['Michael', 'Bob', 'Tracy']
```

变量 `classmates` 就是一个 `list`，用 `len()` 函数可以获得 `list` 元素的个数：

```python
>>> len(classmates)
3
```

`list` 可以通过用索引来访问 `list` 中的每一个元素，所以是从`0` 开始：

```python
>>> classmates[0]
'Michael'
>>> classmates[1]
'Bob'
>>> classmates[2]
'Tracy'
>>> classmates[3]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IndexError: list index out of range
```

当索引超出了范围时，Python 会报一个 `IndexError` 的错误，所以，要确保索引不要越界，记得最后一个元素的所以是 `len(classmates) - 1`。如果要取得最后一个元素，除了计算索引以外，还可以用 `-1` 做索引，直接获取最后一个元素：

```python
>>> classmates[-1]
'Tracy'
```

以此类推，可以获取倒数第2个、倒数第2个：

```python
>>> classmates[-2]
'Bob'
>>> classmates[-3]
'Michael'
>>> classmates[-4]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IndexError: list index out of range
```

`list` 是一个可变的有序表，所以，可以往 `list` 中追加元素到末尾：

```python
>>> classmates.append('Adam')
>>> classmates
['Michael', 'Bob', 'Tracy', 'Adam']
```

也可以把元素插入到指定的位置，比如索引号为1的位置：

```python
>>> classmates.insert(1, 'Jack')
>>> classmates
['Michael', 'Jack', 'Bob', 'Tracy', 'Adam']
```

如果要删除 `list` 末尾的元素，用 `pop()` 方法：

```python
>>> classmates.pop()
'Adam'
>>> classmates
['Michael', 'Jack', 'Bob', 'Tracy']
```

如果要删除指定位置德元素，用 `pop(i)` 方法，其中 `i` 是索引位置：

```python
>>> classmates.pop(1)
'Jack'
>>> classmates
['Michael', 'Bob', 'Tracy']
```

如果要把某个元素替换成别的元素，可以直接赋值给对应的索引位置：

```python
>>> classmates[1] = 'Sarah'
>>> classmates
['Michael', 'Sarah', 'Tracy']
```

`list` 元素也可以是另一个 `list`，比如：

```python
>>> s = ['python', 'java', ['asp', 'php'], 'scheme']
>>> len(s)
4
```

需要注意的是列表 `s` 只有4个元素，其中 `s[2]` 又是一个 `list`，可以拆开这样写：

```python
>>> p = ['asp', 'php']
>>> s = ['python', 'java', p, 'scheme']
```

如果要拿到 `'php'` 可以写 `p[1]` 或者 `s[2][1]`，因此 `s` 可以看作一个二维数组，类似还有三维、四维数组。

如果一个 `list` 中一个元素也没有，就是一个空的 `list`，它的长度为0：

```python
>>> L = []
>>> len(L)
0
```

#### ② 元组 tuple

还有另一种有序列表叫元组：`tuple`。

`tuple` 和 `list` 非常类似，但是 `tuple` 一旦初始化就不能修改，比如同样是列出同学的名字：

```python
>>> classmates = ('Michael', 'Bob', 'Tracy')
```

`classmates` 是一个 `tuple`，因为它是不能变的，它没有 `append()`，`insert()` 方法，不过获取元素的方法和 `list` 是一样的，可以通过 `classmates[0]` 来获取元素，但不能赋值成另外的元素。

不可变的 `tuple` 可以让代码更安全，所以如果可能，尽量用 `tuple` 代替 `list`。

`tuple` 的陷阱：当你定义一个 `tuple` 时，在定义的时候，`tuple` 的元素就必须被确定下来，比如：

```python
>>> t = (1, 2)
>>> t
(1, 2)
```

如果要定义一个空的 `tuple`，可以写成 `()`：

```python
>>> t = ()
>>> t
()
```

但是如果要定义一个只有1个元素的 `tuple`：

```python
>>> t = (1)
>>> t
1
```

但是从输出上看，定义的是 `1` 这个数，而不是 `tuple`。这是因为括号 `()` 既可以表示 `tuple`，又可以表示数学公式中的小括号，这就产生了歧义，因此，Python 规定，这种情况下，按小括号进行计算，计算结果自然是 `1`。所以定义一个只有1个元素的 `tuple` 必须加一个逗号 `,`，来消除歧义：

```python
>>> t = (1,)
>>> t
(1,)
```

Python 在显示只有1个元素的 `tuple` 时，也会加一个逗号 `,`，以免误解成数学计算意义上的括号。

再来看一个“可变”的 `tuple`：

```python
>>> t = ('a', 'b', ['A', 'B'])
>>> t[2][0] = 'X'
>>> t[2][1] = 'Y'
>>> t
('a', 'b', ['X', 'Y'])
```

![tuple-1](https://gitee.com/jiochen/typora/raw/master/img/step-1.png)

![tuple-2](https://gitee.com/jiochen/typora/raw/master/img/step-2.png)

表面上看，`tuple` 的元素确实变了，但其实变的不是 `tuple` 的元素，而是 `list` 的元素。`tuple` 一开始指向的 `list` 并没有改成别的 `list`。所以，`tuple` 所谓的“不变”是说，`tuple` 的每个元素指向永远不变。即指向 `'a'`，就不能改成指向 `'b'`，指向一个 `list`，就不能改成指向其他对象，但指向的这个 `list` 本身是可变的。



### 条件判断

计算机之所以能够做很多自动化的任务，原因在于它可以自己做条件判断。

比如，输入用户年龄，根据年龄打印不同的内容，在 Python 程序中，用 `if` 语句实现。

```python
age = 20
if age >= 18:
    print('your age is', age)
    print('adult')
```

根据 Python 的缩进规则，如果 `if` 语句判断是 `True`，就把缩进的两行 `print` 语句执行了，否则，什么也不做。

也可以给 `if` 添加一个 `else` 语句，意思是，如果 `if` 判断是 `False`，不要执行 `if` 的内容，去把 `else` 执行了。

```python
age = 3
if age >= 18:
    print('your age is', age)
    print('adult')
else:
    print('your age is', age)
    print('teenager')
```

当然上面的判断是很粗略的，完全可以用 `elif` 做更细致的判断：

```python
age = 3
if age >= 18:
    print('adult')
elif age >= 6:
    print('teenager')
else:
    print('kid')
```

`elif` 是 `else if` 的缩写，完全可以有多个 `elif`，所以 `if` 语句的完整形式就是：

```python
if <条件判断1>:
	<执行1>
elif <条件判断2>:
	<执行2>
elif <条件判断3>:
	<执行3>
else:
	<执行4>
```

`if` 语句执行有个特点，它是从上往下判断，如果在某个判断为 `True`，执行完该判断的语句后，就会忽略剩下的  `elif` 和 `else`。

`if` 判断条件还可以简写，比如写：

```python
if x:
	print('True')
```

只要 `x` 是非零数值、非空字符串、非空 `list` 等，就判断为 `True`，否则为 `False`。

很多人会用 `input()` 方法读取用户的输入：

```python
birth = input('birth: ')

if birth < 2000:
    print('00前')
else:
    print('00后')
```

输入 `1988`，结果报错：

```
Traceback (most recent call last):
  File "/Users/chenhao/workspace/pycharm/python-study/Day02/ifinput.py", line 9, in <module>
    if birth < 2000:
TypeError: '<' not supported between instances of 'str' and 'int'
```

这是因为 `input()` 方法返回的数据类型是 `str`，`str` 不能直接和整数进行比较，必须把 `str` 转换成整数，所以我们会用 `int()` 函数来完成：

```python
s = input('birth: ')
birth = int(s)

if birth < 2000:
    print('00前')
else:
    print('00后')
```



### 循环

要计算 1 + 2 + 3，可以直接写表达式：

```python
>>> 1 + 2 + 3
6
```

要计算 1 + 2 + 3 + ... + 10 也勉强能写出来，但是要计算 1 + 2 + 3 + ... + 10000 那就不现实了，所以我们需要循环语句。



#### ① for 循环

```python
names = ['Michael', 'Bob', 'Tracy']

for name in names:
    print(name)
```

这段代码的执行结果如下：

```python
Michael
Bob
Tracy
```

所以 `for x in ...` 循环就是把每个元素代入变量 `x`，然后执行缩进块的语句。

再比如我们想计算 1-10 的整数之和，可以用一个 `sum` 变量做累加：

```python
sum = 0

for x in [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]:
    sum = sum + x

print(sum)
```

如果要计算 1-100 的整数之和，从 1 写到 100 有点困难，但是 Python 提供一个 `range()` 函数，可以生成一个整数序列，再通过 `list()` 函数可以转换为 `list`，比如 `range(5)` 生成的序列是从 0 开始小于 5 的整数。

```python
>>> list(range(5))
[0, 1, 2, 3, 4]
```

所以 `range(101)` 就可以生成 0-100 的整数序列，计算如下：

```python
sum = 0

for x in range(101):
    sum = sum + x

print(sum)
```



#### ② while 循环

`while` 循环只要条件满足，就不断循环，条件不满足时就退出循环。

```python
sum = 0
n = 99

while n > 0:
    sum = sum + n
    n = n - 2

print(sum)
```



#### ③ break

在循环中，`break` 语句可以提前退出循环。

```python
n = 1

while n <= 100:
    if n > 10:
        break
    print(n)
    n = n + 1

print('END')
```



#### ④ continue

在循环中，也可以通过 `continue` 语句，跳出当前的这次循环，直接开始下一次循环。

```python
n = 0

while n < 10:
    n = n + 1
    if n % 2 == 0:
        continue
    print(n)
```

循环是让计算机做重复任务的有效方法。

`break` 语句可以在循环过程中直接退出循环，而 `continue` 语句可以提前结束本轮循环，并直接开始下一轮循环。

**要特别注意是是**，不要滥用 `break` 和 `continue` 语句。`break` 和 `continue` 会造成代码执行逻辑分叉过多，容易出错。大多数循环并不需要用到 `break` 和 `continue` 语句，上面的两个例子，都可以通过改写循环条件或者修改循环逻辑，去掉 `break` 和 `continue` 语句。



### Dict 和 Set

#### ① 字典 dict

Python 内置了字典 `dict`，`dict` 全称 `dictionary`，在其他语言中也称为 `map`，使用键-值(key-value)存储，具有极快的查找速度。

举个例子，假设要根据同学的名字查找对应的成绩，如果用 `list` 实现，需要两个 `list`：

```python
names = ['Michael', 'Bob', 'Tracy']
scores = [95, 75, 85]
```

给定一个名字去查找对应的成绩，就需要先在 `names` 列表中找到给定名字的位置，再从 `scores` 列表中取得对应成绩，若 `list` 越长，则查找会越耗时。

如果用 `dict` 来实现，只需要一个“名字”-“成绩”的对照表，直接根据名字查找成绩，无论这个表有多大，查找速度都不会变慢。

```python
>>> d = {'Michael': 95, 'Bob': 75, 'Tracy': 85}
>>> d['Michael']
95
```

那为什么 `dict` 查找速度这么快？因为 `dict` 的实现原理和查字典是一样的。假设字典包含了 1 万个汉字，我们要查某一个字，一个办法是把字典从第一页往后翻，直到找到我们想要的字为止，这种方法就是在 `list` 中查找元素的方法，`list` 越大，查找越慢。第二种方法是先在字典的索引表里找到这个字的对应的页码，然后这届翻到该页，找到这个字，无论找的是什么字，这种查找速度都会很快，不会随着字典的大小增加而变慢。

`dict` 就是第二种实现方式，给定一个名字，比如 `'Michael'`，`dict` 在内部就可以直接计算出 `Michael` 对应的存放成绩的“页码”，也就是 `95` 这个数字的存放的内存地址，直接取出来，所以速度很快。

所以，这种 `key-value` 存储方式，在放进去的时候，必须根据 key 算出 value 的存放位置，这样取的时候才能根据 key 直接拿到 value。

将数据存入 `dict` 的方式，除了初始化的方式外，还可以通过 key 放入：

```python
>>> d['Adam'] = 67
>>> d['Adam']
67
```

由于一个 key 只能对应一个 value，所以，多次对一个 key 放入 value，后面的值会把前面的值冲掉：

```python
>>> d['Jack'] = 90
>>> d['Jack']
90
>>> d['Jack'] = 88
>>> d['Jack']
88
```

如果 key 不存在，`dict` 就会报错：

```python
>>> d['Thomas']

Traceback (most recent call last):
  File "<pyshell#8>", line 1, in <module>
    d['Thomas']
KeyError: 'Thomas'
```

要避免 key 不存在的错误，有两种方法，一是通过 `in` 判断 key 是否存在：

```python
>>> 'Thomas' in d
False
```

二是通过 `dict` 提供的 `get()` 方法，如果 key 不存在，可以返回 `None`，或者自己指定的 value：

```python
>>> d.get('Thomas')
>>> d.get('Thomas', -1)
-1
```

**注意：**返回 `None` 的时候 Python 的交互环境不显示结果。

要删除一个 key，用 `pop(key)` 方法，对应的 value 也会从 `dict` 删除：

```python
{'Michael': 95, 'Bob': 75, 'Tracy': 85, 'Adam': 67, 'Jack': 88}
>>> d.pop('Bob')
75
>>> d
{'Michael': 95, 'Tracy': 85, 'Adam': 67, 'Jack': 88}
```

**请务必注意，**`dict` 内部存放的顺序和 key 放入的顺序是没有关系的。

和 `list` 比较，`dict` 有以下特点：

1.  查找和插入的速度极快，不会随着 key 的增加而变慢。
2.  需要占用大量的内存，内存浪费多。

而 `list` 相反：

1.  查找和插入的时间随着元素的增加而增加。
2.  占用空间小，浪费内存很少。

所以，`dict` 是用空间来换取时间的一种方法。

`dict` 可以用在需要高速查找的很多地方，在 Python 代码中几乎无处不在，正确使用 `dict` 非常重要，需要牢记的第一条就是 `dict` 的 key 必须是**不可变对象**。这是因为 `dict` 根据 key 来计算 value 的存储位置，如果每次计算相同的 key 得出的结果不同，那 `dict` 内部就完全混乱了，这个通过 `key` 计算位置的算法称为哈希算法。要保证 hash 正确性，作为 key 的对象就不能变。在 Python 中，字符串、整数等都是不可变的，隐刺，可以放心用作 key。而 `list` 是可以变的，就不能作为 key：

```python
>>> key = [1, 2, 3]
>>> d[key] = 'a list'
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: unhashable type: 'list'
```



#### ② 集合 set

`set` 和 `dict` 类似，也是一组 key 的集合，但不存储 value。由于 key 不能重复，所以，在 `set` 中，没有重复的的 key。

要创建一个 `set`，需要提供一个 `list` 作为输入集合：

```python
>>> s = set([1, 2, 3])
>>> s
{1, 2, 3}
```

注意，传入的参数 `[1, 2, 3]` 是一个 `list`，而显示的 `{1, 2, 3}` 只是告诉你这个 `set` 内部有 1，2，3 这 3 个元素，显示的顺序也不表示 `set` 是有序的。

重复的元素在 `set` 中自动被过滤：

```python
>>> s = set([1, 1, 2, 2, 3, 3])
>>> s
{1, 2, 3}
```

通过 `add(key)` 方法可以添加元素到 `set` 中，可以重复添加，但不会有效果：

```python
>>> s.add(4)
>>> s
{1, 2, 3, 4}
>>> s.add(4)
>>> s
{1, 2, 3, 4}
```

通过 `remove(key)` 方法可以删除元素：

```python
>>> s.remove(4)
>>> s
{1, 2, 3}
```

`set` 可以看成数学意义上的无序和无重复元素的集合，因此两个 `set` 可以做数学意义上的交集、并集等操作：

```python
>>> s1 = set([1, 2, 3])
>>> s2 = set([2, 3, 4])
>>> s1 & s2
{2, 3}
>>> s1 | s2
{1, 2, 3, 4}
```

`set` 和 `dict` 唯一区别仅在于没有存储对应的 value，但是，`set` 的原理和 `dict` 一样，所以同样不可以放入可变对象，因为无法判断两个可变对象是否相等，也就无法保证 `set` 内部不会有重复元素。

```python
>>> s3 = set([1, [2, 3], 4])
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: unhashable type: 'list'
```

要始终牢记的是，`a`是变量，而`'abc'`才是字符串对象。有些时候，我们经常说，对象`a`的内容是`'abc'`，但其实是指，`a`本身是一个变量，它指向的对象的内容才是`'abc'`：



#### ③ 不可变对象

`str` 是不可变对象，`list` 是可变对象。

对于可变对象，比如 `list`，对 `list` 进行操作，`list` 内部的内容是会变化的：

```python
>>> a = ['c', 'b', 'a']
>>> a.sort()
>>> a
['a', 'b', 'c']
```

而对于不可变对象，比如 `str`：

```python
>>> a = 'abc'
>>> a.replace('a', 'A')
'Abc'
>>> a
'abc'
```