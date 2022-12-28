# NestJs 셋팅

## 0. 목차

- [NestJs 셋팅](#nestjs-셋팅)
  - [0. 목차](#0-목차)
  - [1. NodeJs 설치](#1-nodejs-설치)
  - [2. 프로젝트 생성](#2-프로젝트-생성)
  - [3. 프로젝트 디렉토리를 VsCode로 연다](#3-프로젝트-디렉토리를-vscode로-연다)
  - [4. main.ts 파일에 포트를 변경 할수](#4-maints-파일에-포트를-변경-할수)
  - [5. 프로젝트 디렉토리로 이동 후 VSCode로 해당 디렉토리를 연다](#5-프로젝트-디렉토리로-이동-후-vscode로-해당-디렉토리를-연다)
  - [6. Create Movie Project](#6-create-movie-project)
  - [7. 어플리케이션 서비스 포트 변경 및 개발 상용 환경 설정](#7-어플리케이션-서비스-포트-변경-및-개발-상용-환경-설정)
  - [9. Controller/Service 생성](#9-controllerservice-생성)
  - [10. Validator 설치](#10-validator-설치)

## 1. NodeJs 설치

> https://nodejs.org/ko/download/ --> 이동 후 node 다운 후 설치  
> npm i -g @nestjs/cli --> nestjs 설치

## 2. 프로젝트 생성

> nest new project_name --> 입력  
> npm 선택  
> cd project_name  
> npm run start:dev

## 3. 프로젝트 디렉토리를 VsCode로 연다

> code .

## 4. main.ts 파일에 포트를 변경 할수

> npx create-next-app@latest --> 입력  
> What is your projet named? --> 프로젝트 디렉토리 입력  
> Would you like to use TypeScript with this project? --> 타입스크립트 사용 여부  
> Would you like to use ESLint with this project? --> ESLint 사용여부

## 5. 프로젝트 디렉토리로 이동 후 VSCode로 해당 디렉토리를 연다

> code .

## 6. Create Movie Project

> nest new project_name  
> cd movie_service  
> npm run start:dev 실행

## 7. 어플리케이션 서비스 포트 변경 및 개발 상용 환경 설정

> npm i @nestjs/config 설치
> npm i cross-env 설치

- 파일 추가(루트디렉토리에 파일 생성 .env.dev or .evn.prod

```
##.evn.dev
NODE_SERVER_PORT=7677

##.evn.prod
NODE_SERVER_PORT=7678
```

- 파일 수정(app.module.ts)

```TypeScript
import { ConfigModule } from '@nestjs/config'; // ## 추가
import { Module } from '@nestjs/common';
import { AppController } from './app.controller';
import { AppService } from './app.service';

@Module({
  imports: [ConfigModule.forRoot({ // ## 변경
    isGlobal: true, // ## 변경 다른 파일에서도 읽어 들일수 있게 설정,
    envFilePath: process.env.NODE_ENV === 'dev' ? '.env.dev' : '.env.prod', // 리얼과 개발 서버 설정 파일 처리
  })], // ## 변경
  controllers: [AppController],
  providers: [AppService],
})
export class AppModule { }
```

- 파일 수정(main.ts)

```TypeScript
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';

async function bootstrap() {
  const port = process.env.NODE_SERVER_PORT; // ##  수정

  const app = await NestFactory.create(AppModule);
  await app.listen(port); // ## 수정
  console.log(`application server port : ${port} starting~~`); // ## 수정
}
bootstrap();
```

- 파일 수정(package.json)

```json
    // 수정전
    "start:dev": "nest start --watch",
    "start:prod": "node dist/main",


    // 수정후
    "start:dev": "cross-env NODE_ENV=dev nest start --watch",
    "start:prod": "cross-env NODE_ENV=prod node dist/main",
```

- 실행
  > npm run start:dev  
  > application server port : 7677 starting~~ <-- 확인

## 9. Controller/Service 생성

> root/nest g co Movie
> 입력 후 파일 생성 확인 movie/movie.controller.ts
> root/nest go s Movie
> 입력 후 파일 생성 확인 movie/moive.service.ts

- controller.ts

  ```TypeScript
    @Get(`/all`)
    movieList(): string {
        return `movie all list`;
    }

  ```

## 10. Validator 설치

> npm i class-validator class-transform

```TypeScript
//main.ts 수정
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';
import { ValidationPipe } from '@nestjs/common'; // ##추가
async function bootstrap() {
  const app = await NestFactory.create(AppModule);

  // 아래 내용 추가-----------------
  app.useGlobalPipes(
    new ValidationPipe({
      transform: true, // 요청값을 dto로 변환
      whitelist: true, // dto에 선언되지 않은 요청값은 제거
      forbidNonWhitelisted: true, // whitelist:true 가 설정되어 있을경우 dto에 설정된 이외의 값이 들어올경우 오류를 낸다.
      disableErrorMessages: true, // 디테일한 오류 메시지를 응답에서 뺀다.
    }),
  ),
  // 위 내용 추가-------------------
    await app.listen(3030);
}
bootstrap();
```
