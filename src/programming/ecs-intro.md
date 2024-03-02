{{#include ../include/header012.md}}

# ECS 编程介绍

这页会教你常见的ESC思维/范式.

---

相关示例:
[`ecs_guide`][example::ecs_guide].

完整的游戏示例:
[`alien_cake_addict`][example::alien_cake_addict],
[`breakout`][example::breakout].

---
ESC是一个分隔数据和行为的编程范式.Bevy将会为你保存[你的数据][cb::ecs-intro-data]和[你的各个功能][cb::ecs-intro-code].
代码会在合适的时间运行.你的代码可以在它需要时访问所需数据.

这使得编写具有灵活性和重用性游戏逻辑([Systems][cb::system])容易.例如,你可以实现:

- 健康值和伤害值可以游戏中所有地方以相同方式工作,无论是玩家,NPC,怪物,交通工具
- 引力和碰撞,任何应该有物理学的东西
- UI 中所有按钮的动画或声音效果

当然,当你需要特定entity有特有的行为时(比如,玩家移动,只适用于玩家),自然也很容易表达.

[如何表现你的数据.][cb::ecs-intro-data]

[如何表现你的功能.][cb::ecs-intro-code]
