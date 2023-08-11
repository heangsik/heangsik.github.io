# NextJs

- [NextJs](#nextjs)
  - [NextJs 설정](#nextjs-설정)
  - [NextJs 환경 셋팅](#nextjs-환경-셋팅)
    - [1. 프로젝트 생성](#1-프로젝트-생성)
    - [2. 구동 포트 변경](#2-구동-포트-변경)
    - [3. SWR 설정](#3-swr-설정)
    - [4. ServerAction](#4-serveraction)
    - [DB연결](#db연결)
      - [설치](#설치)
      - [연결](#연결)
  - [Apple 강좌](#apple-강좌)
  - [NextAuth 활용](#nextauth-활용)
    - [1.깃허브 OAuth설정](#1깃허브-oauth설정)
    - [2.설치](#2설치)
    - [3.소셜 로그인(GitHub)](#3소셜-로그인github)
    - [4.세션 id/pass 방식](#4세션-idpass-방식)
    - [5.Custom LoginPage](#5custom-loginpage)
  - [Recoil 설정](#recoil-설정)
    - [1. 설치](#1-설치)
    - [2. 사용](#2-사용)

## NextJs 설정

1.  nodejs 설치
2.  npx create-next-app project_name --typescript 입력
3.  cd project_name
4.  npm run dev
5.  구동 포트 변경  
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

6.  daisyUI 설치

    -   npm i -D daisyui@latest
    -   설정정보 업데이트(tailwind.config.js)

        ```JavaScript
        // tailwind.config.js
        /** @type {import('tailwindcss').Config} */
        module.exports = {
           plugins: [require("daisyui")], // <--  플러그인 추가>
        };

        ```

7.  TailWind Plugin 설치
    -   Typography
        > npm i -D @tailwindcss/typography
    ```JS
    //tailwind.config.js
    module.exports = {
    theme: {
       // ...
    },
    plugins: [
       require('@tailwindcss/typography'),
       // ...
    ],
    }
    ```
    -   Forms
        > npm i -D @tailwindcss/forms
    ```JS
    //tailwind.config.js
    module.exports = {
    theme: {
       // ...
    },
    plugins: [
       require('@tailwindcss/forms'),
       // ...
    ],
    }
    ```
    -   Aspect ratio
        > npm i -D @tailwindcss/aspect-ratio
    ```JS
    //tailwind.config.js
    module.exports = {
    theme: {
       // ...
    },
    plugins: [
       require('@tailwindcss/aspect-ratio'),
       // ...
    ],
    }
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

### 3. SWR 설정

-   설치

    > npm i swr

    ```JS

      import useSWR from "swr";


      const {data, error} = useSWR('/api/points', url => {    return fetch(url).then(res => res.json())  })

      //-- OR ---

      const fetcher = (...args) => fetch(...args).then(res=>res.json());
      const {data, error} = useSWR('/api/points', fetcher);

      // 위의 내용을 처리 후
        if(error){
            return <div>failed to load</div>
         }
         if(!data){
            return <div>Loading..</div>
         }
         return <div>{data}</div>

    ```

    ### 4. AXIOS 설치

    -   설치
        > npm i axios @types/axios

### 4. ServerAction

-   설정파일
    ```JS
    // next.config.js
    module.exports = {
        experimental: {
           serverActions: true,
        },
     };
    ```
-   사용법

    ```JS
       export default async function Sample() {

          //3. 서버기능만들었음
          async function handleSubmit(formData) {
             'use server'; // <-- 필수
             // 여기에서 DB조회 등을 처리하면 된다.
             console.log(formData)
             console.log(formData.get('title'))
          }


          //2.폼만들었음
          return (
             <form action={handleSubmit}>
                <input type="text" name="title" />
                <button type="submit">Submit</button>
             </form>
          );
       }
    ```

### DB연결

#### 설치

> npm i mongodb

#### 연결

1. JavaScript

    - DbConnector.js

        ```JS
           const { MongoClient } = require("mongodb");

           const url = "mongodb://dev:dev@localhost:27017/";
           const options = { useNewUrlParser: true };
           let connDb;

           const getDbConn = () => {
              // 테스트 일경우만 한다.
              // 개발중 저장시 리플레쉬가 생겨서 들어있는 코드 이다.
              if (!global._mongo) {
                 global._mongo = new MongoClient(url, options).connect();
              }
              connDb = global._mongo;
              // console.log(connDb);
              return connDb;
           };

           const getDb = async (dbName) => {
              return (await getDbConn()).db(dbName);
           };

           const getCollection = async (dbName, collectionName) => {
              return await (await getDb(dbName)).collection(collectionName);
           };

           export { getCollection, getDbConn };

        ```

2. TypeScript

    - Global 셋팅(root/global.d.ts)

    ```TS
       import type { MongoClient } from "mongodb";

       declare global {
          namespace globalThis {
             var _mongo: Promise<MongoClient>;
          }
       }

    ```

    - DbConnector (/app/util/DbConn.ts)

    ```TS
    const { MongoClient } = require("mongodb");

       const url: string = "mongodb://dev:dev@localhost:27017/";
       const options: any = { useNewUrlParser: true };
       let connDb;

       async function getDbConn(): Promise<any> {
          if (!global._mongo) {
             global._mongo = new MongoClient(url, options).connect();
          }
          connDb = global._mongo;
          // console.log(connDb);
          return connDb;
       }

       async function getDb(dbName: string): Promise<any> {
          return (await getDbConn()).db(dbName);
       }

       async function getCollection(dbName: string) {
          const getCollection = async (dbName: string, collectionName: string) => {
             return await (await getDb(dbName)).collection(collectionName);
          };
       }

       export { getCollection, getDbConn };

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

           export default function page(props) {
              let session = useSession();
              console.log(session, new Date());
              return <div>클라이언트 세션</div>;
           }

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

### 5.Custom LoginPage

-   Login페이지 작성

    ```JavaScript
    //login page
    "use client";
    import { signIn } from "next-auth/react";
    import { useRouter } from "next/navigation";

    export default function SignIn(props) {
       const router = useRouter();
       const login = async (e) => {
          e.preventDefault(); // 기본 submit 이벤트를 막는다.
          const id = e.target.id.value;
          const password = e.target.password.value;
          try {
                const response = await signIn("id_password_credentials", { // 나중에 추가될 [...nextauth].js 파일에 CredentialsProvider항목에 선언된 id 이다.
                   id,  //[...nextauth].js의 credentials에 동일한 이름으로 들어가니 잘 체크 해야 한다.
                   password,
                   redirect: false,
                   callbackUrl: "http://localhost:3000/weather", // 로그인 성공 후 이동될 url 이다.
                });
                console.log("login response=", response);
                router.push(response.url); // 로그인 완료 후 라우팅
          } catch (error) {
                console.log(error);
          }
       };
       return (
          <div>
                <div>SignIn</div>
                <div>
                   <form onSubmit={login}>
                      <label>
                            id :
                            <input type="id" name="id" placeholder="user id" />
                      </label>
                      <label>
                            비밀번호 :
                            <input type="password" name="password" />
                      </label>
                      <button type="submit">로그인</button>
                   </form>
                </div>
          </div>
       );
    }
    ```

-   로그인 기능 추가

    ```JavaScript
    import NextAuth from "next-auth";
    import CredentialsProvider from "next-auth/providers/credentials";
    import { getCollection, getDbConn } from "@/app/common/DbConnector";
    import { MongoDBAdapter } from "@next-auth/mongodb-adapter";
    import bcrypt from "bcrypt";
    export const authOptions = {
       providers: [
          CredentialsProvider({
                name: "Credentials",
                type: "credentials",
                id: "id_password_credentials", // 로그인 페이지에 signIn 함수에 첫번째 파라메터
                // 로그인시 실행 되는곳 로그인 페이지 signIn
                async authorize(credentials) {
                   try {
                      console.log("login request:", credentials);
                      const usersCol = await getCollection("dev_db", "users");
                      let user = await usersCol.findOne({ user_id: credentials.id });
                      console.log("user=", user);
                      if (!user) {
                            console.log("해당 유저 없음 id=", credentials.id);
                            throw new Error("해당 유저 없음");
                      }
                      console.log(user.password);
                      const pwcheck = await bcrypt.compare(credentials.password, user.password);
                      if (!pwcheck) {
                            console.log("비번오류");
                            throw new Error("비번오류");
                      }
                      return user;
                   } catch (error) {
                      console.log(error);
                      throw error;
                   }
                },
          }),
       ],
       pages: {
          signIn: "/login/signin", // signIn호출시 표현될 페이지
       },
       session: {
          strategy: "jwt",
          maxAge: 30 * 24 * 60 * 60, // 30일 * 24시간 * 60분 * 60초
       },
       callbacks: {
          jwt: async (token, user) => {
                if (user) {
                   token.user = {};
                   token.user.name = user.name;
                   token.user.email = user.email;
                   token.user.role = user.role;
                }
                return token;
          },
          session: async ({ session, token }) => {
                session.user = token.user;
                return session;
          },
          signIn: async ({ user, account, profile, email, credentials }) => {
                return true;
          },
       },
       secret: "yhsJWT123!@#",
       adapter: MongoDBAdapter(getDbConn()),
    };

    export default NextAuth(authOptions);

    ```

## Recoil 설정

### 1. 설치

      npm i recoil

### 2. 사용

-   Recoil.js 생성

    ```JavaScript
    // /common/recoil/Recoil.js
    'use client'

    import { ReactNode } from 'react'
    import { RecoilRoot } from 'recoil'

    type Props = {
    children: ReactNode
    }

    export default function Recoil({ children }: Props) {
    return <RecoilRoot>{children}</RecoilRoot>
    }
    ```

-   RootLayOut 파일 수정

    ```JavaScript
     import "./globals.css";
     import { Inter } from "next/font/google";
     import { RecoilRoot } from "@/app/common/recoil"; // <-- 추가

     const inter = Inter({ subsets: ["latin"] });

     export const metadata = {
        title: "Create Next App",
        description: "Generated by create next app",
     };

     const getSession = async () => {};

     export default function RootLayout({ children }) {
        return (
           <html lang="en">
                 <body className={inter.className}>
                    <Recoil>{children}</Recoil> //<-div를 Recoil로 변경
                 </body>
           </html>
        );
     }

    ```

-   Atom생성

    ```JavaScript
    // app/atoms/CountAtom.js
    import { atom } from "recoil";

     export const CountAtom = atom({
        key: "CountStat",
        default: 0,
     });

    ```

-   사용

    ```JS
      "use client";
      import { CountAtom } from "@/app/recoil/CountAtom";
      import { useRecoilState } from "recoil";

      export default function recoil(props) {
         let [rCount, setRcount] = useRecoilState(CountAtom);
         const AddBtnR = (e) => {
            console.log("btn click");
            setRcount(rCount + 1);
         };
         return (
         <div>
            <div>recoil</div>
            <div>{rCount}</div>
            <div>
               <button onClick={AddBtnR}>add+1</button>
            </div>
         </div>
         );
      }

    ```
