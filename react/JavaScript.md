# JavaScript Tips

-   [JavaScript Tips](#javascript-tips)
    -   [1. 지정이 안되어 있을때 기본값 지정](#1-지정이-안되어-있을때-기본값-지정)
    -   [2. Event Bubbling 처리](#2-event-bubbling-처리)

### 1. 지정이 안되어 있을때 기본값 지정

```JS
config = {min:0};

config.min ??= 10;
conifg.max ??= 200;

console.log(config);

{min:0, max:200}
```

---

### 2. Event Bubbling 처리

```JS
  e.preventDefault();
```

---
