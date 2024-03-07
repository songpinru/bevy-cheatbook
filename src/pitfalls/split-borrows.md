{{#include ../include/header-none.md}}

# 从struct借用多个字段

在使用 [component][cb::component] 或 [resource][cb::res] 时, 有时你想同时借用多个字段的struct中的几个字段, 可能是可变借用. 

```rust,no_run,noplayground
struct MyThing {
    a: Foo,
    b: Bar,
}

fn my_system(mut q: Query<&mut MyThing>) {
    for thing in q.iter_mut() {
        helper_func(&thing.a, &mut thing.b); // ERROR!
    }
}

fn helper_func(foo: &Foo, bar: &mut Bar) {
    // do something
}
```

这可能会导致编译器出现有关冲突借用的错误:

```
error[E0502]: cannot borrow `thing` as mutable because it is also borrowed as immutable
    |
    |         helper_func(&thing.a, &mut thing.b); // ERROR!
    |         -----------  -----         ^^^^^ mutable borrow occurs here
    |         |            |
    |         |            immutable borrow occurs here
    |         immutable borrow later used by call
```

解决方案是 "重借用" 语法, 这是 Rust 编程中一个常见但不明显的技巧:

```rust,no_run,noplayground
// add this at the start of the for loop, before using `thing`:
let thing = &mut *thing;

// or, alternatively, Bevy provides a method, which does the same:
let thing = thing.into_inner();
```

注意,这行代码触发了[变动检测][cb::change-detection]. 即使后面你没有修改数据,component也会被标记成变动.

## 解释

Bevy通常可以通过特殊包装类型访问数据(比如[`Res<T>`][bevy::Res], [`ResMut<T>`][bevy::ResMut], 和 [`Mut<T>`][bevy::Mut]
(使用[query][cb::query]查询可变 component )). 这使得Bevy能追踪数据.

这些是 "智能指针" 类型,使用Rust [`Deref`][std::Deref] trait解引用你的数据. 他们无缝工作, 你甚至注意不到.

但是，从某种意义上说，它们对编译器是不透明的。如果你可以直接访问struct, Rust 语言 允许单独借用结构体的字段, 但当它包装在另一种类型中时, 这不起作用。

"重借用" 这个技巧, 把包装类有效转换为常规Rust引用. `*thing` 通过[`DerefMut`][std::DerefMut]解引用包装类, 然后`&mut`可以可变借用.
这样`&mut MyStuff` 就替代了 `Mut<MyStuff>`.
