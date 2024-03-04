{{#include ../include/header09.md}}

# 直接访问 World

[`World`][bevy::World]是Bevy ECS存储数据和相关元数据的地方.跟踪[resource][cb::res], [entity 和 component][cb::ec].

通常,[`App`][bevy::App]的调度器会在main World中运行所有的[stage][cb::stage](stage中再运行 [systems][cb::system])
普通的[systems][cb::system]被[system参数类型][builtins::systemparam]限制了能从World中访问的数据.
只能通过它自己的`Commands`][cb::commands]操作.这是大多数典型的Bevy用户代码的行为方式.

但是,也有多种直接访问World的方式,可以给你完全的控制权和自由操作Bevy ECS中存储的数据:
 - [Exclusive systems][cb::exclusive]
 - 实现[`FromWorld`][bevy::FromWorld] 
 -  [`App`][bevy::App] [构造器][cb::app]
 - 手动创建 [`World`][bevy::World],用于 [tests][cb::system-tests] 或 scenes
 - 自定义 Commands
 - 实现自定义 [`Stage`][bevy::Stage]  (不推荐, 首选 exclusive systems)

直接访问世界可以做的事情:
 - 随意生成/销毁 entity, 添加/删除 resource,等. 立即生效(不会像普通[system][cb::system]中的 [`Commands`][cb::commands]一样延迟)
 - 访问任何component, entities, resources
 - 手动运行任意 systems 或 stages

如果你想做一些不适合Bevy典型执行模式/仅每帧运行一次的system(由[stages][cb::stage] 和 [labels][cb::system-label] 组织)时,这相当有用.

通过直接访问World,你可以实现自定义控制流,例如多次循环一些system,从文件导出/导入数据,...

## 使用 `World`

以下是一些可以使用直接World访问 API 的方法.

### `SystemState`

最简单的方式是 [`SystemState`][bevy::SystemState].

这是一种 "模仿system"的类型,使用起来像有多个参数的[system][cb::system].也可以用[queries][cb::query], [变更检测][cb::change-detection], and
甚至 [`Commands`][cb::commands].可以使用所有[system参数][builtins::systemparam].

他还跟踪任何持久化状态,用于[变更检测][cb::change-detection] 或者 缓存以提高性能.
因此如果你计划重用同一个[`SystemState`][bevy::SystemState]多次,你应该将其存储在某处,而不是每次创建一个新的.
每次调用`.get(world)`,就像运行了另一个system.

如果你使用[`Commands`][bevy::Commands],你可以选择何时应用它.
你需要在[`SystemState`][bevy::SystemState]手动调用`.apply(world)`来应用它.

```rust,no_run,noplayground
// TODO: write code example
```

### 运行于 Stage

如果你想运行一些system(常见于[testing][cb::system-tests]),最简单的方式是构建一个临时[`SystemStage`][bevy::SystemStage] ([stages][cb::stage]).
这样你可以重用所有Bevy正常运行system时的调度逻辑.

```rust,no_run,noplayground
// TODO: write code example
```

### 元数据导航

world含有很多导航到数据的元数据,比如存储components, entities, archetypes的信息.

```rust,no_run,noplayground
// TODO: write code example
```
