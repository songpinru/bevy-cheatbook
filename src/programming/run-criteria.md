{{#include ../include/header09.md}}

# 运行条件

运行条件是Bevy在运行时运行特定[systems][cb::system]的一种控制机制.这样你可以在确定条件下才运行你的功能.

运行条件可以被用于单个[system][cb::system], [system sets][cb::systemset], 还有 [stages][cb::stage].

运行条件就是一种返回值类型是[`enumShouldRun`][bevy::ShouldRun]的system.可以和正常system一样接收任何 [system 参数][builtins::systemparam].


这个例子显示了如何使用执行条件来实现不同的多人游戏模式:

```rust,no_run,noplayground
{{#include ../code/src/basics.rs:run-criteria}}
```

## 已知陷阱

### 组合多个运行条件

不可能让一个system取决于多个运行条件.Bevy提供了`.pipe`方法来允许你组成运行条件链,以此来修改运行条件的输出,但是实践非常有限.

考虑使用[`iyes_loopless`][project::iyes_loopless].它可以使用任意数量的运行条件控制你的system,还不会阻止你使用[states][cb::state] 或 [fixed timestep][cb::fixedtimestep].
Consider using [`iyes_loopless`][project::iyes_loopless]. It allows you to
use any number of run conditions to control your systems, and does not prevent
you from using [states][cb::state] or [fixed timestep][cb::fixedtimestep].

### Events

在不会每帧运行的system中接收[events][cb::event]时,在接收系统不执行的帧中，所有发送的事件都将会错过。

为了应对这种情况，你可以实现一个[自定义的事件清理策略][cb::event-manual]，以便手动管理相关事件类型的生命周期。
