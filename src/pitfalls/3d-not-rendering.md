{{#include ../include/header011.md}}

# 3D 对象不展示

这页列出了一些可能常见的问题,如果你生成3D对象,但是屏幕上没有显示.

## 忘记给父实体设置可见型组件

如果你的实体有层次结构,所有的父实体都需要[可见性][cb::visibility] component. 即使父实体不需要渲染东西.

通过插入 [`VisibilityBundle`][bevy::VisibilityBundle] 来修复:

```rust
{{#include ../code011/src/pitfalls/d3_not_rendering.rs:insert-visibilitybundle}}
```

更好的做法是确保首先生成正确的父实体. 如果你没有用包含可见性component的bundle,你可以用[`VisibilityBundle`][bevy::VisibilityBundle] 或

## 离相机太远

如果某些东西超过相机一定距离,会被剔除(未渲染). 默认值是`1000.0`单位. 

可以通过[`PerspectiveProjection`][bevy::PerspectiveProjection] 中的 `far`字段来控制:

```rust
{{#include ../code011/src/pitfalls/d3_not_rendering.rs:perspective-far}}
```

## 缺失vertex属性

确保[`Mesh`][bevy::Mesh]包含所有 着色器/材质 所需vertex属性.

Bevy 的默认 PBR [`StandardMaterial`][bevy::StandardMaterial] 要求所有mesh有:
 - 位置
 - 法线

其他材质可能需要:
 - UV (如果材质需要纹理)
 - 切线 (仅当使用法线贴图时，否则不需要)

如果你要生成自己的mesh数据, 确保提供所有需要的属性.

如果从资源文件加载mesh, 确保他们包含所需属性(检查你的导出设置).

如果需要法线贴图的切线，建议包含属性在您的 GLTF 文件中。这避免了 Bevy 必须在运行时自动生成它们。 
默认情况下，许多 3D 编辑器（如 Blender）不启用此选项。

## GLTF 资源使用不当

请参阅 [GLTF page][cb::gltf] 学习如何正确在Bevy使用GLTF.

GLTF文件很复杂. 包含许多子资源, 用不同的Rust类型表示. 确认你用对了.

确认你在生成一个GLTF Scene,或者使用了正确的 [`Mesh`][bevy::Mesh] 和 [`StandardMaterial`][bevy::StandardMaterial] , 以及相关GLTF Primitive.

如果你使用的是资源路径, 确认你包括了子资源的标签:

```rust,no_run,noplayground
{{#include ../code011/src/pitfalls/d3_not_rendering.rs:gltf-ass-label}}
```

如果你在生成高级 [`Gltf`][bevy::Gltf] [master asset][cb::gltf-master], 它将不起作用.

如果你生成 GLTF Mesh, 它将不起作用.

## 不支持 GLTF

{{#include ../include/gltf-limitations.md}}

## Vertex Order and Culling

默认情况下，Bevy 渲染器采用逆时针顶点顺序，并启用背面剔除。

如果您从代码生成[`Mesh`][bevy::Mesh]，请确保顶点的顺序正确。

## 未优化 / Debug 构建

你的资源可能很长时间才加载? 没有编译优化的Bevy运行很慢. 
拥有大量纹理的复杂GLTF文件可能会花费几分钟才能加载完显示在屏幕上.
优化后可能是即时完成. [看这里][pitfall::perf].
