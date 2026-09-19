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

@Controller('student')
export class StudentController {
    constructor(private readonly studentService: StudentService){}

    @Get()
    async getAllStudents(){
        return this.studentService.getAllStudents();
    }

    @Post()
    async createStudent(@Body() data: {name: string, age: number}){
        return this.studentService.createStudent(data);
    }
}
```
---

#### Install nestjs-zod
```bash
npm install nestjs-zod
```
---

>#### Create koro: student/dto/create-student.dto.ts
#### ``
```bash

```
---
