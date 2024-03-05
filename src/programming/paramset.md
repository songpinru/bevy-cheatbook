{{#include ../include/header012.md}}

# Param Sets

出于安全原因,一个[system][cb::system]不能有多个可能会造成冲突的数据访问的参数.

例子:
 - 多个不兼容的 [queries][cb::query].
 - 使用了[`&World`][bevy::World]的同时还有其他system参数访问指定的数据.
 - …

思考下这个[system][cb::system]:

```rust,no_run,noplayground
{{#include ../code012/src/programming/paramset.rs:conflict}}
```

两个[query][cb::query]都试着可变访问`Health`.他们有不同的[过滤器][cb::query-filter],
但是如果有实体含有`Player` 和 `Enemy` component会发生什么?如果我们知道不会发生,我们可以再加上`Without`过滤器,但是如果可能发生呢?

这样的代码能通过编译(Rust不知道Bevy的ECS语义),但是在运行时会panic.当Bevy尝试运行这个system,它会panic并附带一个system参数冲突的消息:

```
thread 'main' panicked at bevy_ecs/src/system/system_param.rs:225:5:
error[B0001]: Query<&mut game::Health, bevy_ecs::query::filter::With<game::Enemy>> in
system game::reset_health accesses component(s) game::Health in a way that conflicts
with a previous system parameter. Consider using `Without<T>` to create disjoint Queries
or merging conflicting Queries into a `ParamSet`.
```

Bevy 提供了一个解决方案: 用 [`ParamSet`][bevy::ParamSet] 包装不兼容的参数:

```rust,no_run,noplayground
{{#include ../code012/src/programming/paramset.rs:paramset}}
```

这样确保了同时只能有一个会冲突的参数被使用.Bevy现在可以愉快的工作了.

param set最大容纳8个参数.
