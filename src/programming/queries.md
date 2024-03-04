{{#include ../include/header09.md}}

# Queries

官方示例:
[`ecs_guide`][example::ecs_guide].

---

Query 用于访问 [entity中的component][cb::ecs-intro].

使用 [`Query`][bevy::Query]作为 [system 参数][cb::system]时, 你可以指定你想要访问的数据类型,
也可以指定[filters][cb::query-filter]来过滤entity.

把`Query`的类型当成选择entity的 "规范".Query会匹配ECS World中符合你规范的entity.
然后你可以从每个entity访问相关的数据(使用[`Entity`][bevy::Entity] ID),或者遍历所有查出来的entity.

第一个泛型参数就是你要访问的数据.`&`表示共享/只读的数据,`&mut`表示独占/可变数据.
可能查不到component时使用 `Option`(找到的entity中可能没有这个component).如果想要多个component,放到元组里.

```rust,no_run,noplayground
{{#include ../code/src/basics.rs:sys-simple-query}}
```

上面的示例中遍历了所有可以找到的entity.

仅访问 [components][cb::component] 从特定的 [entity][cb::entity]:

```rust,no_run,noplayground
{{#include ../code/src/basics.rs:query-get}}
```

如果你想知道你访问entity的ID,你可以在query中指定 [`Entity`][bevy::Entity]类型.这在遍历时很有用,因为你可以标识查到的entity

```rust,no_run,noplayground
{{#include ../code/src/basics.rs:query-entity}}
```
如果你知道query只会匹配一个entity,你可以使用`single`/`single_mut`(panic on error) 或者`get_single`/`get_single_mut`(返回 [`Result`][std::Result]).
这些方法用于结果只会是一个entity的情况,但是如果不是只有一个entity就会报错.

```rust,no_run,noplayground
{{#include ../code/src/basics.rs:query-single}}
```

## Bundles

Query作用于具体的component.如果使用[bundle][cb::bundle]创建了entity,你需要使用创建bundle的具体的component类型.

新手常见的错误之一就是query时使用bundle类型!

## Query过滤器

添加Query过滤器来缩小检索entity的范围.

使用[`Query`][bevy::Query]的第二个(可选)泛型参数来过滤.

注意query的语法:第一个泛型参数是你要访问的数据(访问多个component时使用元组),第二个泛型参数是过滤(也可以是元组,有多个时).

使用[`With`][bevy::With]/[`Without`][bevy::Without]来限定entity是否有对应的component.

```rust,no_run,noplayground
{{#include ../code/src/basics.rs:sys-query-filter}}
```
如果你不关心这些component中存储的数据,而是只希望找到的实体有(或者没有)component,这会很有用.
如果你想要数据,把component放到第一个泛型参数中(如前文所述),而不是使用过滤器.

多个过滤器可以组合:
 - 使用元组 (和逻辑)
 - 使用`Or<(…)>` 包装 (或逻辑).
   - (注意里面的元组)
