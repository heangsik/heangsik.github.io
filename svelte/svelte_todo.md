# SVELTE Todo
``
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

# TailWind 조건문

```html
  <div
    className={`${isImportant ? "text-red-600 text-lg font-bold" : "text-black"}`}
  ></div>
```

# TailWind 반복 사용

```css
  <style>
    .btn {
      @apply py-2 px-4 font-semibold rounded-lg shadow-md;
    }
    .btn-green {
      @apply text-white bg-green-500 hover:bg-green-700;
    }
    .btn-blue {
      @apply text-white bg-blue-500 hover:bg-blue-700;
    }
  </style>
```

# TailWind 변수 활용

```js
const colors = {
  red: "text-red-600",
  black: "text-black",
};
const textSizes = {
  sm: "text-sm",
  md: "text-md",
  lg: "text-lg",
};
const coloredText = (color, size) => {
  return `${colors[color]} ${textSizes[size]}`;
};
```

```html
<h1 className="{isImpotant" ? coloredText(red, lg) : coloredText(black, sm)}>
  This is Title
</h1>
;
```

# TailWind 컴포넌트화 하기

```js
const colors = {
  red: "text-red-600",
  black: "text-black",
};

const textSizes = {
  sm: "text-sm",
  md: "text-md",
  lg: "text-lg",
};

const Button = ({ color, size, children }) => {
  let textColor = colors[color];
  let textSize = textSizes[size];

  return <button className={`${textColor} ${textSize}`}>{children}</button>;
};
export default Button;
```

## AdorableCSS 설치

- npm i -D adorable-css
- vite.config.js 수정

  ```js
  import {adorableCSS} from "adorable-css/vite" // <-추가

  export default defineConfig({
    plugins: [adorableCSS(), ...] // <- plugin을 맨 처음에 등록합니다.
  })
  ```

- \+layout.svelte 수정

- ```TypeScript
  <script>
  	import Header from './Header.svelte';
  	import './styles.css';
  	import '@adorable.css'; //<- 추가
  </script>
  ```

## CSS Animation ShutCmd

```Css


	@keyframes SlideSpin {
		from {
			transform: rotate(0deg);
		}
		to {
			transform: rotate(360deg);
		}
	}
  .ani_span{/*숏컷 cmd 순서*/
    animation: bounce 1s linear 2s infinite alternate ;
    /*
    animation-name: SlideSpin; //키프레임 이름
    animation-duration: 5s; //에니메이션 진행 시간
    animation-timing-function: linear;//움직이는 방법
    animation-delay: 2s; //에니메이션 진행 전 대기 시간
    animation-iteration-count: infinite;// 작동 횟수
    animation-direction: alternate;//에니메이션 움직이는 방향


    ease - 느리게 -> 빠르게 -> 느리게(기본설정)
    linear - 항상 같은 속도
    ease-in - 느리게 -> 빠르게
    ease-out - 빠르게 -> 느리게
    ease-in-out - 느리게 -> 빠르게 -> 느리게
    cubic-bezier(n,n,n,n) - 사용자 시정
    */
  }
```
