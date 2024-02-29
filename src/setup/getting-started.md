{{#include ../include/header012.md}}

# 入门

本页面涵盖Bevy开发所需的基本设置。

---

对于大多数情况，Bevy就像其他任何Rust库一样。
您需要安装Rust并设置开发环境，就像其他任何Rust项目一样。
您可以使用[Rustup][rustup]安装Rust。
请参阅[Rust's official setup page][rust::getting-started]以获取更详细的说明。

在Linux,您需要某些系统库的开发文件。请参阅[official Bevy Linux dependencies page][bevy::linux-dependencies].

另请参阅[Setup page in the official Bevy Book][bevy::getting-started]和[official Bevy Readme][bevy::readme].

## 创建一个新项目

你可以从IDE/编辑器或者命令行创建一个新的Rust项目:

```sh
cargo new --bin my_game
```

(创建一个名为 `my_game`的新工程)

`Cargo.toml` 文件包含了项目的所有配置.添加最新版本的`bevy`依赖.文件应该看起来像这样:

```toml
[package]
name = "my_game"
version = "0.1.0"
edition = "2021"

[dependencies]
bevy = "0.12"
```

 `src/main.rs` 文件是你主要的源代码文件.从这里开始编写你的Rust代码.对于最小的Bevy [app][cb::app],你最少需要这样:

```rust,no_run,noplayground
{{#include ../code012/examples/minimal.rs}}
```

现在你可以编译并运行你的项目.在编译bevy以及依赖时需要花费一点时间.随后的运行应该很快.你可以从IDE/编辑器执行这个操作,以及命令行:

```sh
cargo run
```

## 文档

你可以这样生成包含你的项目和所有依赖的离线文档 (类似 [docs.rs](https://docs.rs)), 用于脱机使用

```sh
cargo doc --open
```

这各命令会生成HTML文档并在你的浏览器中打开.

这不需要网络连接,并给你提供了一种方便的方式搜索你用到的依赖的API文档,通常比在线版本的文档更有用.

## 可选的其他配置

很快你就可能在Rust默认的dev环境构建时遇到性能低下问题.[参考这里解决.][pitfall::perf]

在持续开发时,重新编译的速度对你很重要,你不必每次测试时都等待Rust编译,构建.[Bevy 入门][bevy::getting-started]里有一些提高编译速度的建议.

你也可以看看[Dev Tools and Editors][cb::tools],里面有一些可能有用的外部开发工具.

## 接下来是?

看看这本书的 [guided tutorial][chapter::tutorial] ,和Bevy提供的 [official examples][bevy::examples]

在[Bevy Assets Website][bevy::assets]可以找到社区提供的其他教程,学习资源,以及可以用到你项目里的[plugins][cb::3rdparty].

加入社区的 [Discord][bevy::discord] 和我们交流!

## 运行遇到问题?

如果遇到问题,先看看[Common Pitfalls][chapter::pitfalls] 是否可以帮助到你. Bevy 社区成员最常见的一些问题的解决方案都记录在那里.

如果需要帮助, 使用 [GitHub Discussions][bevy::ghdiscussions], 或者欢迎在 [Discord][bevy::discord]向我们寻求帮助.

{{#include ../include/gpu-driver-requirements.md}}
