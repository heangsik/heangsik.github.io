# NestJs 셋팅

## 목차

- [NestJs 셋팅](#nestjs-셋팅)
  - [목차](#목차)
  - [NodeJs 설치](#nodejs-설치)
  - [프로젝트 생성](#프로젝트-생성)
  - [프로젝트 디렉토리를 VsCode로 연다](#프로젝트-디렉토리를-vscode로-연다)
  - [Create Movie Project](#create-movie-project)
  - [어플리케이션 서비스 포트 변경 및 개발 상용 환경 설정](#어플리케이션-서비스-포트-변경-및-개발-상용-환경-설정)
  - [Controller/Service 생성](#controllerservice-생성)
  - [Validator 설치](#validator-설치)
  - [MAPPER 설치](#mapper-설치)
  - [Delete 'CR' eslint(prettier/prettier) 오류 처리](#delete-cr-eslintprettierprettier-오류-처리)
  - [TypeOrm Sqlite 셋팅](#typeorm-sqlite-셋팅)
  - [TypeOrm 설정정보 파일로 빼기](#typeorm-설정정보-파일로-빼기)
  - [응답전문에 값제거](#응답전문에-값제거)
  - [Todo](#todo)

## NodeJs 설치

> https://nodejs.org/ko/download/ --> 이동 후 node 다운 후 설치  
> npm i -g @nestjs/cli --> nestjs 설치

## 프로젝트 생성

> nest new project_name --> 입력  
> npm 선택  
> cd project_name  
> npm run start:dev

## 프로젝트 디렉토리를 VsCode로 연다

> code .

---

## Create Movie Project

> nest new project_name  
> cd movie_service  
> npm run start:dev 실행

## 어플리케이션 서비스 포트 변경 및 개발 상용 환경 설정

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
    isGlobal: true, // ## 변경  : 설정 정보를 다른 파일에서도 읽어 들일수 있게 설정,
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

## Controller/Service 생성

> root/nest g co Movie  
> 입력 후 파일 생성 확인 movie/movie.controller.ts  
> root/nest g s Movie  
> 입력 후 파일 생성 확인 movie/moive.service.ts

- controller.ts

  ```TypeScript
    @Get(`/all`)
    movieList(): string {
        return `movie all list`;
    }

  ```

## Validator 설치

> npm i class-validator class-transformer

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

## MAPPER 설치

> npm i mapped-types

- 만약 npm ERR! ERESOLVE unable to resolve dependency tree 발생시
  ```console
    npm ERR! code ERESOLVE
    npm ERR! ERESOLVE unable to resolve dependency tree
    npm ERR!
    npm ERR! While resolving: std_2@0.0.1
    npm ERR! Found: class-validator@0.14.0
    npm ERR! node_modules/class-validator
    npm ERR!   class-validator@"^0.14.0" from the root project
    npm ERR!   peerOptional class-validator@"*" from @nestjs/common@9.2.1
    npm ERR!   node_modules/@nestjs/common
    npm ERR!       @nestjs/mapped-types@"*" from the root project
    npm ERR!
    npm ERR! Could not resolve dependency:
    npm ERR! peerOptional class-validator@"^0.11.1 || ^0.12.0 || ^0.13.0" from @nestjs/mapped-types@1.2.0
    npm ERR! node_modules/@nestjs/mapped-types
    npm ERR!   @nestjs/mapped-types@"*" from the root project
    npm ERR!
    npm ERR! A complete log of this run can be found in:
  ```
- 내용은 mapped가 class-validator를 0.11, 0.12, 0.13지원인데 0.14가 설치 되어 있어 설치 못한다.
- 그리하여 호환되는 class-validator를 설치해야 한다.
- 기존 class-validator 삭제 -> npm uninstall class-validator
- 호환 class-validator 설치 -> npm i class-validator@0.13.0
- 원하는 모듈 설치 -> npm i mapped-types

## Delete 'CR' eslint(prettier/prettier) 오류 처리

- .prettierrc 파일에 "endOfLine": "auto" 추가

## TypeOrm Sqlite 셋팅

- 모듈 설치
  ```scripts
  npm i @nestjs/typeorm typeorm sqlite3
  ```
- app.module.ts 파일 수정

  ```TypeScript

  import { TypeOrmModule } from '@nestjs/typeorm'; //<< 추가
  @Module({
    imports: [
      TypeOrmModule.forRoot({ //<< 추가 ----
        type: 'mysql',
        host: 'localhost',
        port: 3306,
        username: 'root',
        password: '1234',
        database: 'nest',
        autoLoadEntities: true,
        synchronize: true,
        logging: true,
      }),// << 추가 ----
      ProductsModule,
    ],
    controllers: [],
    providers: [],
  })
  export class AppModule {}
  ```

## TypeOrm 설정정보 파일로 빼기

- src/config/db.config.ts 생성
  ```TypeScript
    export const config = () => ({
    app: {
      port: +process.env.APP_SERVER_PORT,
      name: process.env.APP_NAME,
    },
    db: {
      name: process.env.DB_NAME,
      type: process.env.DB_TYPE,
      database: process.env.DB_DB,
      synchronize: process.env.DB_SYNC === 'TRUE' ? true : false,
      dropSchema: true,
      logging: true,
      entities: ['dist/**/*.entity.{js, ts}'],
      // migrations: ['dist/migration/**/*.js'],
      // subscribers: ['dist/subscriber/**/*.js'],
    },
  });
  ```
- 설정파일 수정(내용 추가)

```.evn
 APP_NAME="CRUD App"

 DB_TYPE="sqlite"
 DB_HOST="localhost"
 DB_PORT=5432
 DB_DB="crud.db"
 DB_NAME="CRUD DB"
 DB_SYNC="TRUE"
```

- app.module.ts 수정
  ```TypeScript
  import { Module } from '@nestjs/common';
  import { AppController } from './app.controller';
  import { AppService } from './app.service';
  import { ConfigModule } from '@nestjs/config';
  import { TypeOrmModule } from '@nestjs/typeorm';
  import { config } from './config/config';
  import { DatabaseConnectionService } from './database-connection.service';
  @Module({
  imports: [
    ConfigModule.forRoot({
      isGlobal: true,
      envFilePath: process.env.NODE_ENV === 'dev' ? '.env.dev' : '.env.prod',
      load: [config], // <----추가
    }),
    TypeOrmModule.forRootAsync({
      imports: [ConfigModule],
      useClass: DatabaseConnectionService,
    }),
  ],
  controllers: [AppController],
  providers: [AppService],
  })
  export class AppModule {}
  ```

## 응답전문에 값제거

- 응답전문에 특정값을 제거 하고자 할때 예) 유저정보중 패스워드 등

  ```TypeScript
    //entity 파일에 제거 하고자 하는 값에 Exclude 데코레이터를 단다.
    @Entity()
    export class User{
      @Colummn()
      UserId: string;
      @Colummn()
      UserAge: number;
      @Colummn()
      @Exculude() // <<---- 추가
      Password: string;
      constructor(partial: Partial<전달받는_Dto>) {
        Object.assign(this, partial);
      }
    }
  ```

- 응답을 하는 메서드에 추가

- ```TypeScript
    @UseInterceptors(ClassSerializerInterceptor) //<-- 응답을 반환하는 메서드에 붙인다.
  ```

- main.ts 추가
  ```
    app.useGlobalInterceptors(new ClassSerializerInterceptor(app.get(Reflector)));
  ```

## 패스워드 암호화

- npm i bcrypt @types/bcrypt 설치
- ```JavaScript
  import * as bcrypt from 'bcrypt';
  async encript(oriMsg:string) : Promise<string>{
    return await bcrypt.hash(oriMsg, 10);
  }
  ```

```

```

## Todo

- nest g res todo <-- 입력
- todo.entity.ts 생성

  ```TypeScript
  import {
    BaseEntity,
    Column,
    CreateDateColumn,
    Entity,
    PrimaryGeneratedColumn,
  } from "typeorm";
  @Entity("TODO")
  export class Todo extends BaseEntity {
    constructor() {
      super();
    }
    @PrimaryGeneratedColumn()
    seq: number;
    @Column()
    todo: string;
    @Column({ default: false })
    checked: boolean;
    @Column({ default: false })
    deleted: boolean;
    @CreateDateColumn()
    createDt: Date;
    @Column()
    updateDt: Date;
  }
  ```

- todo.module.ts 수정
  ```TypeScript
  import { Module } from '@nestjs/common';
  import { TodoService } from './todo.service';
  import { TodoController } from './todo.controller';
  import { TypeOrmModule } from '@nestjs/typeorm';
  import { Todo } from './entities/todo.entity';
  @Module({
  imports: [TypeOrmModule.forFeature([Todo])], //<---- 추가
  controllers: [TodoController],
  providers: [TodoService],
  })
  export class TodoModule {}
  ```
- todo.service.ts 수정

  - 레파지토리 추가

  ```TS

  export class LoginService {
    constructor(
      // 아래 추가
      @InjectRepository(Todo)
      private todoRepository: Repository<Todo>,
    ) {}

    //..........
  }
  ```
