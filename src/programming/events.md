{{#include ../include/header011.md}}

# Events

官方示例:
[`event`][example::event].

---

通过event,可以让[systems][cb::system]彼此沟通!

类似于[resources][cb::res] 和 [components][cb::component], event也是Rust `struct`s or `enum`s.只需要创建一个新的类型,派生[`Event`][bevy::Event] trait.

然后,所有的[system][cb::system]都可以发送(广播)刚才创建的类型,所有system也可以接收这种event.

 - 发送events, 使用 [`EventWriter<T>`][bevy::EventWriter].
 - 接收events, 使用 [`EventReader<T>`][bevy::EventReader].

每个 reader会独自追踪要读的event,因此可以多个[system][cb::system]处理同一个event.

```rust,no_run,noplayground
{{#include ../code011/src/programming/events.rs:events}}
```

你需要通过[app构造][cb::app]注册你的event:

```rust,no_run,noplayground
{{#include ../code011/src/programming/events.rs:events-appbuilder}}
```

## 使用建议

event应该是你首选的数据流工具.因为event可以从任何[system][cb::system]发送并被多个system接收,用途广泛.

event是非常有用的抽象层,帮助你解耦.你可以分离不同的功能块,更容易明确[system][cb::system]的责任范围.

你可以想想,一个简单的 "玩家升级" 展示场景,使用event可以让我们轻松扩展我们游戏功能.
如果我们想要展示一个花哨的升级效果或动画,更新UI,等等,我们可以添加更多读event的system,然后做各自的事情.
如果`player_level_up` 是一个不用event的简单的检查玩家XP和直接管理玩家等级的system,将是一个笨拙的游戏开发方式.

## 如何工作

当注册event的类型时,Bevy会创建一个[`Events<T>`][bevy::Events] [resource][cb::res],类似于一个后台存储event的队列.
Bevy也添加了一个用于每帧清理event的 "event维护" [system][cb::system],防止他们耗尽内存.

event存储是 双缓存,意味着event在发送后的下一帧仍然保留。这为你的system提供了在下一帧读取event的机会，如果它们在当前帧没有得到处理的话。

如果不喜欢这种方式,[你可以手动控制event何时清理][cb::event-manual](如果忘记清理,会内存泄露/浪费)

[`EventWriter<T>`][bevy::EventWriter] system参数只是向队列添加event的语法糖,用来访问可变的[`Events<T>`][bevy::Events] [resource][cb::res].
[`EventReader<T>`][bevy::EventReader]稍复杂一点:他访问不可变event和一个你已经读取event的整数计数器.这是它需要使用`mut`关键字的原因.

## 可能的陷阱

小心帧延迟/帧滞后.如果bevy接收的system先于发送的system运行是可能会发生.
此时接收system在下一帧更新前只有一次机会接收event.如果要确保event被立即/同帧处理,可以使用[明确system顺序][cb::system-order].

如果没有每帧或者每两帧帧至少一次读取event,小心丢失event.常见于使用了[fixed timestep][cb::fixedtimestep] 或 [run conditions][cb::rc].

如果你希望event保留时间超过2帧,你可以[实现自定义清理/管理策略][cb::event-manual].
但是你只可以处理你自己的event类型,不能处理Bevy的内置event类型.
