{{#include ../include/header012.md}}

# 配置 Bevy

Bevy 是非常模块化和可配置化的.它被实现为由许多独立的 cargo crates 组成，允许你删除你不需要的部分。
较高级别的功能建立在较底层的基础库之上，并且可以禁用或进行替换。

底层的核心库（如 Bevy ECS）也可以完全独立使用，或者集成到其他非 Bevy 项目中。


## Bevy Cargo Features

在 Bevy 项目中，你可以使用 cargo 的 feature 特性启用或禁用 Bevy 的各个功能。

这里我将解释其中的一部分，以及为什么你可能想要自己设置这些 feature。

许多常见的功能在默认情况下是启用的。如果你想禁用其中一些，
请注意，不幸的是，Cargo 不允许你禁用个别的默认功能，所以你需要先禁用全部默认的 Bevy 功能后，再重新启用你想要的功能。

下面是一个你可能会用到的 Bevy 配置：

```toml
[dependencies.bevy]
version = "0.12"
# Disable the default features if there are any that you do not want
default-features = false
features = [
  # These are the default features:
  # (re-enable whichever you like)

  # Bevy functionality:
  "multi-threaded",     # Run with multithreading
  "bevy_asset",         # Assets management
  "bevy_audio",         # Builtin audio
  "bevy_gilrs",         # Gamepad input support
  "bevy_scene",         # Scenes management
  "bevy_winit",         # Window management (cross-platform Winit backend)
  "bevy_render",        # Rendering framework core
  "bevy_core_pipeline", # Common rendering abstractions
  "bevy_gizmos",        # Support drawing debug lines and shapes
  "bevy_sprite",        # 2D (sprites) rendering
  "bevy_pbr",           # 3D (physically-based) rendering
  "bevy_gltf",          # GLTF 3D assets format support
  "bevy_text",          # Text/font rendering
  "bevy_ui",            # UI toolkit
  "animation",          # Animation support
  "tonemapping_luts",   # Support different camera Tonemapping modes (enables KTX2+zstd)
  "default_font",       # Embed a minimal default font for text/UI

  # File formats:
  "png",    # PNG image format for simple 2D images
  "hdr",    # HDR images
  "ktx2",   # Preferred format for GPU textures
  "zstd",   # ZSTD compression support in KTX2 files
  "vorbis", # Audio: OGG Vorbis

  # Platform-specific:
  "x11",                   # Linux: Support X11 windowing system
  "android_shared_stdcxx", # Android: use shared C++ library
  "webgl2",                # Web: use WebGL2 instead of WebGPU

  # These are other (non-default) features that may be of interest:
  # (add any of these that you need)

  # Bevy functionality:
  "asset_processor",      # Asset processing
  "file_watcher",         # Asset hot-reloading
  "subpixel_glyph_atlas", # Subpixel antialiasing for text/fonts
  "serialize",            # Support for `serde` Serialize/Deserialize
  "async-io",             # Make bevy use `async-io` instead of `futures-lite`
  "pbr_transmission_textures", # Enable Transmission textures in PBR materials
                               # (may cause issues on old/lowend GPUs)

  # File formats:
  "dds",  # Alternative DirectX format for GPU textures, instead of KTX2
  "jpeg", # JPEG lossy format for 2D photos
  "webp", # WebP image format
  "bmp",  # Uncompressed BMP image format
  "tga",  # Truevision Targa image format
  "exr",  # OpenEXR advanced image format
  "pnm",  # PNM (pam, pbm, pgm, ppm) image format
  "basis-universal", # Basis Universal GPU texture compression format
  "zlib", # zlib compression support in KTX2 files
  "flac", # Audio: FLAC lossless format
  "mp3",  # Audio: MP3 format (not recommended)
  "wav",  # Audio: Uncompressed WAV
  "symphonia-all", # All Audio formats supported by the Symphonia library
  "shader_format_glsl", # GLSL shader support
  "shader_format_spirv", # SPIR-V shader support

  # Platform-specific:
  "wayland",              # (Linux) Support Wayland windowing system
  "accesskit_unix",       # (Unix-like) AccessKit integration for UI Accessibility
  "bevy_dynamic_plugin",  # (Desktop) support for loading of `DynamicPlugin`s

  # Development/Debug features:
  "dynamic_linking",   # Dynamic linking for faster compile-times
  "trace",             # Enable tracing for performance measurement
  "detailed_trace",    # Make traces more verbose
  "trace_tracy",       # Tracing using `tracy`
  "trace_tracy_memory", # + memory profiling
  "trace_chrome",      # Tracing using the Chrome format
  "wgpu_trace",        # WGPU/rendering tracing
  "debug_glam_assert", # Assertions to validate math (glam) usage
  "embedded_watcher",  # Hot-reloading for Bevy's internal/builtin assets
]
```

( [here][bevy::features] 有bevy 可通过 cargo 配置的功能清单.)

### 图形 / 渲染

对于一个图形化的应用程序或游戏（大多数 Bevy 项目），你需要 `bevy_winit` 和你选择的渲染特性.
对于[Linux][platform::linux] 支持, 你至少需要 `x11` 或 `wayland`中的一个.

所有使用Bevy渲染的应用,都要有 `bevy_render` 和 `bevy_core_pipeline`.

如果你只需要2D不需要3D,添加`bevy_sprite`.

如果你只需要3D不需要2D,添加 `bevy_pbr`. 如果你 [通过GLTG文件加载 3D 模型][cb::gltf], 添加 `bevy_gltf`.

如果你使用Bevy UI,你需要`bevy_text` 和 `bevy_ui`. 
`default_font`内置了一个简单的字体文件,这对原型设计很有用,因此你的项目中不必有字体资源.
在实际的项目中,为了你的游戏美术风格看起来不错,你或许想要使用自己的字体.
这时你可以禁用`default_font`特性.

如果你想在屏幕上绘制调试线条和形状,添加`bevy_gizmos`.

如果你不需要任何图形(例如例如专用游戏服务器，科学模拟器等),您可以删除所有这些功能.

### 文件格式

你可以使用cargo来启用/禁用 各种不同文件格式加载资源 的支持

 [here][builtins::file-formats] 有详细信息.

### 输入设备

如果你不关心 [手柄(控制器/遥感)][input::gamepad] 支持,你可以禁用 `bevy_gilrs`.

### 特殊平台

#### Linux Windowing Backend

在 [Linux][platform::linux], 你可以选择支持X11, Wayland,或者两个都支持.只有`x11` 是默认开启的, 因为这是传统的视窗后端系统,
它与大多数或甚至于所有的 Linux 发行版兼容这可以使你的工程构建文件更小、编译更快。你可能想额外启用 `wayland` 来完全支持现代 Linux 环境。这将为你的项目增加一些额外的依赖。

某些 Linux 发行版或平台可能会在 X11 上遇到困难，但对 Wayland支持更好。您应该同时启用两者以获得最佳兼容性。

#### WebGPU vs WebGL2

在 [Web/WASM][platform::web], 你可以在这两种渲染后端中选择.


WebGPU 是现代实验性的解决方案,提供了更好的性能和更全的特性支持,
但是浏览器支持有限(只知道最近几个nightly版本的Chrome 和 Firefox中有用)

WebGL2 与所有浏览器的兼容性最好,但是性能低下,并且你在bevy中使用的一些图形特性会受到限制.

cargo feature 启用`webgl2` 将会选择 WebGL2 , 否则使用 WebGPU.

### 开发阶段的特性

在开发你的项目时,这些特性可能会有用:

#### 资源热加载和热处理

 `file_watcher` 特性会启用 [资源热加载][cb::asset-hotreload]支持, 桌面平台上可用.

`asset_processor` 特性会启用 [资源处理][cb::asset-processor], 允许你在开发时自动转换和调优资源.

#### 动态链接

`dynamic_linking` 会使 Bevy 作为一个共享的动态库被构建和链接.,这将加快重构建的速度.

这只支持桌面平台.已知在Linux上运行的不错.Windows 和 macOS 也支持,但是测试比较少,曾经还遇到过问题.

建议不要在推送给别人的release版本中启用此功能 ,除非你有特别的原因并且你知道你在做什么.
它引入了不必要的复杂性（你需要捆绑额外的文件），并有可能导致程序不能正常运行。尽可能只在开发过程中使用这个功能。

基于以上分析，可把这个功能作为  `cargo` 的一个命令行选项来指定，而不是把它放在`Cargo.toml` 中，可能会更方便。
只需像下面这样运行你的项目：

```sh
cargo run --features bevy/dynamic_linking
```

可以添加这个到你的 [IDE/编辑器 配置][cb::ide].

#### Tracing

`trace` 和 `wgpu_trace` 特性在分析和诊断性能问题时可能会很有用.

`trace_chrome` 和 `trace_tracy` 特性用于选择你想用来可视化跟踪的后端.

更多请参考 [Bevy's official docs on profiling][bevy::profiling] .
