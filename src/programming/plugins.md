{{#include ../include/header09.md}}

# Plugins

官方示例:
[`plugin`][example::plugin],
[`plugin_group`][example::plugin_group].

---

随着你的项目发展, 模块化是非常有用的. 你可以把他们分成 "plugin".

Plugin是可以加入[App构造器][cb::app] 中的东西的集合.这样可以从多个地方给app添加东西,比如不同的Rust 文件/模块 或者 crate.

```rust,no_run,noplayground
{{#include ../code/src/basics.rs:plugins}}
```

注意使用`&mut`来访问[`App`][bevy::App],这样你可以做任何事情,就像`fn main`一样.

对于你自己的项目结构,plugin的主要价值是你不必把所有Rust类型和函数声明成`pub`, 只是为了被`fn main`访问并加入到app构造器.
Plugin让你可以从多个地方给app添加东西,比如不同的Rust 文件/模块.

你可以决定plugin如何融入你的游戏.

一些建议:
 - 为不同的 [states][cb::state] 创建plugin.
 - 为不同的子系统创建plugin, 比如 物理系统或者输入处理. 

## Plugin 组

plugin组可以一次性注册多个plugin. Bevy的 [`DefaultPlugins`][bevy::DefaultPlugins]和[`MinimalPlugins`][bevy::MinimalPlugins]就是例子.
这样创建你自己的plugin组:

```rust,no_run,noplayground
{{#include ../code/src/basics.rs:plugin-groups}}
```

给[app][cb::app]插入plugin时,你可以禁用一部分插件.

例如,如果你想手动设置日志(使用你自己的`tracing`订阅者), 你可以禁用bevy的[`LogPlugin`][bevy::LogPlugin]:

```rust,no_run,noplayground
{{#include ../code/src/basics.rs:plugin-groups-disable}}
```
请注意，这只是简单地禁用了功能，但它不是真正删除代码以避免应用程序的二进制文件的膨胀。被禁用的插件仍然会被编译进你的程序中。

如果你想给你的编译产物瘦身，你应该禁用 Bevy 的默认[cargo features][cb::features]，或者单独使用 Bevy 的各种子库。

## Plugin 配置

Plugin也是个 存储初始化/启动 期间 设置/配置 的好方法. 对于那些运行时可能变化的设置,推荐放进[resource][cb::res].

```rust,no_run,noplayground
{{#include ../code/src/basics.rs:plugin-config}}
```
使用[Plugin 组][cb::plugingroup]添加的plugin也可以被配置.bevy [`DefaultPlugins`][bevy::DefaultPlugins]中很多就是这样工作的.
Plugins that are added using [Plugin Groups][cb::plugingroup] can also be
configured. Many of Bevy's [`DefaultPlugins`][bevy::DefaultPlugins] work
this way.

```rust,no_run,noplayground
{{#include ../code/src/basics.rs:defaultplugins-config}}
```

## 发布 Crate

Plugin给你了一个很好的途径来发布基于bevy的库,让其他人可以轻松集成进他们的项目.

如果你想把plugin发布成公共crate,你需要了解[插件作者指南][bevy::plugins_guidelines].

别忘了在官网上提交进入 [Bevy Assets][bevyassets]的申请, 这样大家可以更容易地找到你的插件.
在[Github 仓库][project::bevyassets]提交一个PR就行.

如果你有兴趣支持前沿Bevy(main), [看这里][cb::git-plugins].
