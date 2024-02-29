{{#include ../include/header012.md}}

# 使用前沿 Bevy (bevy main)

Bevy发展非常的快,还有很多未发布的令人激动的东西.这页会给你使用开发中版本bevy的建议.

## 快速开始

如果你不使用第三方插件,并只想使用main分支:

```toml
[dependencies]
bevy = { git = "https://github.com/bevyengine/bevy" }
```

如果你想使用外部插件,你应该阅读本页剩下的部分.你可能要做些额外的兼容工作.

## 你应该使用前沿Bevy吗? 你该使用哪个版本的Bevy?

Bevy 遵循“火车发布”模型，具有宽松的截止日期。每三个月，就会准备一次新的主要发布，其中将包含自上次发布以来的所有新发展（功能、修复等）。
发布日期并不严格，通常会延迟数周以完成后续工作。

此外，Bevy 通常会在每个主要发布后根据需要发布一到两个补丁发布，以修复发布后不久发现的任何错误。
它不会包含所有修复，只是被认为足够关键的小型非破坏性修复。

大多数 Bevy 项目应该使用 crates.io 上的最新版本。
如果您想安全起见，可以在升级到新版本之前等待第一个补丁发布（0.*.1）。
您可能还需要等待您使用的任何第三方插件支持新的 Bevy 版本。

另一方面，对于实验和 Bevy 开发，我们鼓励您尝试从 git 中获取最新的开发中代码！
最新版本通常缺少最新的错误修复、可用性改进和功能。加入其中可能很有吸引力！

如果您是 Bevy 的新手，这可能不适合您。您可能更愿意使用发布的版本。它将与社区插件和文档具有最佳兼容性。

Bevy 的开发中版本经常会有破坏性的更改。因此，对于实际项目来说，使用它可能会非常烦人。
此外，第三方插件的作者通常不会费心保持兼容性。您将经常面临破坏，并且可能不得不自己修复它。

仅建议对更实验性或玩具项目进行此操作。

不过，有一些方法可以帮助您管理破坏并减少问题。感谢 cargo，您可以在方便的时候更新 Bevy，只要您认为自己可以处理任何可能的破坏性更改。

您可能想要考虑对 Bevy 和您使用的任何插件的存储库进行分叉。
使用您自己的fork可以让您轻松地在需要时应用修复，或编辑它们的 `Cargo.toml`  文件。
这样，即使原始存储库进行了破坏性的更改，您也可以继续使用您的项目，而不会受到影响。


如果你使用 Bevy main,强烈建议你通过[Discord][bevy::discord] 和 [GitHub][project::bevy]来联系社区, 以便你可以跟踪正在发生的事情，寻求帮助，或者参与讨论。

## 常见的陷阱：神秘的编译错误

在不同版本的Bevy之间进行更改时(例如,转换现有版本从发布版本到git版本的项目),您可能会得到很多奇怪的意外构建错误.

您通常可以通过删除 `Cargo.lock` 和 `target` 目录来修复:

```sh
rm -rf Cargo.lock target
```

看[这里][pitfall::build-errors] 获得详细信息.

If you are still getting errors, it is probably because cargo is trying
to use multiple different versions of bevy in your dependency tree
simultaneously. This can happen if some of the plugins you use have specified
a different Bevy version/commit from your project.

If you are using any 3rd-party plugins, please consider forking them, so you can
edit their `Cargo.toml` and have control over how everything is configured.

## Cargo Patches

某些情况下,你可能需要"cargo patches"来本地替换依赖.你可能指定插件使用你fork的bevy,而不用fork和编辑插件的`Cargo.toml`,如下:

```toml
# replace the bevy git URL source with ours
[patch."https://github.com/bevyengine/bevy"]
# if we have our own fork
bevy = { git = "https://github.com/me/bevy" }
# if we want to use a local path
bevy = { path = "../bevy" }
# some plugins might depend on individual bevy crates,
# instead of all of bevy, which means we need to patch
# every individual bevy crate specifically:
bevy_ecs = { path = "../bevy/crates/bevy_ecs" }
bevy_app = { path = "../bevy/crates/bevy_app" }
# ...

# replace released versions of crates (crates.io source) with ours
[patch.crates-io]
bevy_some_plugin = { git = "https://github.com/me/bevy_some_plugin", branch = "bevy_main" }
# also replace bevy itself
bevy = { path = "../bevy" }
# ...
```

## 升级 Bevy

建议你在`Cargo.toml`中指定一个经过验证的bevy commit,以便你可以确保仅在需要时才更新,以避免破环.

```toml
bevy = { git = "https://github.com/bevyengine/bevy", rev = "7a1bd34e" }
```

有任何更改,请运行:

```sh
cargo update
```

(或者删除 `Cargo.lock`)

否则,您可能会遇到cargo无法正确解析依赖项而导致的错误.

## 给插件作者的建议

如果你要发布一个插件 crate, 以下是一些建议:
  - 你的仓库中的main分支应该绑定bevy最新发布的版本
  - 在您的存储库中有一个单独的分支,以保持对bevy-main的支持与对已发布的Bevy版本的支持相分离
  - 在README中告诉大家如何找到它
  - 设置CI来提醒你bevy中新的变动是否损坏你的插件

请随意遵循本页面上的所有建议，包括按需使用cargo Patches。cargo Patches仅在您直接构建项目时适用，
而不是作为依赖项，因此它们不会影响您的用户，并且可以安全地保留在您的`Cargo.toml`中。

### CI 设置

这是一个Github Action的示例.每天早上8:00(UTC)会校验你的代码并编译.GitHub会在失败时通知你.

```yaml
name: check if code still compiles

on:
  schedule:
    - cron: '0 8 * * *'

env:
  CARGO_TERM_COLOR: always

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v2

      - name: Install Dependencies
        run: sudo apt-get update && sudo apt-get install g++ pkg-config libx11-dev libasound2-dev libudev-dev
      - uses: actions-rs/toolchain@v1
        with:
          toolchain: stable
          override: true

      - name: Check code
        run: cargo update && cargo check --lib --examples
```
