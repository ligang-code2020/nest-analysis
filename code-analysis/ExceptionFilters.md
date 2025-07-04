## Exception filters 模块
Nest 内置了一个异常处理层 ，负责处理应用程序中所有未捕获的异常。当应用程序代码未处理某个异常时，该层会捕获它并自动返回用户友好的响应。


### @Catch 装饰器
> 源码位置：packages/common/decorators/core/catch.decorator.ts


### @UseFilters 装饰器
> 源码位置：packages/common/decorators/core/exception-filters.decorator.ts