{{#include ../include/header012.md}}

# 介绍: 代码

这一页是个概览,给你展示下Bevy大体上怎么工作的.点击链接跳转到对应的页面了解详情.

---

如[ECS 介绍][cb::ecs-intro]所述,Bevy 管理所有的功能/行为,适时的运行他们,并给他们相应[数据][cb::ecs-intro-data]的访问权.

独立的功能被称为[systems][cb::system].每个system就是一个Rust的函数(`fn`),入参必须是能表达它所访问[数据][cb::ecs-intro-data]的[特殊参数类型][cb::system-param].
可以将函数签名视为从ECS [`World`][bevy::World]中提取东西的 "规范".

下面是一个可能的[system][cb::system].只需要看函数参数,我们就知道需要访问什么[数据][cb::ecs-intro-data].

```rust,no_run,noplayground
{{#include ../code012/src/programming/intro_code.rs:example-system}}
```

(参考: [systems][cb::system], [queries][cb::query], [commands][cb::commands], [resources][cb::res], [entities][cb::entity], [components][cb::component])

## 并行 System

基于你写的[systems][cb::system]的[参数][cb::system-param]类型,Bevy知道每个system访问的数据而不会和其他system冲突.
不会冲突的system(不会同时修改数据)会自动[并行运行][cb::system-parallel]于不同的CPU线程.
这样,你可以多线程,无障碍使用现代多核CPU.

为了最好的并发性能,建议你的功能和[数据][cb::ecs-intro-data]保持较小的粒度.
创建很多小的system,每个实现单一的目标,访问仅需的数据.这样可以让bevy更容易并行执行.
在一个system中执行太多的功能或者太多的数据在一个[component][cb::component] 或 [resource][cb::res] 的 `struct`,会限制并行能力.

Bevy默认是不确定的并行.你的system相对另一个system运行顺序是不可预测的,除非你增加[ordering][cb::system-order]来约束.

## 独占 System

[独占][cb::exclusive] system给了你[完全直接访问][cb::world]ECS [`World`][cb::World] 的方式.
他们可以与其他system并行运行,因为他们可以访问任何东西做任何事.有时你可能需要这种能力.

```rust,no_run,noplayground
{{#include ../code012/src/programming/intro_code.rs:exclusive}}
```

## Schedules

Bevy把system保存在 [schedules][cb::schedule] ([`Schedule`][bevy::Schedule])里.
schedule包含了system和组织他们的元数据,告诉bevy何时何地执行. 
Bevy [Apps][cb::App]通常含有多个schedule,每个都是不同场景(每帧更新,[定时更新][cb::fixedtimestep],app启动,[state][cb::state]变化等)下调用的system的集合

schedule中保存的元数据可以控制system何时执行:
 -  [运行条件][cb::rc] 控制system在调度期间是否运行.这样你可以在某些时间禁用某个system
 -  [顺序][cb::system-order] 约束,如果一个system依赖于另一个system完成.

在schedule中,system可以被组合成[set][cb::systemset].set可以让多个system共享相同的元数据/配置.
system继承他所属的set的配置.set也可以继承其他的set.

这是一个帮你理解schedule的逻辑结构的图.让我们来看看一个假设的“Update”（每帧运行）游戏调度可能会如何组织.

List of [systems][cb::system]:

|[System][cb::system] name|[Sets][cb::systemset] it belongs to|[Run conditions][cb::rc]|[Ordering constraints][cb::system-order]|
|---|---|---|---|
|`footstep_sound`|`AudioSet` `GameplaySet`||`after(player_movement)` `after(enemy_movement)`|
|`player_movement`|`GameplaySet`|`player_alive` `not(cutscene)`|`after(InputSet)`|
|`camera_movement`|`GameplaySet`||`after(InputSet)`|
|`enemy_movement`|`EnemyAiSet`|||
|`enemy_spawn`|`EnemyAiSet`|||
|`enemy_despawn`|`EnemyAiSet`||`before(enemy_spawn)`|
|`mouse_input`|`InputSet`|`mouse_enabled`||
|`controller_input`|`InputSet`|`gamepad_enabled`||
|`background_music`|`AudioSet`|||
|`ui_button_animate`||||
|`menu_logo_animate`|`MainMenuSet`|||
|`menu_button_sound`|`MainMenuSet` `AudioSet`|||
|...||||

List of [sets][cb::systemset]:

|[Set][cb::systemset] name|Parent Sets|[Run conditions][cb::rc]|[Ordering constraints][cb::system-order]|
|---|---|---|---|
|`MainMenuSet`||`in_state(MainMenu)`||
|`GameplaySet`||`in_state(InGame)`||
|`InputSet`|`GameplaySet`|||
|`EnemyAiSet`|`GameplaySet`|`not(cutscene)`|`after(player_movement)`|
|`AudioSet`||`not(audio_muted)`||

请注意，在调度中列出system的顺序并不重要。它们的执行[顺序][cb::system-order]由元数据确定。
Bevy会尊重这些约束，但除此之外，它会尽可能并行运行系统，具体取决于可用的CPU线程。

还请注意，我们的假设游戏是使用许多单独的小型system来实现的。例如，我们不是在`player_movement`system中播放音频，
而是创建了一个单独的`play_footstep_sounds` system。这两个功能可能需要访问不同的[数据][cb::ecs-intro-data]，
因此将它们放在不同的system中可以让Bevy有更多的并行机会。作为单独的system，它们还可以有不同的配置。
`play_footstep_sounds` system可以添加到AudioSet[set][cb::systemset]中，从中继承一个`not(audio_muted)`[运行条件][cb::rc]。

类似地，我们将鼠标和控制器输入放在单独的system中。`InputSet`set允许像`player_movement`这样的system一次性共享对它们的所有排序依赖关系。

你可以看到，Bevy的调度API为你提供了很大的灵活性来组织游戏中的所有功能。你会如何利用这种强大的功能呢？;)

---

下图解释了[schedule][cb::schedule]在代码中如何创建的:

```rust,no_run,noplayground
{{#include ../code012/src/programming/intro_code.rs:example-scheduling}}
```

(learn more about: [schedules][cb::schedule], [system sets][cb::systemset], [states][cb::state], [run conditions][cb::rc], [system ordering][cb::system-order])
