# H2 DB CONSOLE

스프링 시큐리티 작업시 잘 접속 되던 콘솔이 403오류가 난다.
    ![403](../image/2022-10-28%2010%2001%2045.jpg)  
이는 스프링 시큐리티가 콘솔 URL을 접근제어 했기에 발생을 한다.

이경우 해당 URL을 제외 시켜줘야 한다.

```Java
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http.authorizeRequests()
                .mvcMatchers("/h2-console/**").permitAll() // 모든 사용자 접근가능
        http.csrf().disable(); // 로컬인 경우만 해야 한다 운여에 올라가면 난리난다.
        http.headers().frameOptions().disable(); // 프레임도 풀어줘야 한다. 운영에 가면 안된다.
        return http.build();
    }
```

테스트를 해보니
>http.csrf().disable();  
>http.headers().frameOptions().disable();  

이 두가지만 해줘도 되는듯 하다.
