## Module reference 模块

### `ModuleRef`

> ModuleRef 是 NestJS 提供的一个工具，用于在运行时动态地获取或创建模块中的提供者实例，适用于需要根据条件选择服务、解决循环依赖或进行单元测试等场景。

```ts
// notification.controller.ts
import { Controller, Get, Query } from '@nestjs/common';
import { ModuleRef } from '@nestjs/core';
import { NotificationService } from './notification.service';
import { EmailNotificationService } from './email-notification.service';
import { SmsNotificationService } from './sms-notification.service';

@Controller('notifications')
export class NotificationController {
  private notificationService: NotificationService;

  constructor(private moduleRef: ModuleRef) {
  }

  @Get()
  sendNotification(@Query('type') type: 'email' | 'sms') {
    if (type === 'email') {
      this.notificationService = this.moduleRef.get(EmailNotificationService);
    } else if (type === 'sms') {
      this.notificationService = this.moduleRef.get(SmsNotificationService);
    }

    this.notificationService.send('Hello, this is a test message.');
    return { status: 'Message sent!' };
  }
}
```
