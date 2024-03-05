{{#include ../include/header012.md}}

# Non-Send Resources

"Non-send" 指的是那些只能被 "main线程" 访问的数据类型. 这种数据被Rust标记为 `!Send`(没有实现 [`Send`][std::Send] trait).

一些库(通常是system)含有不能被其他线程安全使用的接口.常见于很多低级操作系统接口,比如窗口,图形,声音.
如果你正在写一个用了这些接口的Bevy plugin,你可能会需要这个.

通常,Bevy在一个线程池里运行所有的[systems][cb::system],利用多核CPU.
但是,你可能需要确保一些代码只会在 "主线程" 执行,或者访问的数据在多线程下是不安全的.

## Non-Send System 和数据访问

你可以使用[`NonSend<T>`][bevy::NonSend] /[`NonSendMut<T>`][bevy::NonSendMut] system 参数.
就像使用[`Res<T>`][bevy::Res] / [`ResMut<T>`][bevy::ResMut], 让你可以访问ECS [resource][cb::res] (一个包含数据的全局实例),
这类参数会强制Bevy scheduler只会在主线程上执行这个[system][cb::system].
这确保了数据不会在线程间传递或者从不同线程访问.

Bevy中有一个这种resource的例子,[`WinitWindows`][bevy::WinitWindows].
你通常用来管理窗口的[window entity][cb::window]底层就是它.
他给了你直接访问操作系统窗口管理的能力.

```rust,no_run,noplayground
{{#include ../code012/src/programming/non_send.rs:nonsend}}
```
```rust,no_run,noplayground
{{#include ../code012/src/programming/non_send.rs:nonsend-app}}
```

## 自定义 Non-Send Resources

通常, 为了进入 [resources][cb::res], 他们的类型必须是[`Send`][std::Send].

bevy单独追踪non-Send resource,确保他们只能被[`NonSend<T>`][bevy::NonSend] /[`NonSendMut<T>`][bevy::NonSendMut]访问.

不能使用[`Commands`][cb::commands] 插入non-send resource, 只能使用[直接访问World][cb::world].
这意味着你必须在[exclusive system][cb::exclusive], [`FromWorld`][bevy::FromWorld] 实现,或者自定义stage 中初始化他们.

```rust,no_run,noplayground
{{#include ../code012/src/programming/non_send.rs:insert-nonsend}}
```
```rust,no_run,noplayground
{{#include ../code012/src/programming/non_send.rs:insert-nonsend-app}}
```
