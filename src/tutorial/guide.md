{{#include ../include/header012.md}}

# 新手指引

Welcome to Bevy! :) We are glad to have you in our community!

这部分将会引导你浏览本书，帮助你获得使用bevy的全部知识。
浏览顺序是方便你学习的：从初级到高级

这只是一个帮助您导航的建议。随意跳转这本书 并阅读您感兴趣的任何内容。主目录（左侧边栏） 旨在为任何技能水平的 Bevy 用户提供参考。

---

请务必查看 [the official Bevy examples][bevy::examples]. 如果需要帮助，请使用 [GitHub Discussions][bevy::ghdiscussions], 或者欢迎加入我们的聊天室 [Discord][bevy::discord].

如果你遇到了问题，请优先查看[常见陷阱][chapter::pitfalls]章节, 大多数常见问题都有社区成员提供的详尽的解决方案。

## 基础

这些是使用 Bevy 的绝对必要条件。每个 Bevy 项目，甚至是 一个简单的，需要你熟悉这些概念。

你可以 仅使用这些知识制作一个简单的游戏开发游戏或原型。不过，随着项目的发展，您可能很快会需要了解更多信息。

 - [Bevy配置建议][chapter::setup]
   - [入门][cb::getting-started]
 - [Bevy 编程框架][chapter::programming]
   - [ECS简介][cb::ecs-intro]
   - [Entities, Components][cb::ec]
   - [Bundles][cb::bundle]
   - [Resources][cb::res]
   - [Systems][cb::system]
   - [App Builder][cb::app]
   - [Queries][cb::query]
   - [Commands][cb::commands]
 - [游戏引擎基础][chapter::fundamentals]
   - [坐标系][cb::coords]
   - [Transforms][cb::transform]
   - [Time and Timers][cb::time]
 - [常用图形特性][chapter::graphics]
   - [Cameras][cb::camera]
 - [Bevy 资源管理][chapter::assets]
   - [使用AssetServer加载资源][cb::assetserver]
   - [Handles][cb::handle]
 - [Input Handling][chapter::input]
   - [Keyboard][input::keyboard]
   - [Mouse][input::mouse]
   - [Gamepad (Controller)][input::gamepad]
   - [Touchscreen][input::touch]
 - [Window Management][chapter::window]
   - [Window Properties][cb::window]
   - [Change the Background Color][cb::clearcolor]
 - [Audio][chapter::audio]
   - [Playing Sounds][cb::audio-basic]

## 下一步

您可能需要学习其中的大部分内容才能制作出一个不简单的 Bevy 项目。在你对基础知识有信心之后，你应该学习这些。

 - [Bevy 编程框架][chapter::programming]
   - [Events][cb::event]
   - [System Order of Execution][cb::system-order]
   - [Run Conditions][cb::rc]
   - [System Sets][cb::systemset]
   - [Local Resources][cb::local]
   - [Schedules][cb::schedule]
   - [States][cb::state]
   - [Plugins][cb::plugin]
   - [Change Detection][cb::change-detection]
 - [游戏引擎基础][chapter::fundamentals]
   - [父子层级结构][cb::hierarchy]
   - [Visibility][cb::visibility]
   - [日志/控制台信息][cb::log]
 - [Input Handling][chapter::input]
   - [把光标转换为世界坐标][cookbook::cursor2world]
 - [Bevy资源管理][chapter::assets]
   - [访问资源数据][cb::asset-data]
   - [热重载资源][cb::asset-hotreload]
 - [Bevy配置建议][chapter::setup]
   - [Bevy开发工具和编辑器][cb::tools]
   - [社区插件生态][cb::3rdparty]
 - [音频][chapter::audio]:
   - [空间音效][cb::audio-spatial]

## 中级

这是一些特殊内容，你可能会需要他们，取决于你的项目

 - [Bevy 编程框架][chapter::programming]
   - [Direct World Access][cb::world]
   - [Exclusive Systems][cb::exclusive]
   - [Param Sets][cb::paramset]
   - [System Piping][cb::system-pipe]
 - [游戏引擎基础][chapter::fundamentals]
   - [Fixed Timestep][cb::fixedtimestep]
 - [General Graphics Features][chapter::graphics]
   - [HDR, Tonemapping][cb::hdr]
   - [Bloom][cb::bloom]
 - [Input Handling][chapter::input]
   - [Input Text][input::char]
   - [Drag-and-Drop files][input::dnd]
 - [Bevy资源管理][chapter::assets]
   - [React to Changes with Asset Events][cb::assetevent]
   - [Track asset loading progress][cb::asset-ready]
 - [Programming Patterns][chapter::patterns]
   - [Write tests for systems][cb::system-tests]
   - [Generic Systems][cb::system-generic]
   - [Manual Event Clearing][cb::event-manual]
 - [Window Management][chapter::window]
   - [Grab/Capture the Mouse Cursor][cookbook::mouse-grab]
   - [Set the Window Icon][cookbook::window-icon]
 - [Audio][chapter::audio]
   - [Custom Audio Streams][cb::audio-custom]

## 高级

这部内容适用面较小，如果你希望了解Bevy的内部工作原理，自己扩展bevy的功能，或者做一些高级的事情，你可以学习这部分。

 - [Bevy 编程框架][chapter::programming]
   - [Non-Send][cb::nonsend]
 - [Programming Patterns][chapter::patterns]
   - [Component Storage][cb::component-storage]
 - [Bevy配置建议][chapter::setup]
   - [Customizing Bevy (cargo crates and features)][cb::features]
   - [Using bleeding-edge Bevy (main)][cb::bevy-main]
 - [Bevy Render (GPU) Framework][chapter::gpu]
   - [Render Architecture Overview][cb::render-architecture]
   - [Render Stages][cb::render::stage]
