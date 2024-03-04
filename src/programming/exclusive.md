{{#include ../include/header09.md}}

# Exclusive Systems

exclusive system 是Bevy不会并行运行的[systems][cb::system].他们可以用`&mut World`参数来 [无限制访问][cb::world] 整个ECS [`World`][bevy::World].

exclusive system中,你有ECS中数据的完全控制权,你可以做任何事.

注意exclusive system会限制性能,因为放弃了多线程(同一时刻不会同时运行).

部分场景下exclusive system很有用:
 - 导出多个entity和component成一个文件,比如实现保存或者加载游戏存档,或者编辑器导出的场景
 - 立刻直接生成/销毁[entity][cb::ec],或者创建/删除[resource][cb::res],无延迟(不同于[`Command`][cb::commands])
 - 使用您自己的调度算法运行任意system

详情请看 [直接访问 World ][cb::world] .

```rust,no_run,noplayground
{{#include ../code/src/basics.rs:exclusive-fn}}
```

同普通system一样,你需要使用`.exclusive_system()`把exclusive system加入[App][cb::app].

exclusive system不会在普通system中排序.只会在以下阶段运行:
 - `.at_start()`:  [stage][cb::stage] 开始时
 - `.at_end()`: [stage][cb::stage] 结束时,普通system调用[commands][cb::commands]之后.
 - `.before_commands()`:  [stage][cb::stage]中所有普通system之后,[commands][cb::commands]调用之前

(如果不指定, 默认是 `.at_start()`)

```rust,no_run,noplayground
{{#include ../code/src/basics.rs:exclusive-app}}
```
