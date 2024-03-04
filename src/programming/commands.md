{{#include ../include/header09.md}}

# Commands

官方示例:
[`ecs_guide`][example::ecs_guide].

---

使用[`Commands`][bevy::Commands]生成/销毁entity,在已存在的entity上添加/删除component,管理resource.

操作不会立马生效,他们会被放进队列中等之后安全时执行.看[stages][cb::stage].

(如果你不使用stage,意味着你的其他[systems][cb::system]会在下帧更新时看到结果)

```rust,no_run,noplayground
{{#include ../code/src/basics.rs:example-commands}}
```
