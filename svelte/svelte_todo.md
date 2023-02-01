# SVELTE Todo

## 프로젝트 생성

```Shell
  npm init svelte@next svelte_todo
  √ Which Svelte app template? » SvelteKit demo app
  √ Add type checking with TypeScript? » Yes, using TypeScript syntax
  √ Add ESLint for code linting? ... No / Yes
  √ Add Prettier for code formatting? ... No / Yes
  √ Add Playwright for browser testing? ... No / Yes
  √ Add Vitest for unit testing? ... No / Yes
```

## 이동 및 설치

- 프로젝트 설치

  ```shell
    1: cd svelte_todo
    2: npm install (or npm i)
    3: git init && git add -A && git commit -m "Initial commit" (optional)
    4: npm run dev -- --open
  ```

- tailwind 설치 / 설정

  ```shell
    npm i -D tailwindcss postcss autoprefixer daisyui
    npx tailwindcss init tailwind.config.cjs -p
  ```

- path 설정

  ```js
  // tailwind.config.cjs 수정
  module.exports = {
    content: ["./src/**/*.{html,js,svelte,ts}"], // <-- 수정부분
    theme: {
      extend: {},
    },
    plugins: [require("daisyui")], // <-- 수정
  };
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
