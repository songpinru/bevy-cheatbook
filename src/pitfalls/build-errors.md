{{#include ../include/header011.md}}

# 奇怪的编译错误

有时，当你试图编译你的项目时，你会遇到奇怪的、令人困惑的编译错误，下面提供一些有用的解决方法。

## 更新你的 Rust

首先，确保你的 Rust 是最新的。当使用 Bevy 时，你必须至少使用最新的稳定版 Rust（或者 nightly 版本）。

如果你正在使用 [`rustup`][rustup] 来管理你的 Rust 安装与升级，你可以运行

```shell
rustup update
```

## 重置 cargo 状态

许多类型的构建错误通常可以通过强制 `cargo`  重新生成它的内部状态（重新计算依赖关系等）来解决。
你可以通过删除`Cargo.lock`文件和 `target` 目录来做到这一点。

```shell
rm -rf target Cargo.lock
```

这样做之后，再试着构建你的项目，很可能那些神秘的错误就会消失。

这一招通常能解决构建失败的问题，但你的问题仍未能解决，那你的问题可能需要进一步的调查。
可以通过 GitHub 或 [Discord][bevy::discord]，在 Bevy 社区中寻求帮助。

如果你使用的是 Bevy main 开发分支，而上述方法又未能解决你的问题，那你的错误可能是由第三方插件引起的。
请看[这里](../setup/bevy-git.md#how-to-use-bleeding-edge-bevy)的解决方案。

{{#include ../include/resolver2.md}}
