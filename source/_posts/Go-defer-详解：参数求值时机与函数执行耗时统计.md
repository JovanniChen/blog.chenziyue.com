---
title: Go defer 详解：参数求值时机与函数执行耗时统计
date: 2026-11-13 22:45:21
categories:
  - Go
tags:
  - Go
  - defer
  - 性能分析
---

## 一、问题引入

在 Go 语言中，`defer` 是一个非常常用的关键字，通常用于延迟执行函数调用，例如：

- 文件操作结束后关闭文件。
- 数据库操作结束后释放连接。
- 加锁后在函数退出时解锁。
- 函数执行结束时记录日志。
- 统计函数执行消耗。

今天我们通过一段代码，深入理解 `defer` 的一个重要特性：**defer 后面的函数参数会在注册 defer 时立即求值，而不是等到函数真正执行时才求值。**

先看代码：

```go
package main

import (
    "fmt"
    "time"
)

func main() {
    defer TrackTime(time.Now())

    time.Sleep(500 * time.Millisecond)
}

func TrackTime(pre time.Time) time.Duration {
    elapsed := time.Since(pre)
    fmt.Println("elapsed:", elapsed)
    return elapsed
}
```

运行结束大致如下：

```
elapsed: 500.8ms
```

这里有一个值得思考的问题：

`TrackTime()` 明明是在 `main()` 函数即将结束时才执行的，为什么它能够获取到 `main()` 执行开始时的时间？

这就涉及 Go 中 defer 的参数求值机制。

---

## 二、defer 的核心执行规则

首先，需要明确 Go 中 defer 的两个重要规则：

1. 执行 defer 语句时，会立即对被延迟函数调用所需的函数值和参数求值。
2. 被延迟的函数调用不会立即执行，而是在外围函数即将返回时执行。

简单来说：

**defer 延迟的是函数调用，而不是函数参数的求值。**

### 2.1 通过简单示例理解

```go
package main

import "fmt"

func main() {
    x := 10

    defer fmt.Println("x:", x)

    x = 20
}
```

运行结果：

```
x: 10
```

为什么输出的是 10，而不是 20？我们按照执行顺序分析。

**第一步：定义变量**

```go
x := 10
```

此时变量 `x` 的值为 10。

**第二步：注册 defer**

```go
defer fmt.Println("x:", x)
```

执行这条语句时，Go 会立即计算函数参数，此时参数分别是：

- `"x:"`
- `x`，当前值为 10。

Go 保存这次延迟调用机器参数，等待后续执行。

**第三步：修改变量**

```go
x = 20
```

此时变量 `x` 的值变成了 20，但是，之前注册的 defer 调用已经保存了参数值 10，因此不会受到后续修改的影响。

**第四步：执行 defer**

`main()` 函数准备返回，执行之前注册的 defer，最终输出：

```
x: 10
```

这就是 defer 参数立即求值的特性。

---

## 三、原始代码的执行过程分析

