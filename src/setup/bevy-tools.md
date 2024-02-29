{{#include ../include/header011.md}}

# Bevy开发工具和编辑器

Becy目前还没有官方的百年机器或者类似工具.推出一个官方的编辑器是长远的计划,在此之前,有一些社区提供的工具或许可以帮助到你

---

## 编辑器

[`bevy_inspector_egui`][project::bevy_inspector_egui] 在游戏中给你提供一个类似编辑器的属性检查窗.
让你可以在游戏中实时修改你的components 和 resources的值.

[`bevy_editor_pls`][project::bevy_editor_pls] 是一个可以内嵌到游戏中的类编辑器接口.
它有许多功能,比如切换app状态,飞行相机,性能诊断,还有检查器面板.

[`space_editor`][project::space_editor] 另一个可以内嵌到游戏中的编辑器.
有点像是个受Unity启发的预制工作流.

你也可以把 [Blender][project::blender] 用作关卡/场景编辑器,把你爹场景导出为[GLTF][cb::gltf].
[Blender Bevy Components Workflow][project::blender_bevy_components_workflow] 这个项目提供了这种经验,
通过在Blender配置你的Bevy ECS [Components][cb::component] ,将它们包含在导出的 GLTF 中,并在 Bevy 中使用它们.

## 分析工具

[`bevy_mod_debugdump`][project::bevy_mod_debugdump] 一个针对[App Schedules](../programming/app-builder.md) (所有已注册的
[systems](../programming/systems.md) , [依赖顺序](../programming/system-order.md)), 和 Bevy 渲染图的可视化工具.
