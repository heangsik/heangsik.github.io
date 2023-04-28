# NextJs 설정

1. nodejs 설치
2. npx create-next-app project_name --typescript 입력
3. cd project_name
4. npm run dev
5. 구동 포트 변경  
   main.ts

   ```typescript
   import { NestFactory } from "@nestjs/core";
   import { AppModule } from "./app.module";

   async function bootstrap() {
     const app = await NestFactory.create(AppModule);
     await app.listen(3030); // <-- 원하는 포트를 지정
   }
   bootstrap();
   ```

   또는 package.json 파일에 프로퍼티 추가

   ```json
   "scripts": {
       "prebuild": "rimraf dist",
       "build": "nest build",
       "format": "prettier --write \"src/**/*.ts\" \"test/**/*.ts\"",
       "start": "nest start -p 3030", // <-- p 테그 추가
       "start:dev": "nest start --watch ${PORT-3000}", // <-- 환경변수로 변경 가능
       "start:debug": "nest start --debug --watch",
       "start:prod": "node dist/main",
       "lint": "eslint \"{src,apps,libs,test}/**/*.ts\" --fix",
       "test": "jest",
       "test:watch": "jest --watch",
       "test:cov": "jest --coverage",
       "test:debug": "node --inspect-brk -r tsconfig-paths/register -r ts-node/register node_modules/.bin/jest --runInBand",
       "test:e2e": "jest --config ./test/jest-e2e.json"
   },
   ```

# NextJs 환경 셋팅

1. 프로젝트 생성
   - npx create-next-app@latest
   - What is your project named?(프로젝트 이름 입력)
   - Would you like to use TypeScript with this project? > No/Yes(타입스크립스 사용할텐가?)
   - Would you like to use ESLint with this project? › No / Yes(ESLint를 사용할텐가?)
   - Would you like to use Tailwind CSS with this project? › No / Yes(Tailwind를 사용할텐가?)
   - Would you like to use `src/` directory with this project? No / Yes(src디렉토리를 사용할텐가?)
   - Would you like to use experimental `app/` directory with this project? › No / Yes(실험적? app디렉토리를 사용할텐가)
   - What import alias would you like configured? › @/\* (루트 설정 alias를 어느것으로 하겠는가?)
2. 구동 포트 변경
   - package.json 파일 수정
   ```json
     "scripts": {
        "dev": "next dev -p 4343", <---- -p 포트 번호 추가
        "build": "next build",
        "start": "next start",
        "lint": "next lint"
    }
   ```

# Apple 강좌

1. 환경 셋팅
   - What is your project named?(apple_s1 입력)
   - Would you like to use TypeScript with this project? > No / Yes(No)
   - Would you like to use ESLint with this project? › No / Yes(No)
   - Would you like to use Tailwind CSS with this project? › No / Yes(Yes)
   - Would you like to use `src/` directory with this project? No / Yes(No)
   - Would you like to use experimental `app/` directory with this project? › No / Yes(Yes)
   - What import alias would you like configured? › @/\* (Enter)
2. Code 실행
3.
