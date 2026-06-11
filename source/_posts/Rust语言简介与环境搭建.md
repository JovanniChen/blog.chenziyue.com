---
title: Rust 语言简介与环境搭建
date: 2026-06-01 11:00:00
tags:
---

## 1. Rust 语言历史

Rust 是一门注重安全性、并发性和性能的系统编程语言。

- **创始人**：Graydon Hoare，Mozilla Research 工程师
- **2006 年**：Graydon Hoare 开始个人项目开发 Rust
- **2009 年**：Mozilla 开始赞助 Rust 项目
- **2010 年**：Rust 首次公开亮相（Mozilla Summit 2010）
- **2012 年**：发布第一个 alpha 版本（0.1）
- **2015 年 5 月 15 日**：Rust 1.0 正式发布，标志着语言的稳定
- **2018 年**：发布 Rust 2018 Edition，引入异步编程等重要特性
- **2021 年**：
  - 发布 Rust 2021 Edition
  - Rust 基金会成立，成员包括 AWS、Google、华为、Microsoft、Mozilla
- **至今**：Rust 连续多年在 Stack Overflow 调查中被评为"最受喜爱的编程语言"

```
# Rust 版本发布时间线
2006 ──── Graydon Hoare 个人项目
2009 ──── Mozilla 赞助
2010 ──── 首次公开
2015 ──── 1.0 稳定版发布
2018 ──── Rust 2018 Edition
2021 ──── Rust 2021 Edition / Rust 基金会成立
2024 ──── Rust 2024 Edition
```

---

## 2. Rust 设计目标

Rust 的设计围绕三大核心目标：

### 2.1 安全性（Safety）

Rust 在编译期通过所有权系统、借用检查器等机制消除内存安全问题，无需垃圾回收器（GC）。

```rust
fn main() {
    // Rust 在编译期就能捕获常见的内存错误
    let s1 = String::from("hello");
    let s2 = s1; // s1 的所有权移动到 s2
    // println!("{}", s1); // 编译错误！s1 已失效，防止了 use-after-free
    println!("{}", s2); // 正常输出：hello
}
```

### 2.2 并发性（Concurrency）

Rust 的类型系统和所有权模型在编译期就能防止数据竞争（data race）。

```rust
use std::thread;

fn main() {
    let mut handles = vec![];

    for i in 0..5 {
        // 每个线程拥有自己的数据副本，不会产生数据竞争
        let handle = thread::spawn(move || {
            println!("线程 {} 正在运行", i);
        });
        handles.push(handle);
    }

    for handle in handles {
        handle.join().unwrap();
    }
}
```

### 2.3 实用性（Practicality）

Rust 提供现代化的工具链、优秀的错误信息和丰富的标准库，旨在成为实际生产中可用的语言。

```rust
fn main() {
    // Rust 提供了丰富的迭代器和函数式编程支持
    let sum: i32 = (1..=100)
        .filter(|x| x % 2 == 0)  // 筛选偶数
        .map(|x| x * x)          // 求平方
        .sum();                    // 求和

    println!("1到100中偶数的平方和 = {}", sum);
}
```

---

## 3. Rust 的核心特点

### 3.1 零成本抽象（Zero-Cost Abstractions）

高层抽象不会带来运行时开销，编译后的代码与手写底层代码一样高效。

```rust
fn main() {
    // 使用迭代器的高级抽象，编译后与手写循环性能一致
    let numbers = vec![1, 2, 3, 4, 5];

    // 高级抽象写法
    let doubled: Vec<i32> = numbers.iter().map(|&x| x * 2).collect();
    println!("高级抽象：{:?}", doubled);

    // 等价的手写循环（性能完全相同）
    let mut doubled_manual = Vec::new();
    for &x in &numbers {
        doubled_manual.push(x * 2);
    }
    println!("手写循环：{:?}", doubled_manual);
}
```

### 3.2 内存安全无 GC

Rust 通过所有权系统在编译期管理内存，既不需要手动管理（如 C/C++），也不需要垃圾回收（如 Java/Go）。

```rust
fn main() {
    {
        let s = String::from("hello"); // s 在此作用域有效
        println!("{}", s);
    } // s 离开作用域，内存自动释放（调用 drop），无需 GC

    // println!("{}", s); // 编译错误：s 已不存在
}
```

### 3.3 并发安全

Rust 在编译期阻止数据竞争，被称为"无畏并发"（Fearless Concurrency）。

```rust
use std::sync::{Arc, Mutex};
use std::thread;

fn main() {
    // 使用 Arc（原子引用计数）和 Mutex（互斥锁）安全共享数据
    let counter = Arc::new(Mutex::new(0));
    let mut handles = vec![];

    for _ in 0..10 {
        let counter = Arc::clone(&counter);
        let handle = thread::spawn(move || {
            let mut num = counter.lock().unwrap();
            *num += 1;
        });
        handles.push(handle);
    }

    for handle in handles {
        handle.join().unwrap();
    }

    println!("计数结果：{}", *counter.lock().unwrap()); // 输出：10
}
```

### 3.4 所有权系统

Rust 独特的所有权系统是其最核心的特性，通过编译期检查保证内存安全。

```rust
fn main() {
    // 所有权三大规则的简单演示

    // 规则1：每个值都有一个所有者
    let s1 = String::from("hello");

    // 规则2：同一时刻只有一个所有者
    let s2 = s1; // 所有权从 s1 转移到 s2

    // 规则3：所有者离开作用域时，值被丢弃
    println!("{}", s2);
    // 当 s2 离开作用域时，"hello" 的内存被自动释放
}
```

---

## 4. Rust 应用场景

| 应用场景 | 说明 | 代表项目 |
|---------|------|---------|
| 系统编程 | 操作系统、驱动、嵌入式系统 | Redox OS、Linux 内核模块 |
| WebAssembly | 高性能 Web 应用 | Yew、Leptos、wasm-pack |
| 嵌入式开发 | 微控制器、IoT 设备 | embedded-hal、RTIC |
| CLI 工具 | 命令行工具 | ripgrep、bat、fd、exa |
| Web 后端 | 高性能 Web 服务 | Actix Web、Axum、Rocket |
| 区块链 | 智能合约、区块链节点 | Solana、Polkadot |
| 游戏开发 | 游戏引擎 | Bevy、Amethyst |
| 数据库 | 数据库引擎 | TiKV、SurrealDB |
| 网络编程 | 网络协议、代理 | Tokio、Hyper |

```rust
fn main() {
    // Rust 适合的场景示例：简单的命令行工具
    let args: Vec<String> = std::env::args().collect();

    if args.len() > 1 {
        println!("你好，{}！欢迎学习 Rust！", args[1]);
    } else {
        println!("用法：{} <你的名字>", args[0]);
        println!("Rust 可用于构建高性能的 CLI 工具！");
    }
}
```

---

## 5. 知名 Rust 项目

### Servo
Mozilla 开发的高性能浏览器引擎，是 Rust 语言的旗舰项目。

### ripgrep (rg)
由 Andrew Galloway 开发的超快搜索工具，比 GNU grep 快数倍。

### Deno
由 Node.js 创始人 Ryan Dahl 使用 Rust 构建的 JavaScript/TypeScript 运行时。

### Tokio
Rust 的异步运行时，是 Rust 异步生态的基石。

### Actix Web
基于 Actor 模型的高性能 Web 框架，曾在 TechEmpower 基准测试中名列前茅。

### 其他知名项目
- **Firecracker**：AWS 的微虚拟机管理器，用于 AWS Lambda
- **SWC**：超快的 JavaScript/TypeScript 编译器
- **Alacritty**：GPU 加速终端模拟器
- **Starship**：跨 shell 的命令提示符
- **bat**：带语法高亮的 cat 替代品
- **fd**：快速的 find 替代品
- **delta**：更好的 git diff 查看器
- **Tauri**：轻量级桌面应用框架（Electron 的替代品）
- **Polars**：高性能 DataFrame 库

```rust
fn main() {
    let projects = vec![
        ("Servo",       "浏览器引擎",           "Mozilla"),
        ("ripgrep",     "超快搜索工具",         "Andrew Galloway"),
        ("Deno",        "JS/TS 运行时",         "Ryan Dahl"),
        ("Tokio",       "异步运行时",           "Tokio 团队"),
        ("Actix Web",   "Web 框架",            "Nikolay Kim"),
        ("Firecracker", "微虚拟机管理器",       "AWS"),
        ("Tauri",       "桌面应用框架",         "Tauri 团队"),
    ];

    println!("{:<15} {:<20} {:<15}", "项目", "描述", "开发者/团队");
    println!("{}", "-".repeat(50));
    for (name, desc, dev) in &projects {
        println!("{:<15} {:<20} {:<15}", name, desc, dev);
    }
}
```

---

## 6. 安装 Rust

### 6.1 使用 rustup 安装（推荐）

`rustup` 是 Rust 官方的安装器和版本管理工具。

#### macOS / Linux

```bash
# 一行命令安装 Rust（推荐方式）
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

# 安装完成后，配置环境变量
source $HOME/.cargo/env

# 验证安装
rustc --version
cargo --version
rustup --version
```

#### Windows

```powershell
# 方法1：下载 rustup-init.exe 运行
# 访问 https://rustup.rs 下载安装器

# 方法2：使用 winget
winget install Rustlang.Rustup

# 方法3：使用 scoop
scoop install rustup
```

#### macOS 使用 Homebrew（替代方式）

```bash
# 也可以通过 Homebrew 安装（但推荐使用 rustup）
brew install rustup-init
rustup-init
```

### 6.2 验证安装

```bash
# 查看 Rust 编译器版本
$ rustc --version
rustc 1.75.0 (82e1608df 2023-12-21)

# 查看 Cargo 版本
$ cargo --version
cargo 1.75.0 (1d8b05cdd 2023-11-20)

# 查看 rustup 版本
$ rustup --version
rustup 1.26.0 (5af9b9484 2023-04-05)
```

### 6.3 更新与卸载

```bash
# 更新 Rust 到最新版本
rustup update

# 查看已安装的工具链
rustup show

# 卸载 Rust
rustup self uninstall
```

---

## 7. 工具链介绍

### 7.1 rustc — Rust 编译器

`rustc` 是 Rust 的编译器，用于将 `.rs` 源文件编译为可执行文件。

```bash
# 编译单个文件
rustc main.rs

# 编译并指定输出名称
rustc main.rs -o myapp

# 编译为 release 模式（优化）
rustc -O main.rs

# 查看编译器详细信息
rustc --version --verbose
```

### 7.2 cargo — 包管理器与构建工具

`cargo` 是 Rust 的官方包管理器和构建系统，是日常开发中最常用的工具。

```bash
# 创建新项目
cargo new my_project          # 创建二进制项目
cargo new my_lib --lib        # 创建库项目

# 构建项目
cargo build                   # Debug 构建
cargo build --release         # Release 构建（优化）

# 运行项目
cargo run                     # 构建并运行
cargo run --release           # Release 模式运行

# 检查代码（不生成二进制文件，速度更快）
cargo check

# 运行测试
cargo test

# 生成文档
cargo doc --open

# 发布到 crates.io
cargo publish
```

### 7.3 rustup — 工具链管理器

```bash
# 安装 nightly 版本
rustup install nightly

# 切换默认工具链
rustup default nightly
rustup default stable

# 为特定项目设置工具链
rustup override set nightly

# 添加编译目标（例如 WebAssembly）
rustup target add wasm32-unknown-unknown

# 添加组件
rustup component add rustfmt
rustup component add clippy
```

### 7.4 rustfmt — 代码格式化工具

```bash
# 格式化单个文件
rustfmt src/main.rs

# 使用 cargo 格式化整个项目
cargo fmt

# 检查格式（不修改文件）
cargo fmt -- --check
```

### 7.5 clippy — Lint 工具

```bash
# 运行 clippy 检查代码
cargo clippy

# 自动修复 clippy 建议
cargo clippy --fix

# 将所有警告视为错误
cargo clippy -- -D warnings
```

---

## 8. 第一个 Hello World 程序

### 8.1 直接使用 rustc

创建文件 `hello.rs`：

```rust
// 这是 Rust 的入口函数
// fn 关键字用于定义函数
// main 是程序的入口点
fn main() {
    // println! 是一个宏（macro），不是函数
    // 注意末尾的感叹号 ! 表示这是宏调用
    println!("Hello, World!");
    println!("你好，Rust！");

    // 使用格式化输出
    let name = "Rustacean";  // Rust 开发者的昵称
    let year = 2015;
    println!("我是一名 {}，Rust 于 {} 年发布 1.0 版本！", name, year);
}
```

编译并运行：

```bash
$ rustc hello.rs
$ ./hello
Hello, World!
你好，Rust！
我是一名 Rustacean，Rust 于 2015 年发布 1.0 版本！
```

### 8.2 使用 Cargo（推荐）

```bash
# 创建新项目
$ cargo new hello_rust
     Created binary (application) `hello_rust` package

# 查看项目结构
$ tree hello_rust
hello_rust/
├── Cargo.toml
└── src
    └── main.rs

# 进入项目目录并运行
$ cd hello_rust
$ cargo run
   Compiling hello_rust v0.1.0 (/path/to/hello_rust)
    Finished dev [unoptimized + debuginfo] target(s) in 0.5s
     Running `target/debug/hello_rust`
Hello, world!
```

Cargo 自动生成的 `src/main.rs`：

```rust
fn main() {
    println!("Hello, world!");
}
```

---

## 9. Cargo 项目管理

### 9.1 cargo new — 创建项目

```bash
# 创建二进制（可执行文件）项目
cargo new my_app
# 等价于
cargo new my_app --bin

# 创建库项目
cargo new my_lib --lib

# 在已有目录中初始化项目
cargo init
cargo init --lib
```

项目结构：

```
my_app/
├── Cargo.toml          # 项目配置文件
├── .gitignore          # Git 忽略文件（自动生成）
└── src/
    └── main.rs         # 二进制项目的入口文件

my_lib/
├── Cargo.toml
├── .gitignore
└── src/
    └── lib.rs          # 库项目的入口文件
```

### 9.2 cargo build — 构建项目

```bash
# Debug 构建（默认）
cargo build
# 输出位置：target/debug/my_app

# Release 构建（优化编译，速度更快，体积更小）
cargo build --release
# 输出位置：target/release/my_app

# 查看构建产物大小对比
$ ls -lh target/debug/my_app
-rwxr-xr-x 1 user user 3.8M ...
$ ls -lh target/release/my_app
-rwxr-xr-x 1 user user 380K ...
```

### 9.3 cargo run — 构建并运行

```bash
# 构建并运行
cargo run

# 带参数运行
cargo run -- arg1 arg2

# Release 模式运行
cargo run --release

# 运行特定的 binary
cargo run --bin my_binary
```

### 9.4 cargo check — 快速检查

```bash
# 只检查能否编译，不生成可执行文件（比 cargo build 快很多）
cargo check
```

### 9.5 cargo test — 运行测试

```bash
# 运行所有测试
cargo test

# 运行特定测试
cargo test test_name

# 显示测试中的 println! 输出
cargo test -- --nocapture

# 运行文档测试
cargo test --doc
```

示例测试代码：

```rust
// src/lib.rs
pub fn add(a: i32, b: i32) -> i32 {
    a + b
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_add() {
        assert_eq!(add(2, 3), 5);
    }

    #[test]
    fn test_add_negative() {
        assert_eq!(add(-1, 1), 0);
    }
}
```

### 9.6 cargo doc — 生成文档

```bash
# 生成文档并在浏览器中打开
cargo doc --open

# 包含依赖的文档
cargo doc --document-private-items
```

示例文档注释：

```rust
/// 计算两个数的和
///
/// # 参数
///
/// * `a` - 第一个加数
/// * `b` - 第二个加数
///
/// # 返回值
///
/// 返回 `a` 和 `b` 的和
///
/// # 示例
///
/// ```
/// let result = my_lib::add(2, 3);
/// assert_eq!(result, 5);
/// ```
pub fn add(a: i32, b: i32) -> i32 {
    a + b
}
```

### 9.7 其他常用 Cargo 命令

```bash
# 清理构建产物
cargo clean

# 更新依赖
cargo update

# 安装二进制 crate
cargo install ripgrep

# 查看依赖树
cargo tree

# 审计依赖安全性
cargo audit    # 需要先安装：cargo install cargo-audit

# 查看项目的编译速度分析
cargo build --timings
```

---

## 10. Cargo.toml 配置文件详解

`Cargo.toml` 是 Rust 项目的核心配置文件，使用 TOML 格式。

```toml
# ============ 包信息 ============
[package]
name = "my_project"              # 包名
version = "0.1.0"                # 版本号（遵循语义化版本）
edition = "2021"                 # Rust Edition
authors = ["张三 <zhangsan@example.com>"]  # 作者
description = "一个示例 Rust 项目"          # 项目描述
license = "MIT OR Apache-2.0"              # 许可证
repository = "https://github.com/user/repo" # 仓库地址
readme = "README.md"                        # README 文件
keywords = ["example", "rust"]              # 关键词（最多5个）
categories = ["command-line-utilities"]      # 分类
rust-version = "1.70"                       # 最低 Rust 版本

# ============ 依赖 ============
[dependencies]
serde = "1.0"                    # 简写形式，等同于 "^1.0"
serde_json = "1.0"
tokio = { version = "1", features = ["full"] }  # 指定 features
rand = { version = "0.8", optional = true }      # 可选依赖

# 从 Git 仓库引入
# my_crate = { git = "https://github.com/user/repo", branch = "main" }

# 从本地路径引入
# my_local_crate = { path = "../my_local_crate" }

# ============ 开发依赖（仅在测试和基准测试中使用） ============
[dev-dependencies]
criterion = "0.5"                # 基准测试框架
tempfile = "3.0"                 # 临时文件工具

# ============ 构建依赖（build.rs 中使用） ============
[build-dependencies]
cc = "1.0"

# ============ Features（功能标志） ============
[features]
default = ["json"]               # 默认启用的 features
json = ["serde", "serde_json"]   # 自定义 feature
random = ["rand"]                # 自定义 feature

# ============ 编译配置 ============
[profile.dev]
opt-level = 0                    # Debug 模式优化级别（0-3）
debug = true                     # 包含调试信息

[profile.release]
opt-level = 3                    # Release 模式优化级别
lto = true                       # 启用链接时优化
strip = true                     # 去除调试符号

# ============ 工作区（多包项目） ============
# [workspace]
# members = [
#     "crate_a",
#     "crate_b",
# ]
```

### 版本号语义

```toml
[dependencies]
# 版本要求语法
crate_a = "1.2.3"         # ^1.2.3  >=1.2.3, <2.0.0
crate_b = "~1.2.3"        # ~1.2.3  >=1.2.3, <1.3.0
crate_c = ">=1.0, <2.0"   # 范围指定
crate_d = "=1.2.3"        # 精确版本
crate_e = "*"             # 任意版本（不推荐）
```

---

## 11. Rust 版本与 Edition

### 11.1 发布通道（Channel）

Rust 有三个发布通道：

| 通道 | 说明 | 更新频率 |
|------|------|---------|
| **Stable** | 稳定版，推荐用于生产环境 | 每 6 周发布一次 |
| **Beta** | 测试版，下一个稳定版的候选 | 每 6 周发布一次 |
| **Nightly** | 每日构建，包含实验性功能 | 每天发布 |

```bash
# 安装不同版本
rustup install stable
rustup install beta
rustup install nightly

# 切换默认版本
rustup default stable
rustup default nightly

# 使用特定版本运行
rustup run nightly cargo build
cargo +nightly build    # 简写形式

# 为当前项目指定版本
rustup override set nightly
```

### 11.2 Edition（版次）

Edition 是 Rust 的"方言版本"，每 3 年发布一次，允许进行向后不兼容的语法改进。

| Edition | 年份 | 主要变化 |
|---------|------|---------|
| **2015** | 2015 | 初始 Edition |
| **2018** | 2018 | 模块系统改进、async/await、NLL（非词法生命周期）|
| **2021** | 2021 | 闭包捕获改进、IntoIterator for array、新的 prelude |
| **2024** | 2024 | gen 块、unsafe 改进、RPIT lifetime capture 规则 |

**重要**：不同 Edition 的 crate 可以互相依赖！Edition 不会分裂生态系统。

```toml
# 在 Cargo.toml 中指定 Edition
[package]
name = "my_project"
version = "0.1.0"
edition = "2021"    # 推荐使用最新的 Edition
```

```bash
# 迁移到新的 Edition
cargo fix --edition
```

---

## 12. IDE 推荐

### 12.1 VS Code + rust-analyzer（推荐）

VS Code 是最流行的 Rust 开发编辑器，配合 rust-analyzer 插件可以获得极好的开发体验。

```bash
# 安装 rust-analyzer
# 方法1：在 VS Code 扩展市场搜索 "rust-analyzer" 并安装
# 方法2：使用命令行
code --install-extension rust-lang.rust-analyzer
```

推荐的 VS Code 配置（`settings.json`）：

```json
{
    "rust-analyzer.check.command": "clippy",
    "rust-analyzer.inlayHints.typeHints.enable": true,
    "rust-analyzer.inlayHints.parameterHints.enable": true,
    "rust-analyzer.procMacro.enable": true,
    "editor.formatOnSave": true,
    "[rust]": {
        "editor.defaultFormatter": "rust-lang.rust-analyzer"
    }
}
```

**rust-analyzer 核心功能**：
- 代码补全
- 类型推断提示
- 转到定义 / 查找引用
- 重构（重命名、提取函数等）
- 内联错误提示
- 代码格式化
- 宏展开预览

### 12.2 RustRover（JetBrains）

JetBrains 推出的专用 Rust IDE，提供开箱即用的完整开发体验。

**主要特性**：
- 智能代码补全
- 强大的调试器
- 内置终端
- Git 集成
- 数据库工具
- 非商业用途免费

### 12.3 其他编辑器

```
编辑器           插件/LSP 支持
─────────────────────────────
Neovim           rust-analyzer (通过 nvim-lspconfig)
Emacs            rust-mode + rust-analyzer (通过 lsp-mode 或 eglot)
Sublime Text     rust-analyzer (通过 LSP 插件)
Helix            内置 rust-analyzer 支持
Zed              内置 Rust 支持
```

---

## 13. Rust Playground

Rust Playground 是一个在线 Rust 代码运行环境，无需安装任何工具即可编写和运行 Rust 代码。

- **地址**：[https://play.rust-lang.org](https://play.rust-lang.org)

**功能特性**：
- 支持 Stable、Beta、Nightly 三个通道
- 支持选择不同的 Edition（2015/2018/2021）
- 支持 Debug 和 Release 模式
- 可以查看编译后的汇编（ASM）、LLVM IR、MIR、WASM
- 可以使用 rustfmt 格式化代码
- 可以使用 clippy 检查代码
- 支持分享代码（生成短链接）
- 提供常用 crate（如 serde、rand、regex 等）

```rust
// 在 Playground 中运行这段代码试试
fn main() {
    let languages = vec!["Rust", "Go", "Python", "C++", "Java"];

    println!("编程语言排名（按我的喜好）：");
    for (i, lang) in languages.iter().enumerate() {
        println!("  {}. {}", i + 1, lang);
    }

    // 使用 Rust 的函数式编程特性
    let rust_is_awesome = languages
        .iter()
        .find(|&&lang| lang == "Rust")
        .is_some();

    if rust_is_awesome {
        println!("\nRust 在列表中！让我们开始学习吧！");
    }
}
```

---

## 总结

本章我们了解了：

1. **Rust 的历史与设计哲学**：安全性、并发性、实用性
2. **核心特点**：零成本抽象、内存安全无 GC、并发安全、所有权系统
3. **安装与工具链**：rustup、rustc、cargo、rustfmt、clippy
4. **项目管理**：Cargo 的各种命令和 Cargo.toml 配置
5. **版本体系**：Stable/Beta/Nightly 通道和 Edition 机制
6. **开发环境**：VS Code + rust-analyzer 和其他 IDE 选择

下一章我们将深入学习 Rust 的基础语法与数据类型。
