# Summary

[简介](./introduction.md)
[章节概述](./overview.md)

---

[bevy内置资源列表](./builtins.md)

---

- [Bevy 教程](./tutorial.md)
  - [新手引导](./tutorial/guide.md)

- [Bevy 指导手册](./cookbook.md)
  - [显示帧率](./cookbook/print-framerate.md)
  - [将光标转换为世界坐标](./cookbook/cursor2world.md)
  - [自定义相机投影](./cookbook/custom-projection.md)
  - [3D 平移+动态观察摄影机](./cookbook/pan-orbit-camera.md)
  - [Resource列表](./cookbook/print-resources.md)

---

- [Bevy 初始化](./setup.md)
  - [入门](./setup/getting-started.md)
  - [文本编辑器 / IDE](./setup/editor.md)
    - [Visual Studio Code](./setup/editor/vscode.md)
    - [JetBrains (RustRover, IntelliJ, CLion)](./setup/editor/jetbrains.md)
    - [Kakoune](./setup/editor/kak.md)
    - [Vim](./setup/editor/vim.md)
    - [Emacs](./setup/editor/emacs.md)
  - [自定义 Bevy (功能, 模块)](./setup/bevy-config.md)
  - [社区插件生态](./setup/unofficial-plugins.md)
  - [Bevy开发工具与编辑器](./setup/bevy-tools.md)
  - [性能调优](./setup/perf.md)
  - [使用最新的Bevy (main)](./setup/bevy-git.md)

- [常见陷阱](./pitfalls.md)
  - [奇怪的编译错误](./pitfalls/build-errors.md)
  - [性能问题](./pitfalls/performance.md)
  - [添加system函数遇到问题](./pitfalls/into-system.md)
  - [3D 对象不显示](./pitfalls/3d-not-rendering.md)
  - [从struct借用多个字段](./pitfalls/split-borrows.md)
  - [抖动 (断断续续的移动/动画)](./pitfalls/time.md)
  - [文字/图像 被翻转](./pitfalls/uv-coordinates.md)

- [游戏引擎基础](./fundamentals.md)
  - [坐标系](./fundamentals/coords.md)
  - [转换](./fundamentals/transforms.md)
  - [可见性](./fundamentals/visibility.md)
  - [时间和定时器](./fundamentals/time.md)
  - [日志，控制台消息](./fundamentals/log.md)
  - [父子层次结构](./fundamentals/hierarchy.md)
  - [固定时间步长](./fundamentals/fixed-timestep.md)

- [一般图形特性](./graphics.md)
  - [相机](./graphics/camera.md)
  - [HDR 和色调映射](./graphics/hdr-tonemap.md)
  - [Bloom](./graphics/bloom.md)

- [使用2D](./2d.md)
  - [2D 相机设置](./2d/camera.md)
  - [Sprites and Atlases](./2d/sprites.md)

- [使用3D](./3d.md)
  - [3D 相机设置](./3d/camera.md)
  - [3D 模型和场景 (GLTF)](./3d/gltf.md)

- [输入处理](./input.md)
  - [键盘](./input/keyboard.md)
  - [鼠标](./input/mouse.md)
  - [文本 / 字符](./input/char.md)
  - [手柄 (控制器, 操纵杆)](./input/gamepad.md)
  - [触摸屏](./input/touch.md)
  - [拖放 (文件)](./input/dnd.md)

- [窗口管理](./window.md)
  - [窗口属性](./window/props.md)
  - [更改背景颜色](./window/clear-color.md)
  - [抓取/捕获鼠标光标](./window/mouse-grab.md)
  - [设置窗口图标](./window/icon.md)

- [资源管理](./assets.md)
  - [Handles](./assets/handles.md)
  - [从文件加载资源](./assets/assetserver.md)
  - [访问资源数据](./assets/data.md)
  - [响应资源变化事件](./assets/assetevent.md)
  - [追踪加载过程](./assets/ready.md)
  - [热重载资源](./assets/hot-reload.md)
  - [处理资源](./assets/processing.md)

- [音频](./audio.md)
  - [播放声音](./audio/basic.md)
  - [空间音效](./audio/spatial.md)
  - [自定义音频流](./audio/custom.md)

- [Bevy UI 框架](./ui.md)

- [Bevy核心编程框架](./programming.md)
  - [ESC介绍](./programming/ecs-intro.md)
  - [介绍: 数据](./programming/intro-data.md)
  - [介绍: 代码](./programming/intro-code.md)
  - [App](./programming/app-builder.md)
  - [Systems](./programming/systems.md)
  - [Resources](./programming/res.md)
  - [Entities, Components](./programming/ec.md)
  - [Bundles](./programming/bundle.md)
  - [Queries](./programming/queries.md)
  - [Commands](./programming/commands.md)
  - [Events](./programming/events.md)
  - [本地资源](./programming/local.md)
  - [独占system](./programming/exclusive.md)
  - [直接访问ESC资源](./programming/world.md)
  - [Schedules](./programming/schedules.md)
  - [System执行顺序](./programming/system-order.md)
  - [运行条件](./programming/run-criteria.md)
  - [System Sets](./programming/system-sets.md)
  - [States](./programming/states.md)
  - [Plugins](./programming/plugins.md)
  - [变化检测](./programming/change-detection.md)
  - [System链](./programming/system-piping.md)
  - [ParamSet](./programming/paramset.md)
  - [Non-Send](./programming/non-send.md)

- [Bevy 渲染 (GPU) 框架](./gpu.md)
  - [渲染架构概述](./gpu/intro.md)
  - [渲染阶段](./gpu/stages.md)

- [编程模式](./patterns.md)
  - [泛型系统](./patterns/generic-systems.md)
  - [组件存储 (表/稀疏集合)](./patterns/component-storage.md)
  - [手动清除事件](./patterns/manual-event-clear.md)
  - [编写测试](./patterns/system-tests.md)

- [各平台支持](./platforms.md)
  - [Linux 桌面](./platforms/linux.md)
  - [macOS 桌面](./platforms/macos.md)
  - [Windows 桌面](./platforms/windows.md)
    - [使用 WSL2](./platforms/windows/wsl2.md)
  - [浏览器 (WebAssembly)](./platforms/wasm.md)
    - [优化体积](./platforms/wasm/size-opt.md)
    - [创建自定义网页](./platforms/wasm/webpage.md)
    - [托管到GitHub Pages](./platforms/wasm/gh-pages.md)
  - [交叉编译](./setup/cross.md)
    - [在Linux 编译 Windows](./setup/cross/linux-windows.md)
    - [在 macOS 编译 Windows](./setup/cross/macos-windows.md)

---

[鸣谢](./credits.md)

---

[联系作者](./contact.md)

---

[贡献 Bevy](./contributing-bevy.md)
[贡献这本书](./contributing.md)
