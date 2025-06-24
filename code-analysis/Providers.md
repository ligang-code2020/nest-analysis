## Providers 模块
Providers 是 NestJS 中依赖注入（Dependency Injection, DI）系统的核心部分。
它们可以是服务、存储库、工厂函数等任何能够通过构造函数注入到类中的东西。

### Injectable 的实现
这个装饰器的主要作用是标记一个类为可注入的服务（Provider），以便 NestJS IoC 容器能够识别并管理它。
> 源码实现：packages/common/decorators/core/injectable.decorator.ts