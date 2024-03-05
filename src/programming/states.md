{{#include ../include/header09.md}}

# States

官方示例:
[`state`][example::state].

---

State允许你构造app运行时的 "流程".

你可以通过state实现的事情:

- 一个菜单页或一个加载页
- 暂停或继续游戏
- 不同游戏模式
- …

在每个state下, 你可以有不同的 [systems][cb::system] 被执行. 你也可以在进入或者退出state时添加一次性的设置或用于打扫的system.

要使用state，请定义一个enum类型, 并在你的[app构造器][cb::app]中添加[system sets][cb::systemset]:

```rust,no_run,noplayground
{{#include ../code/src/basics.rs:app-states}}
```

同一个state可以有多个system set.

当你想使用[labels][cb::label] 和 [明确system顺序][cb::system-order]时, 这很有用.

这对[Plugins][cb::plugin]也有用.每个plugin都可以为同一个state添加自己的system set.

state底层是通过[运行条件][cb::runcriteria]实现的.这些特殊的system set构造函数实际上只是为自动添加state和管理执行条件的辅助工具。

## 控制 State

在system内, 你可以通过[`State<T>`][bevy::State] resource 查看或控制state:

```rust,no_run,noplayground
{{#include ../code/src/basics.rs:check-state}}
```

要修改另一个state:

```rust,no_run,noplayground
{{#include ../code/src/basics.rs:change-state}}
```

负责当前state的system执行完后, Bevy会转换到你设定的下一个state.

你可以在一帧更新中做任意数量的state转换.Bevy会处理他们的转换并执行所有相关的system(在进入下一个[stage][cb::stage]之前).

## State 栈

你不需要完全从一个state转换到另一个state，你也可以进行叠加state，形成一个state堆栈。.

这样你就可以实现诸如"游戏暂停"场景、或一个菜单叠加场景的同时，而游戏世界仍然可见或在后台运行。

你可以有一些system，即使在所属state"未激活"时仍然在运行（也就是该state在后台，其他state在它上面运行）。你也可以在"暂停"或"
恢复"state时来添加并运行一次性system。

在[app 构造时][cb::app]:

```rust,no_run,noplayground
{{#include ../code/src/basics.rs:state-stack}}
```

使用 `push`/`pop` 来管理state:

```rust,no_run,noplayground
{{#include ../code/src/basics.rs:state-push-pop}}
```

(如前面讲的,使用 `.set` 来替换堆栈顶部的活动state)

## 已知的陷阱和限制

### 结合其他运行条件

由于state是使用[运行条件][cb::runcriteria]实现的, 所以不能结合其他运行条件使用, 比如[fixed timestep][cb::fixedtimestep].

如果你尝试添加其他运行条件进system set, 会替换Bevy的state管理运行条件!这将使system set不再受state约束!

考虑使用 [`iyes_loopless`][project::iyes_loopless], 这个没有这些限制.

### 多个 Stages

Bevy state不能在多个 [stages][cb::stage] 间交叉工作.有可用替代方案，但它们存在故障和缺陷。

这在实践中这是一个大限制, 限制了你使用[commands][cb::commands].
不能使用Command是个大麻烦,因为你不能在同一帧生成和操作entity, 等等.

考虑使用 [`iyes_loopless`][project::iyes_loopless], 这个没有这些限制.

### 输入

如果你想使用 按钮/键盘 这种 [`Input<T>`][bevy::Input]触发state转换, 你需要使用`.reset`方法手动清理输入:

```rust,no_run,noplayground
{{#include ../code/src/basics.rs:state-input-clear}}
```

(注意需要 [`ResMut`][bevy::ResMut])

不这么做会造成[issues][bevy::1700].

[`iyes_loopless`][project::iyes_loopless] 没有这个问题.

### Events

在不能一直运行的system中接受[events][cb::event], 例如在暂停state, 你会丢失所有接受system没有运行时的event!

为了减轻这种情况, 你可以实现[自定义清理策略][cb::event-manual], 来管理相关event类型的生命周期.
