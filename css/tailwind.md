# TailWind

- [TailWind](#tailwind)
  - [TailWind 조건문](#tailwind-조건문)
  - [TailWind 반복 사용](#tailwind-반복-사용)
  - [TailWind 변수 활용](#tailwind-변수-활용)
  - [TailWind 컴포넌트화 하기](#tailwind-컴포넌트화-하기)

## TailWind 조건문

```html
  <div
    className={`${isImportant ? "text-red-600 text-lg font-bold" : "text-black"}`}
  ></div>
```

## TailWind 반복 사용

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

## TailWind 변수 활용

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

## TailWind 컴포넌트화 하기

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
