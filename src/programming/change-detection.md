{{#include ../include/header09.md}}

# 变化检测

官方示例:
[`component_change_detection`][example::component_change_detection].

---

Bevy允许你轻松的检测数据何时变化. 这样你可以执行一些操作来相应变动.

主要用例是优化, 禁用不必要的工作,只在数据变化时执行. 另一个用例是在变动发生是触发特殊操作,比如配置一些东西或者给某处发送数据.

## Component

### 过滤

你可以弄一个[query][cb::query],查询指定[component][cb::component]被修改了的实体.

使用[query 过滤器][cb::query-filter]:
 - [`Added<T>`][bevy::Added]: 检测新component实例
   - 如果该组件被添加到一个现有的实体中
   - 如果一个带有该组件的新实体被创建出来
  - [`Changed<T>`][bevy::Changed]: 检测component实例变化
   - component被可修改访问时触发
   - component被新增时也触发 (同 [`Added`][bevy::Added])

(如果你想相应删除, 看这里 [移除检测][cb::removal-detection]. 它工作方式不同, 使用起来也更麻烦.)

```rust,no_run,noplayground
{{#include ../code/src/basics.rs:change-detection}}
```

### 查看

如果你只想访问所有entity,不管他们有没有被修改,只是想查看它们的状态.
你可以使用[`ChangeTrackers<T>`][bevy::ChangeTrackers] query参数.

```rust,no_run,noplayground
{{#include ../code/src/basics.rs:changetrackers}}
```

处理所有entity时很有用,可以根据是否修改过做不同的事情.

## Resources

对于[resource][cb::res],变更检测给[`Res`][bevy::Res]/[`ResMut`][bevy::ResMut] system 参数 提供了一个特殊的方法.

```rust,no_run,noplayground
{{#include ../code/src/basics.rs:changed-res}}
```

注意变更检测目前不能用于[state][cb::state]变化(通过[`State`][bevy::State] [resource][cb::res]) (这是[bug][bevy::2343]).

## 能检测到什么?

[`Changed`][bevy::Changed] 检测是被[`DerefMut`][std::DerefMut]触发的.
通过一个可变query访问component,没有使用`&mut`访问就不会触发.

这使得变更检测非常准确.你可以用于优化游戏的性能,或者触发其他事情.

注意:你修改一个component时,bevy不会追踪新值是否和旧值一样.总会触发变更检测.如果你不想这样,只需要自己检查一下:

```rust,no_run,noplayground
{{#include ../code/src/basics.rs:change-if-wrap}}
```

变更检测工作粒度是每个[system][cb::system],这是可靠的.一个system不会感知它自己造成的变动,
只会感知其他system造成的, 并且只有上次看到之后的变化(这个system上次运行之后发生的变化).
如果你的system只是偶尔运行(比如用了 [state][cb::state] 或 [运行条件][cb::runcriteria]),
你不必担心错过变动.

## 可能的陷阱

小心帧延迟/帧滞后. 如果检测system先于修改system运行. 检测system会在下次运行看到变动,通常是下一帧更新时.

如果你需要确保变动 立刻/在同一帧 被处理,你可以[明确system顺序][cb::system-order].

但是,当添加的组件检测使用[`Added<T>`][bevy::Added] (通常使用[`Commands`][cb::commands]添加)时, 明确顺序是不够的, 你需要使用[stages][cb::stage].

# 移除检测

官方示例:
[`removal_detection`][example::removal_detection].

---

移除检测时特殊的.因为,不同于 [变更检测][cb::change-detection],数据不再存在于ECS了(显而易见),所以bevy不能追踪到他的元数据.

即使如此,能够相应移除变动对一些应用很重要,因此Bevy提供了有限的方式.

## Component

你可以查看当前帧被移除的[components][cb::component].数据会在每帧更新后清理.
注意这种用法很麻烦,而且要求你使用多个[stages][cb::stage].

当你移除一个component(使用[Commands][cb::commands] ([`Commands`][bevy::Commands])), 
实际上操作会在这个[stage][cb::stage]的最后执行. 检查移除的[system][cb::system] 必须在同一帧更新的后一个stage.
否则,就检测不到移除.

使用[`RemovedComponents<T>`][bevy::RemovedComponents] 特殊system参数类型,获得的 [`Entity`][bevy::Entity] ID迭代器,
迭代器里是这一帧所有被移除且拥有 `T` 类型的component.

```rust,no_run,noplayground
{{#include ../code/src/basics.rs:removal-detection}}
```

(你可以使用`Entity` ID 配合[`Commands::entity()`][cb::commands] 或 [`Query::get()`][cb::query] 来对这些entity做点什么.)

## Resource

Bevy不提供任何检测 [resources][cb::res] 被移除的API.

你可以使用[`Option`][std::Option]和单独的[`Local`][cb::local] system参数来解决这个问题,需要你自己实现有效检测.

```rust,no_run,noplayground
{{#include ../code/src/basics.rs:res-removal-detection}}
```

注意, 因为你的system中的检测是local, 所以并不一定是同一帧更新发生的.
