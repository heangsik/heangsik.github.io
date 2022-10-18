# Vuex

## 목차
1. [설치](#설치)
2. [사용](#사용)
---
## 설치
``` VueCli
vue create 프로젝트_이름
Manually select features <-- 선택
Babel, Router, Vuex <- 선택
3.x <-- 선택  
Use history mode for router? (Requires proper server setup for index fallback in production)(Y/n) <- Y   
In package.json <- 선택   
Save this as a preset for future projects? (y/N) <- N
```
위의 순서대로 프로젝트를 vue cli를 사용하여 셋팅한다.

---
## 사용
1. store/index.js 파일 생성
```JavaScript
import { createStore } from 'vuex'

export default createStore({
  state: {
  },
  getters: {
  },
  mutations: {
  },
  actions: {
  },
  modules: {
  }
})
```
2. main.js 수정
```JavaScript
import store from '@/store/index.js' <- 추가
createApp(App).use(store).mount('#app') <-- use(store) 추가
```
3. 사용
```JavaScript
// store import
import { useStore } from 'vuex'
// store 선언
const store = useStore();
const loginAction = ()=>{
            store.dispatch('actons_name', {a:'aaa', b:'bbb'});
        };

console.log('stat  접근 :', store.state.xxxxxx);
console.log('getter 접근 :', store.getters.xxxxxx);

const computed_sample = computed(()=>store.getters.xxxxxx);

// watch sample
watch(computed_sample, (newValue, beforValue) =>{console.log("watching sample : ", computed_sample.value)});

```