{{#include ../include/header09.md}}

# 自定义相机投影

**Note**: 这个示例是告诉你如何做到bevy官方不支持/不认可的事情,自付风险.

具有自定义投影的相机(没有使用bevy标准透视或者正交投影).

无论出于什么原因,如果你坚持不使用 [Bevy默认坐标系统][cb::coords],可以这样改变坐标系统.

这里有一个简单的正交投影实现,将-1.0映射到1.0到窗口的垂直轴，并保留窗口的水平轴的比例:

看看bevy是如何构造他的相机bundles的:
 - [2d](https://github.com/bevyengine/bevy/blob/v0.9.0/crates/bevy_core_pipeline/src/core_2d/camera_2d.rs#L46)
 - [3d](https://github.com/bevyengine/bevy/blob/v0.9.0/crates/bevy_core_pipeline/src/core_3d/camera_3d.rs#L72)

此示例基于 2D 相机的设置:

```rust,no_run,noplayground
{{#include ../code/examples/custom-projection.rs:example}}
```
