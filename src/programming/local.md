{{#include ../include/header09.md}}

# 本地资源

官方示例:
[`ecs_guide`][example::ecs_guide].

---

本地资源允许每个[system][cb::system]有自己的数据.这些数据不存储在ECS World,而是和system一起存储.

[`Local<T>`][bevy::Local] 是类似于[`ResMut<T>`][bevy::ResMut]的system 参数, 可以访问一些独立于entity 和 component的可变的数据实例.

[`Res<T>`][bevy::Res]/[`ResMut<T>`][bevy::ResMut] 是所有system共享的全局引用.另一方面,每个[`Local<T>`][bevy::Local]都是单独的实例.

```rust,no_run,noplayground
{{#include ../code/src/basics.rs:local-resource}}
```

必须实现 [`Default`][std::Default] 或者 [`FromWorld`][bevy::FromWorld]. 这样会自动初始化.

一个system可以有多个相同类型的[`Local`][bevy::Local].

## 指定初始值

[`Local<T>`][bevy::Local] 总是使用默认值初始化.

如果你想指定值,可以使用闭包.使用system参数的Rust闭包也是有效的Bevy system,就像独立函数一样.可以用闭包 "把数据放进函数".

下面例子展示如何不用[`Local<T>`][bevy::Local]初始化数据:

```rust,no_run,noplayground
{{#include ../code/src/basics.rs:local-config}}
```
另一种方式是 从 "构造器中"中"return" system:

```rust,no_run,noplayground
{{#include ../code/src/basics.rs:local-config-return}}
```
