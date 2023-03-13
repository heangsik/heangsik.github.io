# NestJs에 TypeOrm (Sqlite)추가

## Project생성

```shell
npm i -g @nestjs/cli <-- nestjs 설치
nest new crud_project
Which package manager would you ❤️  to use? NPM 선택
생성 완료 후 cd crud_project
```

## 필요 모듈 추가 설치

```shell
npm i @nestjs/config @nestjs/typeorm typeorm sqlite3
```

## ide 실행

```shell
cd crud_project
code .
```

## 설정 파일 생성

- 프로젝트 루트에 .env 파일 생성

  ```env
      APP_SERVER_PORT=7672
      APP_NAME="CRUD App"
      DB_TYPE="sqlite"
      DB_HOST="localhost"
      DB_PORT=5432
      DB_DATABASE="crud.db"
      DB_NAME="CRUD DB"
      DB_SYNC="TRUE"
      DB_ENTITES="dist/\*_/_.entity.{js, ts}"
      DB_DROPSCHEMA="TRUE"
      DB_LOGGING="TRUE"
      DB_PASSWORD="yhs"
      DB_USERNAME="yhs"
  ```

## DB접속 정보 설정

- app.module.ts 수정

```TS
import { Module } from '@nestjs/common';
import { TypeOrmModule } from '@nestjs/typeorm';
import { AppController } from './app.controller';
import { AppService } from './app.service';
import { CrudModule } from './crud/crud.module';
import { TodosModule } from './todos/todos.module';

@Module({
  imports: [
    TypeOrmModule.forRoot({ // ----------------- 추가 start
      type: 'sqlite', //--DB
      database: 'crud_1.db',
      autoLoadEntities: true,
      // entities: [join(__dirname, '**', '*.entity{.ts,.js}')],
      synchronize: true,
      logging: true,
      dropSchema: true,
    }),//--------- 추가 end
    CrudModule,
    TodosModule,
  ],
  controllers: [AppController],
  providers: [AppService],
})
export class AppModule {}

```

## 설정 파일 읽어 들이기(접속 정보를 설정파일로 빼는경우)

- config.ts파일 생성(위치:src/config/config.ts)

  ```TypeScript
  export const config = () => ({
      app: {
          port: +process.env.APP_SERVER_PORT,
          name: process.env.APP_NAME,
      },
      db: {
          name: process.env.DB_NAME,
          type: process.env.DB_TYPE,
          database: process.env.DB_DATABASE,
          host: process.env.DB_DATABASE,
          port: process.env.DB_HOST,
          username: process.env.DB_USERNAME,
          password: process.env.DB_PASSWORD,
          synchronize: process.env.DB_SYNC === 'TRUE' ? true : false,
          dropSchema: process.env.DB_DROPSCHEMA === 'TRUE' ? true : false,
          logging: process.env.DB_LOGGING === 'TRUE' ? true : false,
          entities: [process.env.DB_ENTITES],
      },
  });
  ```

- app.module.ts 수정

  ```TypeScript
  import { ConfigModule } from '@nestjs/config';

  import { Module } from '@nestjs/common';
  import { AppController } from './app.controller';
  import { AppService } from './app.service';
  import { config } from './config/config';  // <--- 추가

  @Module({
          imports: [
              ConfigModule.forRoot({
                  isGlobal: true, // <--- 다른 파일에서도 읽어 들이도록 설정
                  envFilePath: '.env' // <-- env파일 설정 기본 .env이다.
                  load: [config], // <-- env파일을 설정 파일로 변경
              }),
          ],
          controllers: [AppController],
          providers: [AppService],
      })
  export class AppModule {}

  ```

- 설정 정보 확인(main.ts)

  ```TypeScript
    import { ConfigService } from '@nestjs/config/dist';
    import { NestFactory } from '@nestjs/core';
    import { AppModule } from './app.module';

    async function bootstrap() {
    const app = await NestFactory.create(AppModule);

    const config = app.get(ConfigService); // <---추가
    console.log(config); //<-- 콘솔에 출력

    await app.listen(config.get('app.port'));//<-- 서버 구동시 설정 정보 셋팅
    }
    bootstrap();
  ```

## TypeOrm 설정

- 서비스 생성
  ```shell
  nest g res crud // 입력
  ? What transport layer do you use? REST API <-- 선택
  ? Would you like to generate CRUD entry points? (Y/n)<-- Y
  ```
- app.module.ts 수정

  ```TS
  import { Module } from '@nestjs/common';
  import { ConfigModule } from '@nestjs/config';
  import { TypeOrmModule } from '@nestjs/typeorm';
  import { AppController } from './app.controller';
  import { AppService } from './app.service';
  import { config } from './config/config';
  import { CrudModule } from './crud/crud.module';

  @Module({
  imports: [
      ConfigModule.forRoot({
      isGlobal: true,
      envFilePath: '.env',
      load: [config],
      }),
      TypeOrmModule.forRoot({ // <---요기부터      type: 'sqlite',
      database: "crud.db", // 파일 생성 이름
      entities: ["dist/**/*.entity.{js, ts}"], // entity 위치 및 파일 이름
      synchronize: true, // 구동시 테이블 생성여부 운영은 이렇게 하면 큰일난다.
      }), //<--요기까지 추가
      CrudModule,
  ],
  controllers: [AppController],
  providers: [AppService],
  })
  export class AppModule {}

  ```

6.

```

```

```

```
