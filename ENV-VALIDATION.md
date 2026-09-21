## .env file validate

#### .env file banao (project root e)
```bash
NODE_ENV=development
PORT=3000
```
---

>#### Create koro: `src/env.validation.ts`
#### ``
```bash
import { z } from 'zod';

export const envSchema = z.object({
  NODE_ENV: z.enum(['development', 'production', 'test']).default('development'),
  PORT: z.coerce.number().default(3000),
});

export type EnvConfig = z.infer<typeof envSchema>;

export function validateEnv(config: Record<string, unknown>): EnvConfig {
  const result = envSchema.safeParse(config);
  if (!result.success) {
    console.error('❌ Invalid environment variables:', result.error.format());
    process.exit(1);
  }
  return result.data;
}
```
---


#### @nestjs/config install koro
```bash
npm install @nestjs/config
```
---

#### `app.module.ts` e wire koro
```bash
import { Module } from '@nestjs/common';
import { ConfigModule } from '@nestjs/config';
import { AppController } from './app.controller';
import { AppService } from './app.service';
import { StudentModule } from './student/student.module';
import { APP_INTERCEPTOR } from '@nestjs/core';
import { ZodSerializerInterceptor } from 'nestjs-zod';
import { validateEnv } from './env.validation';

@Module({
  imports: [
    ConfigModule.forRoot({
      isGlobal: true,
      validate: validateEnv,
    }),
    StudentModule,
  ],
  controllers: [AppController],
  providers: [{ provide: APP_INTERCEPTOR, useClass: ZodSerializerInterceptor }, AppService],
})
export class AppModule {}
```
---

#### Structure update
```bash
src/
├── common/
│   └── filters/
│       └── zod-exception.filter.ts
├── student/
│   ├── dto/
│   │   ├── create-student.dto.ts
│   │   └── student-response.dto.ts
│   ├── student.controller.ts
│   ├── student.service.ts
│   └── student.module.ts
├── env.validation.ts
├── app.module.ts
└── main.ts
```
---
