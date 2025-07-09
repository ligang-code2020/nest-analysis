## Custom providers 模块

### DI

> 依赖注入是一种控制反转 (IoC)技术，其中你将依赖项的实例化委托给 IoC 容器（在我们的例子中是 NestJS
> 运行时系统），而不是在你自己的代码中命令式地执行

### `useValue`

> 该useValue语法适用于注入常量值、将外部库放入 Nest 容器，或用模拟对象替换实际实现。

```ts
// database.module.ts
const databaseConfig = {
  host: 'localhost',
  port: 5432,
  username: 'admin',
  password: 'secret',
};

@Module({
  providers: [
    {
      provide: 'DATABASE_CONFIG', // 自定义 token 名称
      useValue: databaseConfig,   // 注入的值
    },
  ],
  exports: ['DATABASE_CONFIG'],  // 暴露出去让其他模块使用
})
export class DatabaseModule {
}


// database.service.ts
import { Injectable, Inject } from '@nestjs/common';

@Injectable()
export class DatabaseService {
  constructor(
    @Inject('DATABASE_CONFIG') private readonly config: any
  ) {
  }

  connect() {
    console.log(`Connecting to DB at ${this.config.host}:${this.config.port}`);
    // 实际连接数据库逻辑...
  }
}
```

### `useClass`

> 该useClass语法允许您动态地确定令牌应解析为的类，useClass 的主要用途是替换依赖或模拟服务，特别是在测试环境中非常有用。你可以通过
> useClass 来告诉 NestJS 使用不同的类来代替默认的服务类。这使得你可以灵活地改变应用中的某些部分的行为，而不需要修改核心逻辑。

```ts
// test.module.ts
import { Module } from '@nestjs/common';
import { MockCatsService } from './mock-cats.service';
import { CatsService } from './cats.service';
import { TestController } from './test.controller';

@Module({
  providers: [
    {
      provide: CatsService,         // 我们要注入的 token
      useClass: MockCatsService,     // 实际使用的类
    },
  ],
  controllers: [TestController],
  exports: [CatsService],           // 可选：如果你想导出供别的模块使用
})
export class TestModule {
}
```

只要你在模块中配置了 { provide: CatsService, useClass: MockCatsService }，那么任何地方注入 CatsService 都会得到
MockCatsService 的实例。

### `useFactory`

> useFactory 是一种通过函数动态创建提供者的机制, 它允许你在注册提供者时，通过一个工厂函数返回一个值或对象实例，而不是直接提供类或者静态值。

```ts
// app.module.ts
import { Module } from '@nestjs/common';
import { LoggerService } from './logger.service';

@Module({
  providers: [
    {
      provide: 'LOGGER',
      useFactory: () => {
        const logLevel = process.env.LOG_LEVEL || 'info';
        return new LoggerService(logLevel);
      },
    },
  ],
  exports: ['LOGGER'],
})
export class AppModule {
}
```

