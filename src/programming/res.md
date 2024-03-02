{{#include ../include/header012.md}}

# Resources

官方示例:
[`ecs_guide`][example::ecs_guide].

---

Resource不同于[entity][cb::ec],它可以存储全局唯一的数据.

使用Resource包装的[数据][cb::ecs-intro-data] 一定是全局的,例如配置/设置.任何地方Resource都可以很方便的访问.

---

要创建一个新的resource类型,简单定义一个Rust`struct` 或 `enum`,然后派生[`Resource`][bevy::Resource] trait,
类似[components][cb::component] 和 [events][cb::event].

```rust,no_run,noplayground
{{#include ../code012/src/programming/res.rs:struct}}
```

类型必须时唯一的,每个类型最多只有一个实例.如果你需要多个,考虑[entities and components][cb::ec].

Bevy[很多地方使用了resource][builtins::res].你可以使用这些内置的resource启动引擎的很多特性.它们就像你自己定义的一样.

## 访问 Resource

[systems][cb::system]中使用`Res`/`ResMut` 访问resource:

```rust,no_run,noplayground
{{#include ../code012/src/programming/res.rs:systemparam}}
```

## 管理 Resource

如果你需要在运行时创建/移除resource,可以使用[commands][cb::commands] ([`Commands`][bevy::Commands]):

```rust,no_run,noplayground
{{#include ../code012/src/programming/res.rs:commands}}
```
另外,还可以从[独占system][cb::exclusive] 中[直接访问World][cb::world]:

```rust,no_run,noplayground
{{#include ../code012/src/programming/res.rs:exclusive}}
```

Resource也可以在[app 构建][cb::app]时设置好.这样你的resource从开始就存在.

```rust,no_run,noplayground
{{#include ../code012/src/programming/res.rs:app}}
```

## Resource 初始化

对于简单的resource,实现[`Default`][std::Default]:

```rust,no_run,noplayground
{{#include ../code012/src/programming/res.rs:default}}
```

对于需要复杂初始化的resource, 实现 [`FromWorld`][bevy::FromWorld]:

```rust,no_run,noplayground
{{#include ../code012/src/programming/res.rs:fromworld}}
```

注意:如果过度使用[`FromWorld`][bevy::FromWorld]很容易陷入不可维护.

## 使用建议

何何时使用[entities/components][cb::ec]、或何时使用resource来存储数据，通常是关于你想如何访问[数据][cb::ecs-intro-data]的问题：
使用从任何地方全局访问的模式（resources），还是使用 ECS 模式（entities/components）。

即使在你的游戏中某个对象只有一个（比如单人游戏中的玩家），使用entity而使不是resource也是很合适的，
因为entity是由多个component组成的，其中一些component可以与其他entity共用。这可以使你的游戏逻辑更加灵活。
例如，你可以用一个"健康/伤害系统"，对玩家和敌人都有效。

### 设置

resource的常用方式时存储设置和配置.

但是如果有的东西运行时不会改变并且只会在初始化[plugin][cb::plugin]时使用,考虑把他们放在plugin的`struct`,而不是变成resource.

### 缓存

resource也可以存储一些可能经常使用的数据.例如,[资源处理][cb::handle],使用自定义的数据结构而不是entity和component表示地图,等.

[Entitiy和Component][cb::ec] 尽管非常灵活，但并不一定适合所有用例。
如果你希望以其他方式表示你的数据，请尽管这样做。只需创建一个资源并将其放在那里即可。

### 外部库交互

如果你想集成一些外部非bevy的软件进Bevy app,创建一个resource来保存它的状态/数据是非常方便的.

例如，如果你想要使用外部的物理引擎或音频引擎，你可以将所有的相关数据放在一个resource中，并编写一些system来调用它。
这样，你就可以很容易地从Bevy代码中与它进行交互。

如果外部代码不是线程安全的（在Rust术语中称为`!Send`），这对于非Rust（例如C++和操作系统级别）的库来说是很常见的，
你应该使用一个[非Send][cb::nonsend]的Bevy resource。这将确保任何接触该resource的Bevy system都会在主线程上运行。
