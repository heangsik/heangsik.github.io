# Spring 설정
1. start.spring.io 접속
2. 세부 설정 입력
   ![Project](../image/2022-09-29%2013%2038%2052.jpg)
3. 의존성 추가
   ![Dependencies](../image/2022-09-29%2013%2043%2027.jpg)
   - Spring Boot DevTools
   - Lombok
   - Spring Web
   - Spring Security
   - Spring Data JPA
   - H2 Database
4. 설정 정보 추가
    - application.properties -> application.yml으로 변경
        ``` YML
        spring :
            datasource :
                url : jdbc:h2:mem:~/test
                driver-class-name : org.h2.Driver
                username : sa
        ```

# Vue 설정
1. vue create project_name <-- 입력
2. Manually select features <-- 선택
3. Babel, Router, Vuex <- 선택
4. Use history mode for router? (Requires proper server setup for index fallback in production) <-- enter
5.  Where do you prefer placing config for Babel, ESLint, etc.? <-- In package.json 선택
6.   Save this as a preset for future projects? (y/N) <-- enter

# Pina 설치
1. cd project_name
2. npm i pinia 
3. npm i axios <-- 통신 모듈


