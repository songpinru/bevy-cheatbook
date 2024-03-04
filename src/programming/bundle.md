{{#include ../include/header012.md}}

官方示例:
[`ecs_guide`][example::ecs_guide].

---

# Bundles

Bundle 就像创建entity时的"模板"或者 "蓝图".使创建具有相同[components][cb::component]的[entities][cb::entity]变得容易。

使用Bundle,而不是一个一个的添加component,你可以确保你的entity不会遗失某些重要的component.
如果struct的字段不完整Rust编译器会报错,帮助你确保你的代码正确.

Bevy提供了许多[内置bundle][builtins::bundle] ,可以用来生成常见的entity.

如何创建 bundle:

```rust,no_run,noplayground
{{#include ../code012/src/programming/bundle.rs:bundle}}
```

然后你可以在生成entity是使用bundle:

```rust,no_run,noplayground
{{#include ../code012/src/programming/bundle.rs:bundle-spawn}}
```

如果你想有默认值 :

```rust,no_run,noplayground
{{#include ../code012/src/programming/bundle.rs:bundle-default}}
```

然后:

```rust,no_run,noplayground
{{#include ../code012/src/programming/bundle.rs:bundle-spawn-default}}
```

## 松散的component变成bundle

技术上讲,Bevy可以把component元组视为bundle:

```
(ComponentA, ComponentB, ComponentC)
```

这允许你方便的使用多个component(或bundle)生成一个entity,或者在生成entity时添加更多任意component.
但是,这样会失去编译时的正确性优势,相比于`struct`.

```rust,no_run,noplayground
{{#include ../code012/src/programming/bundle.rs:bundle-spawn-loose}}
```

你应该更倾向于创建合适的`struct`,尤其是生成很多相似的entity时.这会是你的代码更容易维护.

## Querying

注意:你不能为一个bundle使用[query][cb::query].bundle只是为了方便创建entity. [system][cb::system]中query需要具体的component类型.

*错误示范*:

```rust,no_run,noplayground
fn my_system(query: Query<&SpriteBundle>) {
  // ...
}
```

正确示范:

```rust,no_run,noplayground
fn my_system(query: Query<(&Transform, &Handle<Image>)>) {
  // ...
}
```

(或者任何你所需的特定component)

