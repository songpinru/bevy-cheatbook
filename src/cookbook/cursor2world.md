{{#include ../include/header012.md}}

# 把光标转换为世界坐标

## 2D 游戏

如果你只有一个窗口(主窗口),适用于大多数游戏,你可以这样:

<details>
  <summary>
  <code>Code (simple version):</code>
  </summary>

```rust,no_run,noplayground
{{#include ../code012/src/cookbook/cursor2world.rs:simple}}
```

```rust,no_run,noplayground
{{#include ../code012/src/cookbook/cursor2world.rs:simple-app}}
```

</details>

如果你有一个多窗口的复杂应用,这里有一个更加复杂版本的示例代码:

<details>
  <summary>
  <code>Code (multi-window version):</code>
  </summary>

```rust,no_run,noplayground
{{#include ../code012/src/cookbook/cursor2world.rs:multiple-windows}}
```

```rust,no_run,noplayground
{{#include ../code012/src/cookbook/cursor2world.rs:multiple-windows-app}}
```

</details>

## 3D 游戏

如果您希望能够检测光标指向的 3D 对象，这里有各好用的插件(非官方):
[`bevy_mod_picking`][project::bevy_mod_picking].

对于具有平坦地平面的简单自上而下的相机视图游戏，只计算光标下方地面的坐标就足够了。

<button class="button_wasm_cbexample" id="button_cursor_3d_ground_plane">Load Interactive Example</button>

在交互式示例中，有一个具有非默认位置和旋转的地平面。
有一个红色的立方体(使用全局坐标定位)和一个蓝色的立方体(使用本地坐标定位) ，
后者是地平面的[child entity][cb::hierarchy]。它们都应该跟着光标走。


<details>
  <summary>
  <code>Code and explanation:</code>
  </summary>

```rust,no_run,noplayground
{{#include ../code012/src/cookbook/cursor2world.rs:3d-ground-plane}}
```

```rust,no_run,noplayground
{{#include ../code012/src/cookbook/cursor2world.rs:3d-ground-plane-app}}
```

If the ground is tilted/rotated or moved, the global and local coordinates
will differ, and may be useful for different use cases, so we compute both.

For some examples:
 - if you want to spawn a [child][cb::hierarchy] entity, or to quantize
   the coordinates to a grid (for a tile-based game, to detect the grid tile under the cursor),
   the local coordinates will be more useful
 - if you want to spawn some overlays, particle effects, other independent game entities,
   at the position of the cursor, the global coordinates will be more useful

</details>

<script type="module" src="/loadwasm.js"/>
