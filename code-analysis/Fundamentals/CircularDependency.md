## Circular dependency 模块

> 当两个类相互依赖时，就会发生循环依赖。例如，类 A 需要类 B，而类 B 也需要类 A。在 Nest 中，模块之间以及提供程序之间都可能出现循环依赖。

```ts
@Module({
  controllers: [UsersController],
  providers: [UsersService, AuthService],
  exports: [UsersService],
  imports: [CatModule],
})
export class UsersModule {
}


@Module({
  controllers: [CatController],
  providers: [CatService, SharedService],
  exports: [CatService],
  imports: [UsersModule],
})
export class CatModule {
}
```
上述代码会报错
```text
Potential causes:
- A circular dependency between modules. Use forwardRef() to avoid it. Read more: https://docs.nestjs.com/fundamentals/circular-dependency
- The module at index [0] is of type "undefined". Check your import statements and the type of the module.
```

为了解决模块之间的循环依赖，请forwardRef()在模块关联的两端使用相同的实用函数。例如：
```ts
@Module({
  controllers: [UsersController],
  providers: [UsersService, AuthService],
  imports: [forwardRef(() => CatModule)],
  exports: [UsersService],
})
export class UsersModule {}

@Module({
  controllers: [CatController],
  providers: [CatService, SharedService],
  exports: [CatService],
  imports: [forwardRef(() => UsersModule)],
})
export class CatModule {}
```