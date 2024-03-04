{{#include ../include/header09.md}}

# Stages

Bevy会运行所有包含在stage中的[systems][cb::system].每帧更新,Bevy按顺序执行每个stage.
在每个stage,Bevy的调度算法可以同时运行很多system,使用多核CPU获得更好的性能.

stage之间的边界实际上是硬性同步点.确保所有上一个stage的system完成,然后才开始下一个stage的system,
并且有个时刻没有system处于执行中.

这可以使 [Commands][cb::commands]安全应用.任何system通过 [`Commands`][bevy::Commands] 执行的操作都在这个stage结束时应用.

{{#include ../include/builtins.md:stages}}

默认情况下,当你添加system时,会进入[`CoreStage::Update`][bevy::CoreStage].
Startup systems会进入[`StartupStage::Startup`][bevy::StartupStage].

Bevy的内部system在其他stage,以确保他们被排在相对游戏逻辑正确的顺序.

如果你想添加你自己的system进Bevy内部的stage,你需要小心与Bevy内部system的意外交互.
切记:Bevy 内部是使用普通system 和 ECS实现的，就像你自己的东西一样!

你可以增加自己的stage.比如,如果我希望debug system在游戏逻辑后执行:

```rust,no_run,noplayground
{{#include ../code/src/basics.rs:custom-stage}}
```

如果你需要管理system相对于另一个system何时运行,一般最好不要使用stage控制,而是使用[明确system顺序][cb::system-order].
stage限制了游戏的并行能力和性能.

另外,当你真的要确保前一个system已经完成时,stage使事情容易.stage也是应用[Commands][cb::commands]唯一方法.

如果你的system依赖其他system中使用[Commands][cb::commands]执行的动作时,并且需要在相同的帧内执行,放在不同的stage是唯一实现方法.
