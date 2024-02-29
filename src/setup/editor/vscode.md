{{#include ../../include/header012.md}}

# Visual Studio Code

如果你是 VSCode用户并且你想在这页增加一些内容,请提一个[GitHub Issue][project::cb::issues].

## Rust 语言支持

为了更好支持Rust, 安装 Rust Analyzer 插件.

### 加速 Rust Analyzer

如果你在 `.cargo/config.toml`设置了非默认的linker来加速编译,Rust Analyzer将会忽略它.你还要配置RA才能正常工作,按下面这样设置 (VSCode `settings.json`):

Windows:

```json
"rust-analyzer.cargo.extraEnv": {
    "RUSTFLAGS": "-Clinker=rust-lld.exe"
}
```

Linux (mold):

```json
"rust-analyzer.cargo.extraEnv": {
    "RUSTFLAGS": "-Clinker=clang -Clink-arg=-fuse-ld=mold"
}
```

Linux (lld):

```json
"rust-analyzer.cargo.extraEnv": {
    "RUSTFLAGS": "-Clinker=clang -Clink-arg=-fuse-ld=lld"
}
```

## `CARGO_MANIFEST_DIR`

运行你的app/game时,Bevy会搜索`BEVY_ASSET_ROOT` 或 `CARGO_MANIFEST_DIR`环境变量指定的`assets` 文件夹.
这可以使`cargo run`在终端正确工作.

如果你想通过VSCode的非标准方式(例如debugger)运行你的项目,你必须确认这个配置是生效的.

如果没有设置这个,bevy会在它所在的文件夹搜索可执行`assets`文件,这使得分发容易.但是在开发时,由于你的可执行文件被`cargo`放在了`target`目录,bevy不能正确找到`assets`.

下面的代码片段展示了怎么创建一个用于调试 Bevy 的运行配置
(使用 `lldb`):

(这是为了开发 Bevy ,并使用`breakout`示例进行测试)

(根据你的需求自行修改)

```json
{
    "type": "lldb",
    "request": "launch",
    "name": "Debug example 'breakout'",
    "cargo": {
        "args": [
            "build",
            "--example=breakout",
            "--package=bevy"
        ],
        "filter": {
            "name": "breakout",
            "kind": "example"
        }
    },
    "args": [],
    "cwd": "${workspaceFolder}",
    "env": {
        "CARGO_MANIFEST_DIR": "${workspaceFolder}",
    }
}
```

为了支持动态链接,你还应该在`"env"`部分中增加:

Linux:

```json
"LD_LIBRARY_PATH": "${workspaceFolder}/target/debug/deps:${env:HOME}/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib",
```

(如果你使用不同的工具链或者架构,记得替换 `stable-x86_64-unknown-linux-gnu` )

Windows: I don't know. If you do, please [file an issue][project::cb::issues]!
