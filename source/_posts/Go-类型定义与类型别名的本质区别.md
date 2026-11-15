---
title: Go 类型定义与类型别名的本质区别
date: 2026-11-10 22:04:08
categories:
  - Go
tags:
  - Go
  - 类型系统
  - 类型别名
---

上一篇我们介绍了 Go 类型系统的基本概念，并留下了一个问题：

下面两种写法究竟有什么区别？

**写法一：定义一个新类型**

```go
type UserID int64
type ProductID int64
```

**写法二：定义类型别名**

```
type UserID = int64
type ProductID = int64
```

