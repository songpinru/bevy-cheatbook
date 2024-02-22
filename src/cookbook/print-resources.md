{{#include ../include/header012.md}}

# 所有 Resource 类型清单

这里展示了如何打印所有已添加为[resources][cb::res]的类型的清单.

```rust,no_run,noplayground
{{#include ../code012/examples/print-resources.rs:example}}
```

```rust,no_run,noplayground
{{#include ../code012/examples/print-resources.rs:app}}
```
它列出了[ECS World][cb::ecs-intro-data] (由所有注册的插件，您自己的插件等)中当前存在的所有resources的类型。

请注意，这不会给你提供一个所有可以用于resource的类型清单。
为此，你应该查阅API文档，查找实现 [`Resource`][bevy::Resource] trait的类型。


[有关 Bevy 中提供的类型的摘要，请参阅此处][chapter::builtins]

