{{#include ../include/header012.md}}

# 性能调优

Bevy提供了很多可以在大多数场景提高性能的特性,这些特性大多数都是默认开启的.然而在某些项目中,这可能是负优化.

幸运的是,他们中的大多数都是可配置的.绝大多数的使用者都不该更改这些设置,
但如果你的game在默认配置下遇到性能问题,这个页面会告诉你一些可以尝试修改的项,看看他们是否能帮助你.

Bevy设计默认配置时是考虑了*可伸缩性的*.也就是说,当你在项目中添加更多的功能和复杂性时,不用太过担心性能.
Bevy会为充分利用可用的硬件(GPU,CPU,多线程)而自动分配负载.

然而,这可能对简单的项目有负作用,或者在某些场景下有不良影响.

这个取舍是合理的,因为小而简单的游戏无论如何都会很快,即使有其他的开销,但是大而复杂的游戏会从这种先进的调度器中收益,从而避免瓶颈.
在开发游戏时，不会因为添加更多内容而降低性能。

## 多线程开销

Bevy有一个智能的多线程执行器,你的[systems][cb::system]可以自动在多个CPU内核之间[并行执行][cb::ecs-intro-code],
当没有数据访问冲突时, [遵守排序约束][cb::system-order].
这是非常棒的,因为你可以持续添加system来做更多的事,并在你的游戏中实现更多的特性,Bevy会让你毫不费力的充分利用现代多核CPU!

然而,智能调度器会给所有常用操作(例如每次 [system][cb::system]运行)带来一些额外开销.
在每一帧中几乎没有工作的项目,尤其是如果你的system完成的非常快,那么所有的这些开销加起来会远超实际的工作.

你可能想禁用多线程,实时禁用后你的游戏会不会表现更好.

### 只在Update时禁用多线程

多线程可以在每个[调度阶段][cb::schedule]禁用.这意味着可以很容易只在你的代码/游戏逻辑(在`Update`时)中禁用多线程,Bevy引擎内部的systems仍旧保持启用.

这能加速一些没有太多游戏逻辑的简单游戏,但引擎仍多线程运行.

你可以通过[app builder][cb::app]编辑特定的[schedule][cb::schedule]设置 :

```rust,no_run,noplayground
{{#include ../code012/src/setup/perf.rs:singlethread-updateonly}}
```

### 完全禁用多线程

如果你想完全禁用多线程,你可以移除`multi-threaded`这个cargo特性.

 `Cargo.toml`

```toml
[dependencies.bevy]
version = "0.12"
default-features = false
features = [
   # re-enable everything you need, without `multi-threaded`
   # ...
]
```

[(如何为bevy配置cargo特性)][cb::features]

通常这是不推荐的.Bevy是为多线程设计的.只有在你真的需要的时候(你要进行确实需要如此的特殊构建,例如WASM或者老的硬件)再考虑它.

##多线程配置

你可以配置Bevy使用多少CPU线程.

Bevy为三个不同的目的创建线程:
 - Compute: 你的system运行时和每一帧工作时
 - AsyncCompute: 独立于帧率的后台处理
 - I/O: 加载资源和其他的磁盘/网络活动

默认条件下, Bevy在以下情况 *拆分/分区* 可用的CPU线程:
 - I/O: 25% 可用CPU线程, 最小 1, 最大 4
 - AsyncCompute: 25% 可用CPU线程, 最小 1, 最大 4
 - Compute: 所有剩余的线程

这意味着没有 *过度规划* .每个硬件CPU线程都用于一个特定的目的.

这为混合CPU负载提供了平衡.尤其是那些加载了很多资源(特别是游戏中动态加载的资源)的游戏,专用的I/O线程将减少停顿和加载时间.
后台计算将不会影响帧率等.

Examples:

|CPU Cores/Threads|# I/O|# AsyncCompute|# Compute|
|-----------------|-----|--------------|---------|
|1-3              |1    |1             |1        |
|4                |1    |1             |2        |
|6                |2    |2             |2        |
|8                |2    |2             |4        |
|10               |3    |3             |4        |
|12               |3    |3             |6        |
|16               |4    |4             |8        |
|24               |4    |4             |16       |
|32               |4    |4             |24       |

Note: Bevy 目前没有为对非对称（big.LITTLE或Intel P/E内核）CPU做特殊处理.
在理想条件下,使用big/P的核数用于计算,little/E核数用于I/O会比较好.

### 过度规划

如果你的游戏使用非常少的I/O(资源加载)或者后台计算,默认配置可能不是最优的.这些线程大多时间都会空闲.
与此同时,对游戏帧率很重要的帧更新循环计算,被限制在了少数线程.这在少核CPU(少于4线程)影响更严重.

例如,在我的项目中,我通常在加载场景时加载所有的资源,因此I/O线程通常在游戏过程中没有被使用.
我很少使用异步计算.

如果你的游戏也是如此,你可能想让所有的CPU线程可以被用于计算.这可以提升你的帧率,尤其是少核的CPU上.
然而,任何游戏过程中的异步计算或者I/O负载都会影响你的游戏性能/帧率稳定性.

下main时如何修改:

```rust,no_run,noplayground
{{#include ../code012/src/setup/perf.rs:taskpool-overprovision}}
```

这是一个完整自定义配置项的示例:

```rust,no_run,noplayground
{{#include ../code012/src/setup/perf.rs:taskpool-custom}}
```

## 流水线渲染

Bevy 有[流水线渲染架构][cb::render-architecture]. 这意味着Bevy中和GPU相关的[systems][cb::system](这些system运行在CPU上，但为每一帧的GPU工作做准备)将与下一帧的所有正常system并行运行.
Bevy将并行渲染前一帧和下一帧的更新。

这将通过更好地利用CPU的多线程能力来提高GPU的利用率（减少GPU空闲等待CPU分配任务的可能性）.
通常，这可以带来10-30%的更高帧率，有时甚至更多。

然而，这种并行处理也可能影响感知输入延迟（即“点击到显示”延迟），而且通常会使延迟变得更糟。
玩家的输入效果可能会在屏幕上延迟一帧才显示出来。这种延迟可能会被更高的帧率所补偿，也可能不会。
下面是一个图表，用于可视化这种情况：

![Timeline comparing pipelined and non-pipelined rendering. In the pipelined
case, one additional frame is displayed before the effects of the mouse click
can be seen on-screen.](/img/pipelined-latency.png)

实际鼠标点击发生在帧与帧之间.上面图中,#4帧时Bevy检测到输入.
在流水线模式下,前一帧的渲染是并行完成的,因此输入不会影响到下一帧的频幕显示.

非流水线模式下,用户输入会延迟1帧.流水线模式下,用户输入会延迟2帧.

然而，在上图中，帧速率大到足以使由于流水线渲染而增加的输入延迟更快地被处理和显示。你的应用程序可能并没有这么幸运。

---

如果你关心延迟超过帧率,你可能想关闭流水线渲染.为了最低延迟,你可能想[禁用 VSync][cb::vsync].

这痒关闭流水线渲染:

```rust,no_run,noplayground
{{#include ../code012/src/setup/perf.rs:disable-pipelined-rendering}}
```

## 集群前向渲染

默认情况下，Bevy使用集群前向渲染架构来处理3D渲染。
视口（显示游戏画面的屏幕区域）被分割成矩形/体素，以便可以独立处理场景中每个小部分的光照。
这允许你在3D场景中使用大量灯光，而不会破坏性能。

这些集群的尺寸会影响渲染性能。默认设置对于大多数3D游戏来说都是不错的，但根据你的游戏进行微调可能会提高性能。

在采用俯视视角的游戏（如许多策略和模拟游戏）中，大多数灯光往往与相机的距离相似。
在这种情况下，你可能希望减少Z切片的数量（这样屏幕就被分割成更小的X/Y矩形，但每个矩形覆盖更多的距离/深度）：

```rust,no_run,noplayground
{{#include ../code012/src/setup/perf.rs:cluster-smallz}}
```

对于使用非常少的灯光或灯光影响整个场景的游戏（例如在小房间/室内区域），您可能想要尝试禁用集群渲染:

```rust,no_run,noplayground
{{#include ../code012/src/setup/perf.rs:cluster-single}}
```

除非在特殊情况下,修改这些设置可能会降低性能.
