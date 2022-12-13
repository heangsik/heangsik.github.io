NextJs 설정   
===

1. nodejs 설치
2. npx create-next-app project_name --typescript 입력
3. cd project_name
4. npm run dev
5. 구동 포트 변경   
    main.ts
    ```typescript 
    import { NestFactory } from '@nestjs/core';
    import { AppModule } from './app.module';

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
        "start:dev": "nest start --watch",
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