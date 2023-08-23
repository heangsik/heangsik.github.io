# Spring Boot Security JWT

## 권한 설정
-  authenticated() ;  인증된 사용자의 접근을 허용
-  fullyAuthenticated(): 인증된 사용자의 접근을 허용,  rememberMe인증 제외
-  permitAll(): 무조건 허용
-  denyAll(): 무조건 차단
-  anonymous(): 익명사용자 허용
-  rememberMe(): rememberMe 인증 사용자 접근 허용
-  access(String): 주어진 SpEL표현식의 평가 결과가 true 이면 접근허용
-  hasRole(String): 사용자가 주어진 역할이 있다면 접근을 허용
-  hasAuthority(String): 사용자가 주어진 권한이 있다면 허용
-  hasAnyRole(String...): 사용자가 주어진 어떤권한이라도 있으면 허용
-  hasAnyAuthority(String...): 사용자가 주어진 권한중 어떤 것이라도 있다면 허용
-  hasIpAddress(String): 주어진 IP로 부터 요청이 왔다면 접근을 허용


## WebMvcTest
- 시프링 시큐리티가 적용되어 있는 상테에서 테스트를 진행시 401, 403 오류가 발생된다.
- 원인은 스프링 SecurityFilterChain을 아무리 잘 설정 하여더라도 WebMvcTest를 진행시 스프링에서 기본으로 제공되는 정보가 로드되어 설정한 정보가 적용되질 않는다.
- 해결법
    -  @WithMockUser(username = "user", roles = {"USER"}) 또는 @WithMockUser를 적용하여 유저의 권한을 가상으로 준다.
    - mockMvc.perform(post("/api/v1/user/join").**with(csrf())** 를 하여 csrf를 패스 시켜야 한다.
    - 만약 WithMockUser가 적용이 불가능 하다면 build.gradle에 testImplementation 'org.springframework.security:spring-security-test'를 추가한다.
