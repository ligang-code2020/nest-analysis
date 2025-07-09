## Interceptors 模块

> 源码位置：packages/common/decorators/core/injectable.decorator.ts

> 主要负责消费所有拦截器并组合成一个完整的调用链。</br>
> 源码位置：packages/core/interceptors/interceptors-consumer.ts


> 源码位置：packages/common/decorators/core/use-interceptors.decorator.ts

```ts
/*
// 多个拦截器执行顺序
┌────────────┐
│ Logging (1)│ ← 先执行 before
├────────────┤
│Transform(2)│ ← 第二个 before
├────────────┤
│  Cache (3) │ ← 最后 before
└─────┬──────┘
      ▼
[ Controller Method ]
      ▼
┌─────┴──────┐
│  Cache (3) │ ← 先执行 after
├────────────┤
│Transform(2)│ ← 第二个 after
├────────────┤
│ Logging(1) │ ← 最后 after
└────────────┘
*/
```