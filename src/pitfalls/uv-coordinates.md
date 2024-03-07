{{#include ../include/header011.md}}

#  Bevy中的UV坐标

在Bevy, 纹理/图像 像素的垂直轴,还有着色器的纹理采样方向, 指向向下,从上到下. 原点在左上.

与 [在Bevy其他地方使用World坐标系统][cb::coords]一致, Y轴指向朝上.

但是, 他与 大多数存储像素数据的图像文件格式 以及 大多数图像API工作方式(DirectX, Vulkan, Metal,
WebGPU, 除了 OpenGL)一致.

OpenGL (还有基于此的框架) 是不一样的. 如果你之前经验是这个, 你可能发现纹理被垂直翻转了.

---

如果你使用mesh, 确保有正确的UV值. 如果是其他软件创建的, 确保选择了正确设置.

如果你在写一个自定义着色器, 确保你的UV算法是对的.

## 精灵

如果2D 精灵 图像被翻转 (不论什么原因), 可以使用 Bevy 的精灵翻转特性修复:

```rust,no_run,noplayground
{{#include ../code011/src/pitfalls/uv_coordinates.rs:sprite-flipping}}
```

## Quad

如果你想在 [`Quad`][bevy::Quad] mesh 显示一个图像(或是自定义着色器), 你可以如下所示进行垂直翻转:

```rust,no_run,noplayground
{{#include ../code011/src/pitfalls/uv_coordinates.rs:quad-flipping}}
```

(这个变通方法是必要的,因为 Bevy 的[`Quad`][bevy::Quad] 原语中 `flipped` 特性仅支持水平翻转, 但我们希望垂直翻转)
