{{#include ../include/header012.md}}

# Systems

官方示例:
[`ecs_guide`][example::ecs_guide],
[`startup_system`][example::startup_system],
[`system_param`][example::system_param].

---

system是Bevy运行的方法.实际就是Rust的函数.

这是你实现所有游戏逻辑的地方.每个system都需要指定需要访问的东西,Bevy会尽可能的并行运行他们.

这些函数只能接受[特定参数类型][builtins::systemparam],来指定哪些[data][cb::ecs-intro-data]需要使用.
如果你在函数中使用了不支持的参数类型,[你会得到一个编译错误!][pitfall::intosystem].

以下是一些可选项:
 - 使用`Res`][bevy::Res]/[`ResMut`][bevy::ResMut]访问[resources][cb::res]
 - 使用[queries][cb::query] ([`Query`][bevy::Query])访问[entity中的component][cb::component]
 - 使用[Commands][cb::commands] ([`Commands`][bevy::Commands]) 创建/销毁entity,component,resource
 - 使用[`EventWriter`][bevy::EventWriter]/[`EventReader`][bevy::EventReader] 发送/接收 [events][cb::event]

[详细参数列表!][builtins::systemparam]

```rust,no_run,noplayground
{{#include ../code012/src/programming/systems.rs:sys-debug-res}}
```

System 参数可以组合成元组(还可以嵌套). 这样很容易组织参数.

```rust,no_run,noplayground
{{#include ../code012/src/programming/systems.rs:sys-param-tuple}}
```

{{#include ../include/builtins.md:systemparam-limits}}

还有一种特殊的system: [独占system][cb::exclusive].他们拥有[ECS World的完全访问权][cb::world],
可以访问任何所需数据,但是不能并行执行.对于大多数场景,你应该使用普通的system.

```rust,no_run,noplayground
{{#include ../code012/src/programming/systems.rs:exclusive}}
```

## Runtime

你需要把你的system在[app 构建][cb::app]时加载进去:

```rust,no_run,noplayground
{{#include ../code012/src/programming/systems.rs:systems-appbuilder}}
```

小心:编写了一个新的`fn`然后忘记把他加载进app时常见的一个错误!,如果你新写的代码没有运行,先检查有没有加载进app.

对于简单的项目这就足够了.

system被[schedule][cb::schedule]包含.添加成[`Update`][bevy::Update]的system每帧都会执行.
添加成[`Startup`][bevy::Startup]的system只会在开始时执行一次.还有[其他][builtins::schedule].

当你的项目发展的更加复杂时,你需要使用一些Bevy提供的工具来管理何时何地执行system,如
[explicit ordering][cb::system-order], [run conditions][cb::rc], [system
sets][cb::systemset], [states][cb::state].
