{{#include ./include/header011.md}}

# 内置资源列表

此页面是buvy所提供的所有重要内容的快查清单。

 - [SystemParams](#systemparams)
 - [Assets](#assets)
 - [文件格式](#file-formats)
 - [GLTF 资源标签](#gltf-asset-labels)
 - [Shader Imports](#shader-imports)
 - [`wgpu` Backends](#wgpu-backends)
 - [Schedules](#schedules)
 - [Run Conditions](#run-conditions)
 - [Plugins](#plugins)
 - [Bundles](#bundles)
 - [Resources (Configuration)](#configuration-resources)
 - [Resources (Engine User)](#engine-resources)
   - [Main World](#engine-resources)
   - [Render World](#render-world-resources)
   - [Low-Level `wgpu` access](#low-level-wgpu-resources)
 - [Resources (Input)](#input-handling-resources)
 - [Events (Input)](#input-events)
 - [Events (Engine)](#engine-events)
 - [Events (System/Control)](#system-and-control-events)
 - [Components](#components)

## SystemParams

这是所有可以在 [system][cb::system] 函数中使用的参数类型

{{#include ./include/builtins.md:systemparams}}

## Assets

[(关于资源的详细信息)][cb::asset]

以下是 Bevy 默认注册的资产类型.

{{#include ./include/builtins.md:assets}}

## 文件格式

这里是bevy支持的资源的文件格式。每一个都可以使用 [cargo 特性][cb::features] 启用/关闭。有的是默认开启的，有的不是。

{{#include ./include/builtins.md:file-formats}}

有一些非官方的插件可用于添加对更多文件格式的支持。

## GLTF 资源标签

[子资源的GLTF资源路径标签][cb::gltf-asset-path]

{{#include ./include/builtins.md:gltf-asset-labels}}

## Shader Imports

TODO

## `wgpu` 后端

{{#include ./include/builtins.md:wgpu-backends}}

## Schedules

{{#include ./include/builtins.md:schedules}}

{{#include ./include/builtins.md:render-sets}}

## Run Conditions

TODO

## Plugins

TODO

## Bundles

Bevy 内置 [bundle][cb::bundle] 类型, 可以生成不同种类的实体

{{#include ./include/builtins.md:bundles}}

## Resources

[(resources的详细信息)][cb::res]

### Configuration Resources

这些resources允许你改变bevy设定的部分工作方式。

这些应该在启动时设置好，但也可以在运行时变更（通过[system][cb::system])）：

{{#include ./include/builtins.md:resources-config}}

在运行时不可修改的设置不会使用resources表示。它们通过相应的 [plugins](#plugins)进行配置。

### Engine Resources

这些resources提供了运行时访问不同游戏引擎特性的功能。

如果需要他们的状态或者控制各自bevy部分，请通过你提供的 [systems][cb::system]访问他们。这些 resources 位于 [Main World][cb::render-architecture]. [See here for the
resources in the Render World](#render-world).

{{#include ./include/builtins.md:resources-main}}

#### Render World Resources

这些 resources 位于 [Render World][cb::render-architecture]. 
通过 rendering systems 访问(运行于 [render stages][cb::render::stage]).

{{#include ./include/builtins.md:resources-render}}

还有很多在Render World中未提及的resources，要么是因为它们是 Bevy 内部的渲染算法，
要么因为它们只是 Main World 中等效资源提取的副本 。


#### 低阶 `wgpu` Resources

通过这些resources, 你可以直接访问`wgpu` 的API控制GPU.
这适用于Main World 和 Render World.

{{#include ./include/builtins.md:resources-wgpu}}

### Input Handling Resources

这些resources表示不同输入设备的当前状态. 从你的
[systems][cb::system]中读取 并 [处理用户输入][chapter::input].

{{#include ./include/builtins.md:resources-input}}

## Events

[(events详细信息)][cb::event]

### Input Events

这部分[events][cb::event] 由输入设备触发，读取他们并[处理用户输入][cb::input].

{{#include ./include/builtins.md:events-input}}

### Engine Events

 Bevy应用正常运行期间发生的内部相关[Events][cb::event]。

{{#include ./include/builtins.md:events-engine}}

### System and Control Events

来自操作系统/窗口系统 或用于控制bevy的Events

{{#include ./include/builtins.md:events-system}}

## Components

完整的各component的类型列表由于太细碎无法在此列出

请看: [(List in API Docs)][bevy::impl::Component]

