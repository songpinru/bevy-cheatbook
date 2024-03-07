{{#include ../include/header011.md}}

# 难懂 Rust 编译错误

在给Bevy [app][cb::app]添加[systems][cb::system]时, 你可能会遇到令人困惑的编译错误.

## 初学者常见错误

  - 用了 `commands: &mut Commands` 而不是 `mut commands: Commands`.
  - 用了 `Query<MyStuff>` 而不是 `Query<&MyStuff>` 或 `Query<&mut MyStuff>`.
  - 用了 `Query<&ComponentA, &ComponentB>` 而不是 `Query<(&ComponentA, &ComponentB)>`
    (忘记了元组)
  - 直接用了你的resource类型, 没有用 [`Res`][bevy::Res] 或 [`ResMut`][bevy::ResMut] .
  - 直接使用你的component类型, 没有放进 [`Query`][bevy::Query].
  - 在[query][cb::query] 中使用 [bundle][cb::bundle] 类型 . 你需要单独的component,
  - 在函数中使用其他类型.

注意 `Query<Entity>` 是对的, 因为 Entity ID 是特殊的; 它不是component.

## Error adding function as system

错误可能如下:

```
error[E0277]: the trait bound `for<'a, 'b, 'c> fn(...) {system}: IntoSystem<(), (), _>` is not satisfied
   --> src/main.rs:5:21
    |
5   |         .add_system(my_system)
    |          ---------- ^^^^^^^^^ the trait `IntoSystem<(), (), _>` is not implemented for fn item `for<'a, 'b, 'c> fn(...) {system}`
    |          |
    |          required by a bound introduced by this call
    |
    = help: the following other types implement trait `IntoSystemConfigs<Marker>`:
    = ...
```

The error (confusingly) points to the place in your code where you try to add the system,
but in reality, the problem is actually in the `fn` function definition!

This is caused by your function having invalid parameters. [Bevy can
only accept special types as system parameters!][builtins::systemparam]

## query格式不正确

您可能还会出现如下所示的错误:

```
error[E0277]: the trait bound `Transform: WorldQuery` is not satisfied
   --> src/main.rs:10:12
    |
10  |     query: Query<Transform>,
    |            ^^^^^^^^^^^^^^^ the trait `WorldQuery` is not implemented for `Transform`
    |
    = help: the following other types implement trait `WorldQuery`:
              &'__w mut T
              &T
              ()
              (F0, F1)
              (F0, F1, F2)
              (F0, F1, F2, F3)
              (F0, F1, F2, F3, F4)
              (F0, F1, F2, F3, F4, F5)
            and 54 others
note: required by a bound in `bevy::prelude::Query`
   --> ~/.cargo/registry/src/index.crates.io-6f17d22bba15001f/bevy_ecs-0.10.0/src/system/query.rs:276:37
    |
276 | pub struct Query<'world, 'state, Q: WorldQuery, F: ReadOnlyWorldQuery = ()> {
    |                                     ^^^^^^^^^^ required by this bound in `Query`
```

为了访问你的component, 你需要使用引用语法 (`&` 或 `&mut`).

```
error[E0107]: struct takes at most 2 generic arguments but 3 generic arguments were supplied
   --> src/main.rs:10:12
    |
10  |     query: Query<&Transform, &Camera, &GlobalTransform>,
    |            ^^^^^                      ---------------- help: remove this generic argument
    |            |
    |            expected at most 2 generic arguments
    |
note: struct defined here, with at most 2 generic parameters: `Q`, `F`
   --> ~/.cargo/registry/src/index.crates.io-6f17d22bba15001f/bevy_ecs-0.10.0/src/system/query.rs:276:12
    |
276 | pub struct Query<'world, 'state, Q: WorldQuery, F: ReadOnlyWorldQuery = ()> {
    |            ^^^^^                 -              --------------------------
```

当你想对多个component使用query,你需要把他们放进元组:
`(&Transform, &Camera, &GlobalTransform)`.
