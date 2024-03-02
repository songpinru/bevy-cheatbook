{{#include ../include/header012.md}}

# App

可参考的官方例子：所有例子 :)

特别是可以看看完整的游戏例子:
[`alien_cake_addict`][example::alien_cake_addict],
[`breakout`][example::breakout].

---
要进入Bevy的runtime,你需要配置一个 [`App`][bevy::App].通过App你可以定义构成你的项目的所有组成部分:
[plugins][cb::plugin], [systems][cb::system] (及其configuration/metadata:
[run conditions][cb::rc], [ordering][cb::system-order], [sets][cb::systemset]),
[event][cb::event] 类型, [states][cb::state], [schedules][cb::schedule]…

通常在项目的 `main`函数中创建[`App`][bevy::App].但是你不必在main里添加所有的东西.
如果你想从多个地方(比如Rust文件或者crate)给app添加功能,使用[plugins][cb::plugin].
随着你的项目发展,你需要这样来保持项目整洁.

```rust,no_run,noplayground
{{#include ../code012/src/programming/app_builder.rs:main}}
```

注意:`add_systems`/`add_plugins`/`configure_sets` 函数可使用元组一次性添加多个项.

[Component][cb::component] 不需要被注册.

Schedule（[还][bevy::279]）不能在运行时修改，你想运行的所有[systems][cb::system]必须提前在 [`App`][bevy::App] 中添加/配置.
你可以使用[运行条件][cb::rc]控制各个system.也可以使用[`MainScheduleOrder`][bevy::MainScheduleOrder] [resource][cb::res] [动态启用/禁用触发条件][cb::schedule-dynamic].

## Bevy内置功能

Bevy游戏引擎自己的内置功能使用 [plugin 组][cb::plugingroup]表示.
每个Bevy app都要先添加它,有以下两种:
 - [`DefaultPlugins`][bevy::DefaultPlugins] 如果是全功能的game/app
 - [`MinimalPlugins`][bevy::MinimalPlugins] 最小依赖,类似无头服务

## 配置数据

通常,你可以通过[systems][cb::system]来配置[数据][cb::ecs-intro-data].
使用system里的[Commands][cb::commands]或者[独占 systems][cb::exclusive] 来[访问World][cb::world].

使用system设置在启动时需要初始化的东西,或在菜单、游戏模式、级别等之间过渡时执行某些操作，可以使用使用[state][cb::state] 进入/退出 system.

你也可以直接在构造app时初始化数据.这通常用于[resources][cb::res],无论何时,都可以[直接访问World][cb::world]获取.

```rust,no_run,noplayground
{{#include ../code012/src/programming/app_builder.rs:world}}
```

## 退出 App

为了优雅退出bevy,使用[system][cb::system] 发送[`AppExit`][bevy::AppExit] [event][cb::event]:

```rust,no_run,noplayground
{{#include ../code012/src/programming/app_builder.rs:appexit}}
```

对于原型开发，Bevy 提供了一个方便的system，你可以添加这个system，以便在按下 Esc 键时关闭当前聚焦的窗口。当所有窗口都关闭时，Bevy 将自动退出。

```rust,no_run,noplayground
{{#include ../code012/src/programming/app_builder.rs:close_on_esc}}
```
