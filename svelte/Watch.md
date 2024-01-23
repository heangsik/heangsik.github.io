# 시계 만들기

## init

- 입력

  > npm create vite@latest s_watch_1

- Select a framework: **_Svelte_** <-선택
- Select a variant: » **_JavaScript_** <-선택

## tailwind 설치

- 입력

  > npm i -D tailwindcss postcss autoprefixer daisyui  
  > npx tailwindcss init tailwind.config.cjs -p

- 출력
  ```cmd
  Created Tailwind CSS config file: tailwind.config.js
  Created PostCSS config file: postcss.config.js
  ```
- 설정
  - tailwind.config.cjs
    ```js
    /** @type {import('tailwindcss').Config} */
    export default {
      content: ["./src/**/*.{html,js,svelte,ts}"], //<--- 수정
      theme: {
        extend: {},
      },
      plugins: [require("daisyui")], //<--- 수정
    };
    ```
  - app.css
    ```css
    # 파일 가장 위에 아래 내용 추가
    @tailwind base;
    @tailwind components;
    @tailwind utilities;
    ```
