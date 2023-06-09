# NextJs

-   [NextJs](#nextjs)
    -   [NextJs 설정](#nextjs-설정)
    -   [NextJs 환경 셋팅](#nextjs-환경-셋팅)
        -   [1. 프로젝트 생성](#1-프로젝트-생성)
        -   [2. 구동 포트 변경](#2-구동-포트-변경)
    -   [Apple 강좌](#apple-강좌)
    -   [NextAuth 활용](#nextauth-활용)
        -   [1.깃허브 OAuth설정](#1깃허브-oauth설정)
        -   [2.설치](#2설치)
        -   [3.소셜 로그인(GitHub)](#3소셜-로그인github)
        -   [4.세션 id/pass 방식](#4세션-idpass-방식)

## NextJs 설정

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

## NextJs 환경 셋팅

### 1. 프로젝트 생성

-   npx create-next-app@latest
-   What is your project named?(프로젝트 이름 입력)
-   Would you like to use TypeScript with this project? > No/Yes(타입스크립스 사용할텐가?)
-   Would you like to use ESLint with this project? › No / Yes(ESLint를 사용할텐가?)
-   Would you like to use Tailwind CSS with this project? › No / Yes(Tailwind를 사용할텐가?)
-   Would you like to use `src/` directory with this project? No / Yes(src디렉토리를 사용할텐가?)
-   Would you like to use experimental `app/` directory with this project? › No / Yes(실험적? app디렉토리를 사용할텐가)
-   What import alias would you like configured? › @/\* (루트 설정 alias를 어느것으로 하겠는가?)

### 2. 구동 포트 변경

-   package.json 파일 수정

```json
  "scripts": {
     "dev": "next dev -p 4343", <---- -p 포트 번호 추가
     "build": "next build",
     "start": "next start",
     "lint": "next lint"
 }
```

## Apple 강좌

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

## NextAuth 활용

### 1.깃허브 OAuth설정

-   Setting -> Developer setting -> New OAuth app
    -   Application Name : 어플 이름
    -   Homepage URL : 홈페이지
    -   Authorization callback URL : 홈페이지로 동일해도 상관 없음(로그인 후 돌아갈 URL)
-   Client ID, Client secrets 를 복사 해둠

### 2.설치

-   npm i next-auth

### 3.소셜 로그인(GitHub)

-   /pages/api/auth/[...nextauth].js 파일 생성

    ```JavaScript
       // [...nextauth].js
       import NextAuth from "next-auth";
       import GithubProvider from "next-auth/providers/github";
       export const authOptions = {
          providers: [
             GithubProvider({
                   clientId: "위에서 받은 Client ID",
                   clientSecret: "위에서 받은 Client secrets",
             }),
          ],
          secret: "JWT생성시 사용되는 암호",
       };

       export default NextAuth(authOptions);
    ```

-   로그인 기능 구현

    ```JavaScript
    "use client";
    import { signIn, signOut } from "next-auth/react";
    const LoginOut = (props) => {
       return (
          <div>
                <div>
                   <button
                      onClick={() => {
                            signIn();
                      }}
                   >
                      login
                   </button>
                </div>
                <div>
                   <button
                      onClick={() => {
                            signOut();
                      }}
                   >
                      logout
                   </button>
                </div>
          </div>
       );
    };
    export default LoginOut;
    ```

-   로그인 상태 확인

    -   Server

        ```JavaScript
        // page.js
        import { authOptions } from "@/pages/api/auth/[...nextauth]";
          import { getServerSession } from "next-auth";

          const ServerSession = async (props) => {
             const session = await getServerSession(authOptions);
             console.log(session);
             return (
                <div>
                      <div>서버세션</div>
                      <div>{JSON.stringify(session)}</div>
                </div>
             );
          };
          export default ServerSession;
        ```

    -   Client
        -   클라이언트에서 가져오면 문제가 있을수 있으니 서버에서 넘기는 걸로 하는게 좋다.

    ```JavaScript
    // layout.js
    "use client";

       import { SessionProvider } from "next-auth/react";

       export default function Layout({ children }) {
          return <SessionProvider>{children}</SessionProvider>;
       }

       // page.js
       "use client";

       import { useSession } from "next-auth/react";

       const ClientSession = async (props) => {
          let session = useSession();
          if (session) {
             console.log(session);
          }
          return (
             <div>
                   <div>클라이언트 세션</div>
             </div>
          );
       };
       export default ClientSession;
    ```

### 4.세션 id/pass 방식

-   DB Connector 설치
    -   npm i mongodb
-   DB adapter 설치
    -   npm i @next-auth/mongodb-adapter
    -   만약 몽고디비가 아님 각 디비에 맞는 아답터를 사용하면 됨
-   BCrypt 설치
    -   npm i bcrypt
-   DB접속 기능 생성

    -   DbConnector.js

        ```JS
        // DbConnector.js
        const { MongoClient } = require("mongodb");

        const url = "mongodb://dev:dev@localhost:27017/";
        const options = { useNewUrlParser: true };
        let connDb;

        const getDbConn = () => {
           if (!global._mongo) {
              global._mongo = new MongoClient(url, options).connect();
           }
           connDb = global._mongo;
           // console.log(connDb);
           return connDb;
        };

        export { getDbConn };

        ```

-   /pages/api/auth/[...nextauth].js 파일 수정

    ```JS
     // [...nextauth].js
     import NextAuth from "next-auth";
     import CredentialsProvider from "next-auth/providers/credentials";
     import { getDbConn } from "@/app/common/DbConnector";
     import { MongoDBAdapter } from "@next-auth/mongodb-adapter";
     export const authOptions = {
        providers: [
           CredentialsProvider({
                 name: "credentials",
                 // 로그인 페이지 만드는곳
                 credentials: {
                    // ID나 다른 값을 등을 입력 받고 싶을경우 이곳에 수정 한다.
                    email: { label: "email", type: "text" },
                    password: { label: "password", type: "password", placeholder: "password" },
                 },
                 // 로그인시 실행 되는곳
                 async authorize(credentials) {
                    console.log("authodize");
                    // dev_db에 users컬렉션에 유저가 저장되어 있다고 가정함
                    let user = await (await getCollection("dev_db", "users")).findOne({ email: credentials.email });
                    if (!user) {
                       console.log("해당 유저 없음 email=", credentials.email);
                       return null;
                    }
                    const pwcheck = await bcrypt.compare(credentials.password, user.password);
                    if (!pwcheck) {
                       console.log("비번오류");
                       return null;
                    }
                    return user;
                 },
           }),
        ],
        session: {
           // 세션 방식 및 유효시간 지정
           strategy: "jwt",
           maxAge: 30 * 24 * 60 * 60, // 30일 * 24시간 * 60분 * 60초
        },
        callbacks: {
           jwt: async ({ token, user }) => {
                 if (user) {
                    token.user = {};
                    token.user.name = user.name;
                    token.user.email = user.email;
                    // 유저의 롤이나 세션에 다른 값을 희망 하면 이곳에 넣는다.
                 }
                 return token;
           },
           session: async ({ session, token }) => {
                 session.user = token.user;
                 return session;
           },
        secret: "JWT생성시 사용되는 암호",
        adapter: MongoDBAdapter(getDbConn()),
     };

     export default NextAuth(authOptions);
    ```
