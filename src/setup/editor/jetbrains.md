{{#include ../../include/header012.md}}

# JetBrains (RustRover, IntelliJ, CLion)

如果你是JetBrains 用户并且你想在这页增加一些内容,请提一个 [GitHub Issue][project::cb::issues].

## Rust 语言支持

使用 [queries][cb::query] 时, 由于Bevy使用了过程宏,类型信息会丢失.可以启用[procedural macro support][intellij-rust::6908]来修复.

1. 在 `Help | Find Action` 对话框中输入`Experimental feature`
2. 启用 `org.rust.cargo.evaluate.build.scripts` 和 `org.rust.macros.proc`
