## Zod Validation


## Create Module, Service, and Controller
```bash
nest g module student
```
```bash
nest g controller student
```
```bash
nest g service student
```
---

#### Install zod, nestjs-zod
```bash
npm install zod nestjs-zod
```
---

>#### Create koro: `student/dto/create-student.dto.ts`
#### `create-student.dto.ts`
```bash
import { createZodDto } from 'nestjs-zod';
import { z } from 'zod';

// Ei schema-i validation rule define kore
export const CreateStudentSchema = z.object({
  name: z.string().min(2, 'Name must be at least 2 characters'),
  age: z.number().int().positive().max(120),
});

// Eta DTO class banay jeta Nest bujhe
export class CreateStudentDto extends createZodDto(CreateStudentSchema) {}
```
---


#### `main.ts`
```bash
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';
import { ZodValidationPipe } from 'nestjs-zod';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  app.useGlobalPipes(new ZodValidationPipe()); // ei line ta add koro
  await app.listen(process.env.PORT ?? 3000);
}
bootstrap();
```
---

#### `student.service.ts`
```bash
import { Injectable } from '@nestjs/common';

@Injectable()
export class StudentService {
    private students = [
        { id: 1, name: 'Wasim', age: 29 },
        { id: 2, name: 'Hannan', age: 25 },
    ];

    async createStudent(data: {name: string, age: number}){
        const newStudent = {
            id: Date.now(),
            ...data,
        };
        this.students.push(newStudent);
        return newStudent;
    }

    async getAllStudents(){
        return this.students;
    }
}
```
---

### `student.controller.ts`
```bash
import { Body, Controller, Get, Post } from '@nestjs/common';
import { StudentService } from './student.service'
import { CreateStudentDto } from './dto/create-student.dto';

@Controller('student')
export class StudentController {
    constructor(private readonly studentService: StudentService){}

    @Get()
    async getAllStudents(){
        return this.studentService.getAllStudents();
    }

    @Post()
    async createStudent(@Body() data: CreateStudentDto){
        return this.studentService.createStudent(data);
    }
}
```
---


>## OUTPUT
<img width="1304" height="606" alt="image" src="https://github.com/user-attachments/assets/d016223f-15ad-4f0c-bc1b-2dcaaa722dd5" />

<img width="1305" height="409" alt="image" src="https://github.com/user-attachments/assets/023a7139-cb23-4491-8476-11558c0652e0" />

---
