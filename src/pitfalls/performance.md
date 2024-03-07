{{#include ../include/header-none.md}}

# 性能

## 未经优化的调试编译

你可以在 debug 和 dev 模式下启用编译器的优化功能!

你可以对依赖启用更高的优化(包括Bevy),但不要包括你的代码,以保证编译速度!

在 `Cargo.toml` 或 `.cargo/config.toml`:

```toml
# Enable max optimizations for dependencies, but not for our code:
[profile.dev.package."*"]
opt-level = 3
```

这足以使Bevy快速运行.只会在清理了构建后慢一点,不会影响重新编译时间.

如果你自己的代码包含CPU密集部分,你可能还需要启用针对CPU的优化.
但是,这样可能会极大的影响某些项目的编译速度(相当于完整的发布构建),因此通常不建议这么做.

```toml
# Enable only a small amount of optimization in debug mode
[profile.dev]
opt-level = 1
```

**警告!** 如果你使用调试器(比如 `gdb` 或 `lldb`) 逐步调试你的代码, 所有的优化都可能扰乱调试.
断点可能会被跳过,代码可能会跳到意想不到的地方.如果你想逐步调试你的代码,你可能需要设置`opt-level = 0`.

### 为什么这是必要的?

没有编译优化的Rust是非常慢的. 特别是使用Bevy时,默认的cargo构建设置会导致糟糕的运行时性能. 资源加载会很慢, FPS也会很低.

常见现象:
  - 从GLTF文件加载有大量纹理的高分辨率3D模型时,可能会用好几分钟!
    这可能会误导你认为自己的代码没有起作用,因为在加载完成前你不会在屏幕上看到任何东西.
  - 生成一些2D精灵或者3D模型后,帧率下降到没法玩的程度.

### 为什么不用 `--release`?

你可能听到过这个建议: 只用`--release` 运行! 然而这是个坏主意. 别这么做.

Release模式禁用 "调试断言" : 开发阶段的额外检查. 很多库在这个模式下还有其他东西. 
在Bevy和WGPU中就有对着色器和GPU API用法的校验.Release模式禁用了这些检查,会导致少信息的崩溃, 热重载出问题, 或者没发现潜在的 错误/无效 逻辑.

Release模式也导致增量编译变慢. 这降低了Bevy的编译速度,在开发中很烦人.

---

根据本页顶部的建议, 你不需要使用`--release`构建, 只是为了用充足的性能测试你的游戏的话.
你只需要在要发布给你的用户时再release构建就行.

如果你想,也可以为release构建启用LTO (Link-Time-Optimization) ,用编译速度换取更高的性能:

```toml
[profile.release]
lto = "thin"
```
