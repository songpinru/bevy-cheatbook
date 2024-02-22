{{#include ../include/header09.md}}

# 平移+动态观察摄影机

这是与社区贡献的代码.

当前版本由 [**@mirenbharta**][cb::1]维护.
由 [**@skairunner**][cb::cookbook::2] 完成的初始版本.

---

这与3D编辑软件Blender中的摄像机控制器类似。

使用鼠标右键进行旋转，中键进行平移，滚动滚轮进行向内/向外移动。

```rust,no_run,noplayground
{{#include ../code/examples/pan-orbit-camera.rs:example}}
```
