# SVELTE 셋팅

## 목차

- [SVELTE 셋팅](#svelte-셋팅)
  - [목차](#목차)
  - [프로젝트 설치](#프로젝트-설치)
  - [포트 변경](#포트-변경)
  - [tailwind (with SvelteKit)설치](#tailwind-with-sveltekit설치)
  - [Test](#test)

## 프로젝트 설치

- svelte
  ```
    npm create svelte@latest project_name
    cd procject name
    npm i
    npm run dev
  ```
- svelteKit

  ```
    npm init svelte@next project_name
    cd project_name
    npm i
    npm run dev
  ```

- vite
  ```
    npm create vite@latest project_name (enter)
    Need to install the following packages:
      create-vite@5.0.0
    Ok to proceed? (y) (enter)
    ? Select a framework: Svelte 선택
    ? Select a variant: » SvelteKit 선택(이건 알아서 해라)
  ```

## 포트 변경

```json
  "scripts": {
    "build": "cross-env NODE_ENV=production webpack",
    "dev": "webpack serve --content-base public --port 5566"
  }
```

## tailwind (with SvelteKit)설치

- 설치

  - 입력

  ```
    npm i -D tailwindcss postcss autoprefixer daisyui
    npx tailwindcss init -p
  ```

  - 출력

  ```cmd
  Created Tailwind CSS config file: tailwind.config.js
  Created PostCSS config file: postcss.config.js
  ```

- path 설정
  - tailwind.config.js 수정
    ```js
    module.exports = {
      content: ["./src/**/*.{html,js,svelte,ts}"], // <-- 수정부분
      theme: {
        extend: {},
      },
      plugins: [require("daisyui")], // <--- 수정
    };
    ```
  - svelte.config.js 수정
    ```js
    import adapter from "@sveltejs/adapter-auto";
    import { vitePreprocess } from "@sveltejs/vite-plugin-svelte"; // <--- 추가
    /** @type {import('@sveltejs/kit').Config} */
    const config = {
      kit: {
        adapter: adapter(),
      },
      preprocess: vitePreprocess(), // <--- 추가
    };
    export default config;
    ```
- styles.css 수정

  ```js
    //./src/routes/styles.css 수정
    @import "@fontsource/fira-mono";
    @tailwind base; // <<- 추가
    @tailwind components; // <<- 추가
    @tailwind utilities; // <<- 추가

    :root {
      --font-body: Arial, -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto,
        Oxygen, Ubuntu, Cantarell, "Open Sans", "Helvetica Neue", sans-serif;
      --font-mono: "Fira Mono", monospace;
      --color-bg-0: rgb(202, 216, 228);
      --color-bg-1: hsl(209, 36%, 86%);
      --color-bg-2: hsl(224, 44%, 95%);
      --color-theme-1: #ff3e00;
      --color-theme-2: #4075a6;
      --color-text: rgba(0, 0, 0, 0.7);
      --column-width: 42rem;
      --column-margin-top: 4rem;
      font-family: var(--font-body);
      color: var(--color-text);
    }
    // .......
  ```

## Test

- vitest 설치
  > npm i -D vitest  
  > npm i -D @testing-library/svelte jest-dom jsdom  
  > npm i -D @vitest/ui
- 실행 스크립트 추가
  ```js
  # package.json 내용 수정
    "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview",
    "test": "vitest" <-- 추가
  },
  ```
- vitest 설정파일 추가

  - 프로젝트 root에 파일 추가

  ```js
  // vite.config.js
  import { defineConfig } from "vite";
  import { svelte } from "@sveltejs/vite-plugin-svelte";

  export default defineConfig({
    plugins: [svelte({ hot: !process.env.VITEST })],
    test: {
      globals: true,
      environment: "jsdom",
    },
  });
  ```

-
