# Log4J2 설정

## 1. 목차

- [Log4J2 설정](#log4j2-설정)
  - [1. 목차](#1-목차)
  - [2. 의존성 설정](#2-의존성-설정)
  - [3. 스프링 설정 변경](#3-스프링-설정-변경)
  - [4. Log4J2설정파일 생성](#4-log4j2설정파일-생성)
  - [5. Log설정 외부로 빼기](#5-log설정-외부로-빼기)

## 2. 의존성 설정

- 기본 logback 의존성을 제거한다.
  ```
  // build.gradle
  configurations {
      all {
          exclude group: 'org.springframework.boot', module: 'spring-boot-starter-logging'
      }
  }
  ```
- log4j2의존성 추가
  ```
  // build.gradle
  dependencies {
      implementation 'org.springframework.boot:spring-boot-starter-log4j2'
      implementation 'com.fasterxml.jackson.dataformat:jackson-dataformat-yaml'
  }
  ```
  - log4j2설정 파일을 yml로 사용하는경우 jasckson-dataformat-yaml을 넣어줘야 한다. xml이나 properties로 설정을 할때는 필요 없다.

## 3. 스프링 설정 변경

- applicatoin.yml
  ```YML
  logging:
    config: classpath:log4j2_local.yml <-- log4j2설정 파일
  ```

## 4. Log4J2설정파일 생성

- log4j2_local.yml 생성

  ```YML
  Configuration:
    name: logger_name
    Properties:
        Property:
            - name: log_path
              value: "log file path"
            - name: log_file_name
              value: "log file name"
            - name: log_pattern
              value: "log pattern"

    Appenders:
        Console:
            name: console_appender
            target: SYSTEM_OUT
            PatternLayout:
                disableAnsi: false # 콘솔 로그 찍을때 ansi값을 표기 한다. 기본으 true여서 색상이 안보인다.
                pattern: ${log_pattern} # 위에 property에 선언된 값이다.
        RollingFile:
            PatternLayout:
                pattern: ${log_pattern}
            fileName: ${log_path}/${log_file_name}
            filePattern: ${log_path}/%d{YYYYmm}/${log_file_name}.%d{YYYYmmDD}_%i
            name: file_appender
            Policies:
                SizeBasedTriggeringPolicy:
                    Size: 10KB # 파일이 지정된 사이즈가 될때 변경된다.
                TimeBasedTriggeringPolicy:
                    interval: 1 # 파일 패턴에 %i값이 1씩 증가한다.

    Loggers:
        Root:
            level: info
            AppenderRef:
                - ref: console_appender
                - ref: file_appender
        Logger:
            - name: kr.co.xxx # 로깅을 희망하는 패키지 명을 넣는다.
              level: info
              additivity: false # 로그 이벤트를 상위로 보내는 옵션이다 true면 여러번 로그가 찍힐수 있다.
            AppenderRef:
                - ref: console_appender
                - ref: file_appender
  ```

## 5. Log설정 외부로 빼기

- 빌드 후 설정 파일 외부에서 설정하기
  ```
  java -jar api.jar --spring.config.location=file:./application.yml
  ```
- application.yml에서 변경하기
  ```YML
  logging:
    config: file:./log4j2_local.yml <-- log4j2설정 파일
  ```
- 구동시 vm값드로 넣기

  ```CMD
  -- windows --
  java -jar api.jar --spring.config.location=file:./application.yml --logging.config=file:./log4j2_local.yml

  -- linux --
  java -jar api.jar -Dspring.config.location=file:./application.yml -Dlogging.config=file:./log4j2_local.yml
  ```

  applicaion.yml에서 logging.config값은 삭제 해야한다.
