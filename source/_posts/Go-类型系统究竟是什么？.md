---
title: Go 类型系统究竟是什么？
date: 2026-11-06 17:56:34
tags:
---

### 一、类型系统到底是什么？

先看一段非常简单的 Go 代码：

```go
package main

import "fmt"

func main() {
    var age int = 25
    var price float64 = 99.9
    var name string = "Go"

    fmt.Println(age, price, name)
}
```

这段代码没有什么特别之处，我们定义了三个变量：

- `age` 的类型是 `int`。
- `price` 的类型是 `float64`。
- `name` 的类型是 `string`。

在初学 Go 的时候，我们通常只把类型理解成变量的一个属性，例如：

```go
var age int
```

意思就是定义一个整数变量，但从编程语言的角度来看，类型的作用远不止告诉编译器变量应该占用多少内存，类型系统实际上承担着多项职责。

**第一，描述数据的结构。**

例如：

```go
type User struct {
    ID       int64
    Username string
    Age      int
}
```

`User` 描述了一种数据结构，规定了它包含哪些字段，以及每个字段分别是什么类型。

**第二，约束允许执行的操作。**

例如：

```go
var age int = 25

age = age + 1
```

整数支持加法，因此这个表达式合法，但下面的代码不合法：

```go
var name string = "Go"

name = name + 1
```

因为 Go 不允许直接将 `string` 和 `int` 相加。

**第三，约束不同数据之间的关系。**

例如：

```go
var age int = 25
var price float64 = 99.9

age = price
```

编译器会拒绝这段代码，原因不是它不知道如何转化，而是 Go 的类型规则不允许把一个 `float64` 值直接赋给 `int` 变量，如果确实需要转换，必须明确表达：

```go
age = int(price)
```

**第四，支持抽象和多态。**

例如：

```go
type Animal interface {
    Speak()
}

type Dog struct{}

func (Dog) Speak() {
    fmt.Println("汪汪")
}

func main() {
    var animal Animal = Dog{}
    animal.Speak()
}
```

这里的 `Animal` 不描述具体的数据结构，而是描述一种行为约束。只要某个类型实现了 `Speak()` 方法，就可以满足这个接口，这就是 Go 类型系统支持接口抽象的方式，所以，我们可以先建立一个认识：

**Go 的类型系统不仅决定变量是什么，还决定值可以进行什么操作、不同类型之间能否建立关系，以及程序能否通过编译。**
