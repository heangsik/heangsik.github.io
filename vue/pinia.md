![pinia](https://pinia.vuejs.org/logo.svg)

## PINIA

    [출처](https://velog.io/@sehyunny/advantages-of-pinia-vs-vuex)   
    [공홈](https://pinia.vuejs.org/)
---

## 목차

- [PINIA](#pinia)
- [목차](#목차)
- [설치](#설치)
- [main.js 업데이트](#mainjs-업데이트)
- [store 파일 생성](#store-파일-생성)
- [사용](#사용)


---

## 설치

```shell
    yarn add pinia
    or 
    npm install pinia
```

## main.js 업데이트

```JavaScript
    import { createApp } from "vue";
    import { createPinia } from "pinia";
    import App from "./App.vue";
    const app = createApp(App);
    app.use(createPinia());
    app.mount("#app");
```

## store 파일 생성

```JavaScript
    import { defineStore } from "pinia";

    export const useCounterStore = defineStore("counter", {
        state: () => {
            return { count: 0 };
        },
        actions: {
            increment(value = 1) {
                this.count += value;
            },
        },
        getters: {
            doubleCount: (state) => {
                return state.count * 2;
            },
            doublePlusOne() {
                return this.doubleCount + 1
            },
        },
    });
```

## 사용

```JavaScript
    <script setup>
        import { useCounterStore } from "./stores/counter";
        const store = useCounterStore();
    </script>
    <template>
        <h1>Count is {{ store.count }}</h1>
        <h2>Double is {{ store.doubleCount }}</h2>
        <button @click="store.increment()">Increment</button>
    </template>
```

    #PINIA, #Vuex
