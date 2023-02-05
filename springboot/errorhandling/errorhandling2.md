# Security & JWT 에러처리

생성일: 2022년 12월 1일 오후 2:14

Security

인증 Authentication - 인증 하는거

인가 Authorization  - 권한 주는거

에러처리시 Security에서 커스텀 하는 클래스들

* AccessDeniedHandler - 권한체크하고 권한 없으면 동작 - 권한
* AuthenticationEntryPoint - 인증안된 유저가 요청했을때 - 인증

개발자가 커스텀 예외처리 하는건 스프링 영역임

But 시큐리티는 스프링 이전에 필터링 하고 DispatcherServlet 에 들어옴

그래서 exception 마다 자세히 에러처리해줌

본인은 인증만 구현함

* 시큐리티 에러 종류

```
/**
 * Security exception종류
* UsernameNotFoundException :계정 없음
* InternalAuthenticationServiceException :존재하지 않는 아이디
* InsufficientAuthenticationException :자격증명 신뢰할 수 잆음(jwt)
 * BadCredentialsException :비밀번호 불일치(현재email, pw불일치시 실행됨)
 * AccountExpiredException :계정만료
* CredentialsExpiredException :비밀번호 만료
* DisabledException :계정 비활성화
* LockedException :계정 잠김
*/
```

* `AuthenticationEntryPoint` 를 구현한 시큐리티 에러 처리 `commence` 오버라이드
* `ApiException` 의 경우 참고 [에러 핸들링 **ExceptionHandler**](%E1%84%8B%E1%85%A6%E1%84%85%E1%85%A5%20%E1%84%92%E1%85%A2%E1%86%AB%E1%84%83%E1%85%B3%E1%86%AF%E1%84%85%E1%85%B5%E1%86%BC%20ExceptionHandler%2015217a94272a422e82e413221009a8b2.md)

```java
@Override
    public void commence(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse,
                         AuthenticationException e) throws IOException {

        if (e.getClass().equals(BadCredentialsException.class)) {

            ApiException ex = new ApiException(ErrorCode.ERROR_USER_LOGIN_INVALID);
            HashMap<String, String> errors = apiExceptionHandler.handleException(ex, httpServletResponse);
            log.error("Unauthorized error: {}",errors);

        }

				.....(생략)

}
```

* handleException 메서드
* `@ExceptionHandler` 같은걸로 처리되지 않아서 `HttpServletResponse` 를 활용해 응답을 보냄
* 참고

[Handle spring security authentication exceptions with @ExceptionHandler](https://stackoverflow.com/questions/19767267/handle-spring-security-authentication-exceptions-with-exceptionhandler)

```java
/**
     * Security&JWT Filter Exception
     */
    public HashMap<String, String> handleException(ApiException ex, HttpServletResponse res) throws IOException {

        res.setContentType("application/json");
        HashMap<String, String> errors = new HashMap<>();
        errors.put("error_code", ex.getErrorCode().getErrorCode()+"");
        errors.put("error_user_msg", ex.getErrorCode().name());
        res.getWriter().println(
                "{ \"error_code\": \"" + ex.getErrorCode().getErrorCode()+"" + "\""
                +", \"error_user_msg\": \"" + ex.getErrorCode().name() + "\" }");

        return errors;
    }
```

JWT 에러 종류

```
/**
 * MalformedJwtException, SignatureException :잘못된 구성의 토큰
* ExpiredJwtException :만료된 토큰
* UnsupportedJwtException :지원하지 않는 포멧의 토큰
*/
```

* 여기있는게 다는 아님 필요해보이는것만 일단 검색해서 사용했음

```java
@Override
protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain)
        throws ServletException, IOException {
    try {
        Authentication authentication = tokenProvider.getAuthentication(request);
        SecurityContextHolder.getContext().setAuthentication(authentication);

        filterChain.doFilter(request, response);
    } catch (JwtException e) {
        ApiException ex = new ApiException(ErrorCode.ERROR_USER_TOKEN);
        HashMap<String, String> errors = apiExceptionHandler.handleException(ex, response);
log.error("jwt error: {} : {}", errors ,e.getMessage());
    }
}
```

* `OncePerRequestFilter` 를 구현하고 `doFilterInternal` 를 오버라이드
* 인증과 같은 방식, `handleException` 사용
* `getAuthentication` 메서드는 `TokenProvider` 클래스를 만들고 토큰을 파싱해 유저정보를 가져옴

```java
public String getAuthenticationUser(String token) {
    try {
        return Jwts.parser()
                .setSigningKey(jwtSecret)
                .parseClaimsJws(trimToken(token))
                .getBody()
                .getSubject();
    } catch (MalformedJwtException | SignatureException e) {
log.error("{} : {}", ErrorCode.ERROR_USER_TOKEN_INVALID.getErrorCode(),
                ErrorCode.ERROR_USER_TOKEN_INVALID.getLogMsg());

    } catch (ExpiredJwtException e) {
log.error("{} : {}", ErrorCode.ERROR_USER_TOKEN_EXPIRED.getErrorCode(),
                ErrorCode.ERROR_USER_TOKEN_EXPIRED.getLogMsg());

    } catch (UnsupportedJwtException e) {
log.error("{} : {}", ErrorCode.ERROR_USER_TOKEN_SUPPORT.getErrorCode(),
                ErrorCode.ERROR_USER_TOKEN_SUPPORT.getLogMsg());

    }
    return null;
}

public Authentication getAuthentication(HttpServletRequest request) {
    String token = request.getHeader(HEADER_STRING);
    if (token != null) {
        // parse the token.
        String user = getAuthenticationUser(token);
        return user != null ? new UsernamePasswordAuthenticationToken(user, null,emptyList()) : null;
    }
    return null;
}
```

* 토큰 생성이나 get 등 토큰에 관련된 메서드를 다 여기다 만듬, 때문에 여기서 예외처리해줌

```java
JWTAuthenticationFilter authFilter = new JWTAuthenticationFilter(this.tokenProvider);

http
 ...
		.exceptionHandling().authenticationEntryPoint(unauthorizedHandler)
		.addFilterBefore(authFilter, UsernamePasswordAuthenticationFilter.class);
```

* `WebSecurityConfigurerAdapter` 를 구현한 시큐리티 config 파일의 설정
* 인증관련 에러 핸들링 넣어주고
* UsernamePasswordAuthenticationFilter 는 인증관련 클래스인데 (아이디 비번 체크함) 인증전에 토큰관련된 예외처리 먼저한다는 거임

### 개발시 문제 상황

헬스체크를 하는 데 http 로 ‘/’ 경로로 헬스체크했음, 시큐리티 인증 에러가남, beanstalk 이 http로 헬스체크해서 해결해야했음

그래서 에러의 이유를 찾아봄

* json 이 아니라 html 로 요청 (/) 했을때 에러가남 -> 뷰리졸버를 찾는데 없어서 에러가 나고 -> /error 요청 -> resources, static 등 아래 정적파일 찾는데 -> 이것들에 관해 인증 허용이 안되서 (에러페이지에 인증X) 인증에러가남

`WebSecurityConfigurerAdapter` 를 구현한 클래스에 정적파일 접근 권한 줌

```java
/*
     * resources, static 아래 파일들의 접근권한 설정
     */
    @Override
    public void configure(WebSecurity web) throws Exception {
        web.ignoring()
                .requestMatchers(
                        PathRequest.toStaticResources().atCommonLocations()
                );
    }
```

에러페이지 404 도 접근허용 해줌

```java
@Override
    protected void configure(HttpSecurity http) throws Exception {
        JWTAuthenticationFilter authFilter = new JWTAuthenticationFilter(this.tokenProvider);

        http
                .cors()
                .and()
                .csrf().disable() // Restful API 방식 사용으로 disable 처리
                .exceptionHandling().authenticationEntryPoint(unauthorizedHandler)
                .and()
                .sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS)
                .and()
                .authorizeRequests()
                .antMatchers("/",
                        ...
                        "/404"
                        ).permitAll()
                .antMatchers(HttpMethod.OPTIONS).permitAll()
                .anyRequest().authenticated()
                .and()
                .addFilterBefore(authFilter, UsernamePasswordAuthenticationFilter.class);
    }
```

참고

[https://imbf.github.io/spring/2020/06/29/Spring-Security-with-JWT.html](https://imbf.github.io/spring/2020/06/29/Spring-Security-with-JWT.html)

[https://hou27.tistory.com/entry/Spring-Security-JWT](https://hou27.tistory.com/entry/Spring-Security-JWT)

[https://devlog-wjdrbs96.tistory.com/429](https://devlog-wjdrbs96.tistory.com/429)

* CORS

[https://atoz-develop.tistory.com/entry/Spring-BootSpring-Web-MVC-ViewController%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%B4%EC%84%9C-%EB%B7%B0-%EB%A7%A4%ED%95%91%ED%95%98%EA%B8%B0](https://atoz-develop.tistory.com/entry/Spring-BootSpring-Web-MVC-ViewController%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%B4%EC%84%9C-%EB%B7%B0-%EB%A7%A4%ED%95%91%ED%95%98%EA%B8%B0)

* WebMvcConfig 는 시큐리티 필터가 먼저 막음,,, 그래서 필터에서 허용해줘야됨

[https://csy7792.tistory.com/243](https://csy7792.tistory.com/243)

* csrf 도 잘 설정해줘야한다고함 Spring Security csrf token 검색 ㄱ

[https://hou27.tistory.com/entry/Spring-Security-JWT](https://hou27.tistory.com/entry/Spring-Security-JWT)

[https://spring.io/blog/2022/02/21/spring-security-without-the-websecurityconfigureradapter](https://spring.io/blog/2022/02/21/spring-security-without-the-websecurityconfigureradapter)

[https://kimchanjung.github.io/programming/2020/07/02/spring-security-02/](https://kimchanjung.github.io/programming/2020/07/02/spring-security-02/)
