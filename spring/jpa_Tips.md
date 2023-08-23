# JPA 팁들

### @NotNull @NotEmpty @NotBlank 차이

-   @NotNull
    -   null을 허용하지 않음
    -   "", " "는 허용함
-   @NotEmpty
    -   null, ""를 허용하지 않음
    -   " " 허용
-   @NotBlank
    -   null, "", " "셋다 허용하지 않음
-   비교정리
    | 타입      | 미허용        | 허용    |
    | --------- | ------------- | ------- |
    | @NotNull  | null          | "", " " |
    | @NotEmpty | null, ""      | " "     |
    | @NotBlank | null, "", " " | 없음    |

---


### Sql Logging
- Spring Boot 3에서는 아래와 같이 한다.
    ```YML
    spring:
    jpa:
        properties:
        hibernate:
            format_sql: true
    logging:
    level:
        org.hibernate.SQL: debug
        org.hibernate.orm.jdbc.bind: trace

    ```
    - 3버젼으로 업데이트 되면서 로깅 방법이 변경되었다.