{{#include ../include/header011.md}}

# Bevy Time vs. Rust/OS time

不要使用[`std::time::Instant::now()`][std::Instant]获得当前时间. 
[从bevy获取时间信息][cb::time], 使用[`Res<Time>`][bevy::Time].

调用std时间函数,Rust(还有操作系统)提供了精确的时间. 但是这不是你想要的.

你的system被Bevy调度器并行运行, 这意味着他们可能在每一帧的不同时刻调用! 这导致 不一致/不确定 的时间,并使游戏不响应或者卡顿.

Bevy的[`Time`][bevy::Time]给你的时间信息, 在每一帧更新周期都是一致的. 它旨在用于游戏逻辑.

这不是特定于 Bevy 的，而是适用于一般的游戏开发。总是从游戏引擎中获取时间，而不是从编程语言或者 操作系统中获取时间。
