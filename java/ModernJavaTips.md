# 现代 Java 技巧和代码风格指南

*注意：此指南不代表唯一正确的「标准」，而只是一个优秀风格的参考*

## Checkstyle & Findbugs

Java 项目应该在编译的时候（例如使用 maven 或者 gradle 时）使用 Checkstyle 和 Findbugs 的插件，检查和统一至少以下格式：

- `if`, `else` 必须带有 `{` 和 `}`，单行的例外（原因可以搜索 `GOTO fail; GOTO fail;`），但也要少写
- 类、变量和静态变量的命名，必须符合 Pascal，Camel 和全大写规则
- 每个 `.java` 文件只能有一个 top level class
- 滥用 `==` 的情况
- concurrency bugs
- 滥用 `System.exit`，`Thread.sleep` 的情况

## Immutability

POJO 应该尽量做到：

1. 构造函数私有
2. public static create method (工厂方法)
3. private final fields
4. public getters (without setters of course)
5. 对所有传入的 `Collection` 和 `Map` 都复制
6. 尽量使用 Guava 的 `ImmutableCollection` (即： `ImmutableList`, `ImmutableSet`)，这样 5. 的性能损失也就很小（ `ImmutableCollection` 的复制是 O(1) 操作）—— *when in doubt, copy*

## Avoid nulls

避免在尽量多情况下使用 null，而是使用 `java.util.Optional`，特别是函数返回值。函数（包括构造函数）参数都用 `Objects.requireNonNull` 检查。对于可能为 null 的情况，用 `Optional.ofNullable` 包装，并且标记 `@Nullable`。

注意 Guava 的 `ImmutableCollection` 也不允许 null。

## Use Preconditions and checks

使用 Guava 的 `Preconditions` 类，具体是 `checkArgument`，`checkState`。

凡是用户的输入，都必须用 `checkArgument` 检查，如果有误，及早的抛出 `IllegalArgumentException`；对于依赖其他系统的状态，尽量使用 `checkState` 来保证状态符合预期。对于依赖第三方服务的情况，尤其如此。

必须检查各种非纯函数（网络调用，文件读写，任何 IO 操作）的返回值，并且用上述两个方法检查。

善于用 `assert` 来检查组件的逻辑错误。注意的是 `assert` 只有在 JVM 使用 `-ea` 运行时才起作用，因而适合做较为耗时的检查，这样在 production 环境中可以关闭。又因检查的程序逻辑错误，因而抛出的是 Error 而非 Exception（默认行为是直接 shutdown JVM），在测试环境中可以有效的发现 bug。

## Use `ListenableFuture` when possible

待续
