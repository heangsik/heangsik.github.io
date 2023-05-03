# graceful shutdown

## 목차

- [graceful shutdown](#graceful-shutdown)
  - [목차](#목차)
  - [설정](#설정)
  - [종료법](#종료법)

## 설정

- actuator추가
  ```
      --build.gradle
      dependencies{
          implementation 'org.springframework.boot:spring-boot-starter-actuator'
      }
  ```
- 설정 변경
  ```YML
  --application.yml
  server:
    shutdown: graceful
  management:
      endpoints:
          web:
            exposure:
                include: shutdown
            base-path: /xxxxxx
      endpoint:
          shutdown:
              enabled: true
  ```

## 종료법

- url
  - http://xxxx.xxxx.xxxx.xxxx:port/actuator/shutdown
  - 만약 base-path를 설정 했을시는 actuator말고 설정한 경로를 지정해야 한다.
- cmd
  - kill -15 pid
