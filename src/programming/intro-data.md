{{#include ../include/header012.md}}

# 介绍: 数据

这一页是个概览,给你展示下Bevy大体上怎么工作的.点击链接跳转到对应的页面了解详情.

---
如[ECS 介绍][cb::ecs-intro]所述,Bevy帮你存储你的数据并使你可以在需要时灵活访问,无论何时何地.

ESC的数据结构被称为 [`World`][bevy::World].它存储并管理着所有的数据.
在复杂场景,可能会有[多个world][cb::multi-world],并且每个都表现的像自己是独立的ESC.
然而,通常你只有一个为[App][cb::app]设置的main World

您可以用两种不同的方式表示数据:
[Entities/Components](#entities--components), and [Resources](#resources).

## Entities / Components

从概念上来讲,你可以把它与数据库或电子表格中的表进行类比.
不同的数据类型([Components][cb::component]),就像表里的"列",
然后还有很多的"行"([Entities][cb::entity])包含多个component的值或实例.
[`Entity`][bevy::Entity] ID就像是行数,他是找到特定component值的一个整数索引.

Component的类型是空的`struct`(被称为[标记component][cb::component-marker]).
他们使用起来就像标签一样,用于标记特定的entity,或者开启某种行为.例如,你可以用于表示玩家entity,标记玩家正在追击的敌人,当前关卡结束时要销毁的entity等.


以下是一个图示，有助于您可视化逻辑结构。勾选标记显示每个entity上存在的component类型。
空单元格表示该component不存在。在此示例中，我们有一个玩家、一个摄像头和几个敌人。

|[`Entity`][bevy::Entity] (ID)|[`Transform`][bevy::Transform]|`Player`|`Enemy`|[`Camera`][bevy::Camera]|`Health`|...|
|---|---|---|---|---|---|---|
|...|||||||
|107|✓ `<translation>` `<rotation>` `<scale>`|✓|||✓ `50.0`||
|108|✓ `<translation>` `<rotation>` `<scale>`||✓||✓ `25.0`||
|109|✓ `<translation>` `<rotation>` `<scale>`|||✓ `<camera data>`|||
|110|✓ `<translation>` `<rotation>` `<scale>`||✓||✓ `10.0`||
|111|✓ `<translation>` `<rotation>` `<scale>`||✓||✓ `25.0`||
|...|||||||

以这种方式表示事物会给您带来灵活性.例如，你可以为你的游戏创建一个 `Health` component。
然后，你可以有许多entity代表你游戏中的不同事物，如玩家、NPC 或怪物，所有这些事物都可以有一个 `Health` 值（以及其他相关component）

一个显而易见的模式是使用entity表达 "游戏/场景中的对象",比如相机,玩家,敌人,光源,属性,UI元素等等.
当然,也不局限于此.ECS是通用的数据结构,你可以创建entity和component来保存任何数据.
例如,你可以创建一个entity保存一系列的设置或配置参数,以及其他抽象物.

访问entity和component保存的数据需要用[queries][cb::query].比如,你想实现一个游戏机制,写一个[system][cb::system],
指定了哪个component你想访问,然后做些事情.你可以遍历所有的匹配的entity,或者访问你指定的那个(如果你知道他们的[`Entity`][bevy::Entity] ID的话)

```rust,no_run,noplayground
{{#include ../code012/src/programming/intro_data.rs:query}}
```

Bevy能自动追踪你的[systems][cb::system]访问的数据,并在多核CPU上[并行运行][cb::system-parallel].
这样,你可以无障碍的使用多线程!

如果你想修改你的数据结构本身(而不是仅仅访问已存在的数据),例如创建或者移除entity和component,你需要使用其他方法.
Bevy不能改变内存布局因为其他的system可能在运行.可以使用[Commands][cb::commands]来缓存/延迟执行这种操作,
等数据不冲突时执行.你也可以使用[exclusive systems][cb::exclusive]来[cb::exclusive][直接访问 World][cb::world],
如果你想立即执行某些操作(但是不是多线程执行).

[Bundles][cb::bundle]像是一组常用component的"模板",帮助你生成新的entity,而不会确实某些东西.

```rust,no_run,noplayground
{{#include ../code012/src/programming/intro_data.rs:commands}}
```

### 与面向对象相比较

面向对象编程告诉你要把一切看作 "对象",每个对象都是 "class" 的一个实例.class在一个位置为该类型的所有对象指定数据和功能.
同一个class的对象有类似的数据(值不一样)和一样的函数.


这与ECS相反.在ECS,任何[entity][cb::entity]都可以有任意数据(任意的[components][cb::component]组合).
entity是用来标识数据的.你的[systems][cb::system]是操纵你的数据的方法,他们可以轻松找到他们所需的数据,并实现所需的行为.

如果你是一个面向对象的程序员,你可能会倾向于定义一个包含玩家所有字段/属性的大型单体`struct Player`.
在Bevy中，这种做法被认为是不良实践，因为这样做会使处理数据变得更加困难，并可能限制性能。
相反，当不同的数据片段可能需要独立访问时，你应该使它们更加细化。

例如,在游戏中玩家可以表示为一个entity,由单独的表示健康值,XP,或者其他游戏相关内容的component(单独的`struct`)组成.
你还可以把[`Transform`][bevy::Transform] ([transform][cb::transform])这种标准的Bevy componet附加到该实体上.

然后,每一个功能项(每个[system][cb::system])只需使用[query][cb::query]查询它所需的数据.
通用的功能(如 健康值/伤害 系统)可以被应用与任何匹配了component的entity,不论是玩家还是游戏中的其他东西.

但是,如果数据经常被一起访问,你应该把他们放进一个单独的`struct`.
例如,bevy的[`Transform`][bevy::Transform] 和 [`Color`][bevy::Color].
这中方式适用于字段不会被独立使用的场景.

### 额外内部细节

组成entity的component集合/组合被称为entity的原型.Bevy在内部跟踪他们,以组织RAM中的数据.
具有相同原型的entity的数据被一起存储,这允许CPU高效的访问和混村这些数据.

如果你在已存在的entity上增加/移除component,你就在修改原型,可能会使Bevy复制数据到其他位置.

[Bevy的component存储.][cb::component-storage]

Entity IDs是可重复使用的.[`Entity`][bevy::Entity]实际上有两个整型数字:ID和 "generation".
你销毁一些entity后,他们的ID可以被重新用于新生成的entity,但是bevy会自增他们的generation.

## Resources

如果某个东西是全局唯一的(单例),而且它不与其他数据关联,使用[Resource][cb::res].

例如,你可以创建一个resource来保存你的游戏的图形设置,或者当前的游戏模式及进度.

这是个简单的存储数据方式,只要你知道你不需要Entities/Components的灵活性.

```rust,no_run,noplayground
{{#include ../code012/src/programming/intro_data.rs:res}}
```
