# 로그인 구축

-   [로그인 구축](#로그인-구축)
    -   [Spring 설정](#spring-설정)

## Spring 설정

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
    - MariaDB Driver
    - Validation
    - implementation 'io.jsonwebtoken:jjwt-api:0.11.2'
    - implementation 'io.jsonwebtoken:jjwt-jackson:0.11.2'
    - runtimeOnly 'io.jsonwebtoken:jjwt-impl:0.11.2'

    ```
    // build.gradle
    dependencies {
        implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
        implementation 'org.springframework.boot:spring-boot-starter-security'
        implementation 'org.springframework.boot:spring-boot-starter-validation'
        implementation 'org.springframework.boot:spring-boot-starter-web'
        compileOnly 'org.projectlombok:lombok:1.18.24'
        developmentOnly 'org.springframework.boot:spring-boot-devtools'
        runtimeOnly 'org.mariadb.jdbc:mariadb-java-client'
        annotationProcessor 'org.projectlombok:lombok:1.18.24'
        testImplementation 'org.springframework.boot:spring-boot-starter-test'
        testImplementation 'org.springframework.security:spring-security-test'
        implementation 'io.jsonwebtoken:jjwt-api:0.11.2'
        implementation 'io.jsonwebtoken:jjwt-jackson:0.11.2'
        runtimeOnly 'io.jsonwebtoken:jjwt-impl:0.11.2'
    }
    ```

````

4. 설정
    - application.properties -> application.yml으로 변경
        ```YML
        # DB 접속 설정
        spring:
            datasource:
                # MaridaDb 셋팅
                url: jdbc:mariadb://localhost:3306/dev
                driver-class-name: org.mariadb.jdbc.Driver
                username: yhs
                password: yhs
                # H2 DB 셋팅
                # url : jdbc:h2:mem:~/test
                # driver-class-name : org.h2.Driver
                # username : sa
            security :
                user :
                    name : user_name // 테스트시 사용할 id
                    passwrod : user_pass // 테스트시 사용할 password
        # JWT설정
        jwt:
            header: Authorization
            secret-key: 2hvcHBhLWRvbnQtYml0ZS1tZS1zcHJpbmctYm9vdC1qd3QtdGVzdC1zZWNyZXQta2V5LWNob3BwYS1kb250LWJpdGUtbWUtc3ByaW5nLWJvb3Qtand0LXRlc3Qtc2VjcmV0LWtleQo=
            token-validity-in-seconds: 86400
        ```
5. 로그인 기본 id 및 password 설정
    ```YML
    spring :
         security :
             user :
                 name : user_name
                 passwrod : user_pass
    ```
6. 접근 권한 설정

    ```Java
     package kr.co.yhs.authserver.config;

     import org.springframework.context.annotation.Bean;
     import org.springframework.context.annotation.Configuration;
     import org.springframework.security.config.annotation.web.builders.HttpSecurity;
     import org.springframework.security.config.annotation.web.configurers.AbstractHttpConfigurer;
     import org.springframework.security.web.SecurityFilterChain;

     @Configuration
     public class SecurityConfig {

         @Bean
         public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
             http.csrf(AbstractHttpConfigurer::disable).cors(t -> t.disable())
                     .formLogin(t -> t.defaultSuccessUrl("/top/login/success", true).permitAll())
                     .authorizeHttpRequests(authioriz -> authioriz
                             .requestMatchers("/all/**", "/top/**", "/*").permitAll() // 모든 사용자 접근
                             .requestMatchers("/user/**").authenticated() // 로그인 사용자 접근
                             .requestMatchers("/role/**").hasAnyRole("MANAGER", "ADMIN") // 권한이 있는 사용자 접근
                             .anyRequest().denyAll() // 나머진 모두 거절
                     )

             ;
             return http.build();
         }

     }

    ```

## NextJs 프로젝트 생성

1. 프로젝트 생성

    ```cmd
        npx create-next-app project_name

        Would you like to use TypeScript? › No / Yes(타입스크립스 사용 할꺼야? yes)
        Would you like to use ESLint? › No / Yes(eslint사용할꺼야? yes)
        Would you like to use Tailwind CSS? › No / Yes(tailwind 사용할꺼야? yes)
        Would you like to use `src/` directory? › No / Yes(src 폴더 사용할꺼야? no)
        Would you like to use App Router? (recommended) › No / Yes(app router사용할꺼야? yes)
        Would you like to customize the default import alias? › No / Yes(기본 import alias를 변경 할꺼야? no)

        cd ../project_name
        code .
    ```
````
