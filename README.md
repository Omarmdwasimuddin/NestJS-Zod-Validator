## Zod Validator

#### Install Zod
```bash
npm install zod
```
---


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

### `student.service.ts`
```bash
import { Injectable } from '@nestjs/common';
import { InjectPinoLogger, PinoLogger } from 'nestjs-pino';

@Injectable()
export class StudentService {
    constructor(
        @InjectPinoLogger(StudentService.name) private readonly logger: PinoLogger,
    ){}

    private students = [
        { id: 1, name: 'Wasim', age: 29 },
        { id: 2, name: 'Hannan', age: 25 },
    ];

    async createStudent(data: {name: string, age: number}){
        this.logger.info({ data }, 'Creating new student');
        const newStudent = {
            id: Date.now(),
            ...data,
        };
        this.students.push(newStudent);
        this.logger.info({ id: newStudent.id }, 'Student created successfully')
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
import { InjectPinoLogger, PinoLogger } from 'nestjs-pino';

@Controller('student')
export class StudentController {
    constructor(
        private readonly studentService: StudentService,
        @InjectPinoLogger(StudentController.name) private readonly logger: PinoLogger,
    ){}

    @Get()
    async getAllStudents(){
        return this.studentService.getAllStudents();
    }

    @Post()
    async createStudent(@Body() data: {name: string, age: number}){
        this.logger.info({ payload: data }, 'Incoming create-student request');
        return this.studentService.createStudent(data);
    }
}
```
---
