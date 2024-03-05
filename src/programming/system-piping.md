{{#include ../include/header09.md}}

# System 链

官方示例:
[`system_piping`][example::system_piping].

---

你可以把多个Rust函数组合成一个Bevy [system][cb::system].

你可以把一堆有输入和输出的函数链接到一起,变成一个大的system.这被称为 "system链".

你可以认为是创造了一个由多个块组成的 system "模块".这样,你就能在多个system中重用一些通用的 代码/逻辑. 

注意system链不是一种system间的交流方式. 如果你想在system间传递数据, 你应该使用[Event][cb::event].

## 例子: 处理 [`Result`][std::Result]s

system链的一个用处是返回错误(可以使用Rust的`?`操作符),然后由一个单独的函数处理错误:

```rust,no_run,noplayground
{{#include ../code/src/basics.rs:system-io}}
```
这些函数不能被单独[注册][cb::app]成system(Bevy不知道如何处理输入/输出).通过把他们"链"在一起,我们创建了一个合理的system: 

```rust,no_run,noplayground
{{#include ../code/src/basics.rs:system-pipe}}
```

## 性能警告

小心:Bevy对待整个system链就像是一个大的system,具有所有的system参数和数据访问要求.这导致并行化会被限制,影响性能.

如果你创建了多个 "system链",他们都有一个包含可变访问的公共函数,那么这会阻止所有system并行执行!
