{{#include ../include/header09.md}}

# System 执行顺序

Bevy的调度算法是为了发挥最大性能设计的,在可用的CPU线程上尽可能并行最多的system.

当system之间没有数据冲突时,可能发挥最大性能.但是当一个system需要某个数据的可变(独享)访问权时,其他需要访问这个数据的system不能在同一时刻运行.
Bevy通过system的函数标签(其参数的类型)来确定这些.

在某些情况下,默认执行顺序是不确定的.Bevy不关心system何时运行,每一帧system的运行顺序都可能不同.

## 有何影响?

许多情况下,你不需要担心.

但是,有时你需要某些system以特殊的顺序运行.例如:

- 可能你写的某个system的数据需要在另一个system先修改完?
- 一个system需要接收另一个system发送的[events][cb::event]
- 你使用了 [变更检测][cb::change-detection]

这些情况下,system以错误的顺序运行可能使他们到下一帧才生效.少部分情况下,按照游戏逻辑,可能会造成严重的逻辑bug!

这是否重要取决于你.

一般游戏中的许多东西,比如果汁的视觉效果,延迟一帧不是什么问题.可能不值得为此担忧.如果你不在乎,随机的顺序可能会获得更好的性能.

另一方面,比如处理玩家输入这类,可能会导致烦人的延迟,你应该尽可能修复它.

## 明确 System 顺序

如果一个system必须在另一个system之前或之后运行,你可以添加顺序约束:

```rust,no_run,noplayground
{{#include ../code/src/basics.rs:system-order}}
```

`.before`/`.after` 可能根据需要在一个system上多次使用.

## 标签

对于更高级的用例,你可以使用[labels][cb::label].标签可以是字符串,或者派生`SystemLabel`的自定义类型(比如`enum`).

这样你可以使用一个约束一次性影响多个system.你可以在一个system上打多个标签.你也可以在多个system上使用多个标签.

每个标签都是一个参考点,其他system可以围绕它排序

```rust,no_run,noplayground
{{#include ../code/src/basics.rs:system-labels}}
```

当多个system有相同的标签或者顺序,使用[system sets][cb::systemset]更方便.

## 循环依赖

如果有多个彼此互相依赖的system,那么显然是不可能完全解决循环依赖问题.
If you have multiple systems mutually depending on each other, then it is
clearly impossible to resolve the situation completely like that.

你应该重新设计你的游戏以避免这种情况,或者只能接受现实.你可以按照你想要的方式对system显示排序,至少确保最终结果是可预测的.
