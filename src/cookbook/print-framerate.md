{{#include ../include/header012.md}}

# 显示帧率

您可以使用 Bevy 的内置诊断器来测量帧速率 （FPS），以便监控性能。

给你的[app][cb::app]添加Bevy诊断插件来开启它。

```rust,no_run,noplayground
{{#include ../code012/src/cookbook/print_framerate.rs:setup}}
```

## 打印到控制台/日志

最简单的使用方式就是打印诊断信息到控制台([log][cb::log])。如果你只希望在dev阶段生效，你可以添加一个条件编译属性。

```rust,no_run,noplayground
{{#include ../code012/src/cookbook/print_framerate.rs:log}}
```

## 游戏内 / 屏幕 FPS 显示

你可以使用Bevy UI创建一个游戏内FPS 计数器。

推荐你创建一个新的具有据对位置的root UI（没有父节点的实体），这样你可以控制精确的显示位置，并且不会影响你的其他UI。

这里是如何弄一个好看的FPS计数器的示例代码：

<details>
  <summary>
  <code>Code Example (Long):</code>
  </summary>

```rust,no_run,noplayground
{{#include ../code012/src/cookbook/print_framerate.rs:ui}}
```

```rust,no_run,noplayground
{{#include ../code012/src/cookbook/print_framerate.rs:ui-app}}
```

</details>
