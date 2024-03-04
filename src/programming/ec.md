{{#include ../include/header012.md}}

官方示例:
[`ecs_guide`][example::ecs_guide].

---

# Entity

[ECS World中如何存储数据.][cb::ecs-intro-data]

从概念上来说,一个entity由一组不同的component组成.每个component是Rust中的type(`struct` 或 `enum`),
entity也可以用type来保存值.

技术上,一个entity只是一个简单的ID(想象 表/表格 里的行数),用来寻找相关的数据的值(在表里的不同列).

在bevy, [`Entity`][bevy::Entity] 就是这个值.由两个整数组成:ID 和 "generation"(ID可以被重用,在释放旧的entity后).

你可以使用[`Commands`][cb::commands] 或者 [exclusive `World` access][cb::world] 生成和销毁entity.

```rust,no_run,noplayground
{{#include ../code012/src/programming/ec.rs:spawn-despawn}}
```

很多的entity可能都有相同的component.你可以使用[Bundles][cb::bundle]来简化你生成entity.

# Component

组件是实体关联的数据.

要创建一个新的component类型,只需定义一个 Rust `struct` 或 `enum`，并派生 [`Component`][bevy::Component] trait。

```rust,no_run,noplayground
{{#include ../code012/src/programming/ec.rs:component}}
```

component类型必须是唯一的，Rust 的每种类型中只能对应一个 component。

## Newtype Components

使用 newtype 模式来包装数据类型，从基本类型中制作出独特的component：

```rust,no_run,noplayground
{{#include ../code012/src/programming/ec.rs:component-newtype}}
```

## 标记 Component

可以使用空的struct来标记entity.这被称为 "标记component". [query 过滤][cb::query-filter]时非常有用.

```rust,no_run,noplayground
{{#include ../code012/src/programming/ec.rs:component-marker}}
```

## 访问 Components

[systems][cb::system]中访问Component , 使用 [queries][cb::query].

你可以认为query是访问数据的 "规范".他会搜索所有符合你要求的entity.

```rust,no_run,noplayground
{{#include ../code012/src/programming/ec.rs:query}}
```

## 增加/删除 Components

使用 [`Commands`][cb::commands] 或 [exclusive `World` access][cb::world] 增加/删除entity上的component.

```rust,no_run,noplayground
{{#include ../code012/src/programming/ec.rs:insert-remove}}
```
