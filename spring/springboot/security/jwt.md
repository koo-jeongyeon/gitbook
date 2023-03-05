# JWT

JWT 토큰으로 로그인

* 로그인시 원래는 localhost:8080/login 을 호출하면 스프링 시큐리티가 알아서 UserDetailsService 빈을 찾아 loadUserByUsername을 호출하는데 filterChain 에서 formLogin을 disable하고 커스텀 필터를 등록해서 로그인을 할것이다.&#x20;

#### SecurityConfig.java

security 기본에 적어놓은 config와 같다

```java
@Configuration
@EnableWebSecurity // 시큐리티 활성화 -> 기본 스프링 필터체인에 등록
public class SecurityConfig {

   @Autowired
   private UserRepository userRepository;

   @Autowired
   private CorsConfig corsConfig;

   @Bean
   SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
      return http
            .csrf().disable()
            .sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS)
            .and()
            .formLogin().disable()
            .httpBasic().disable()
            .apply(new MyCustomDsl()) // 커스텀 필터 등록
            .and()
            .authorizeRequests(authroize -> authroize.antMatchers("/api/v1/user/**")
                  .access("hasRole('ROLE_USER') or hasRole('ROLE_MANAGER') or hasRole('ROLE_ADMIN')")
                  .antMatchers("/api/v1/manager/**")
                  .access("hasRole('ROLE_MANAGER') or hasRole('ROLE_ADMIN')")
                  .antMatchers("/api/v1/admin/**")
                  .access("hasRole('ROLE_ADMIN')")
                  .anyRequest().permitAll())
            .build();
   }

   public class MyCustomDsl extends AbstractHttpConfigurer<MyCustomDsl, HttpSecurity> {
      @Override
      public void configure(HttpSecurity http) throws Exception {
         AuthenticationManager authenticationManager = http.getSharedObject(AuthenticationManager.class);
         http
               .addFilter(corsConfig.corsFilter())
               .addFilter(new JwtAuthenticationFilter(authenticationManager))
               .addFilter(new JwtAuthorizationFilter(authenticationManager, userRepository));
      }
   }

}
```



#### JwtAutenticationFilter.java

* 로그인시 실행되는 필터를 상속받은 필터를 만들어줘서 SecurityConfig 에 등록해줘야함
* UsernamePasswordAuthenticationFilter는 AuthenticationManager가 매개변수로 필요함. 위의 SecurityConfig.java의 필터를 등록하는 부분을 보면 AuthenticationManager 객체를 생성해서 넘겨주는걸 볼 수 있음
* PrincipalDetailsService 와 PrincipalDetails는 security 기본에 작성되어 있음, 생성해줘야함

```java

//스프링 시큐리티에서 UsernamePasswordAuthenticationFilter 가 있음
// /login 요청해서 username,password 전송하면 이 필터가 동작을 함, 근데 formlogin을 disable해놔서 지금 자동으로 작동안함
//그래서 필터를 상속받아 만들어주고 securityconfig에 등록시켜 주는것임
@RequiredArgsConstructor
public class JwtAuthenticationFilter extends UsernamePasswordAuthenticationFilter{

   private final AuthenticationManager authenticationManager;
   
   // Authentication 객체 만들어서 리턴 => 의존 : AuthenticationManager
   // 인증 요청시에 실행되는 함수 => /login (로그인시도시 실행됨)
   @Override
   public Authentication attemptAuthentication(HttpServletRequest request, HttpServletResponse response)
         throws AuthenticationException {
      
      System.out.println("JwtAuthenticationFilter : 진입");
      /*
      * 1. username,password 받아서
      * 2. 정상인지 로그인 시도를 함. authenticationManager로 로그인 시도를 하면 PrincipalDetailsService가 호출됨
      * 3. loadUserByUsername 함수 실행됨
      * 4. PrincipalDetail를 세션에 담고 -> 세션에 안담아주면 권한 관리가 안됨
      * 5. JWT토큰을 만들어서 응답
      */
      // request에 있는 username과 password를 파싱해서 자바 Object로 받기
      ObjectMapper om = new ObjectMapper();
      LoginRequestDto loginRequestDto = null;
      try {
         loginRequestDto = om.readValue(request.getInputStream(), LoginRequestDto.class);
      } catch (Exception e) {
         e.printStackTrace();
      }
      
      System.out.println("JwtAuthenticationFilter : "+loginRequestDto);
      
      // 유저네임패스워드 토큰 생성
      UsernamePasswordAuthenticationToken authenticationToken = 
            new UsernamePasswordAuthenticationToken(
                  loginRequestDto.getUsername(), 
                  loginRequestDto.getPassword());
      
      System.out.println("JwtAuthenticationFilter : 토큰생성완료");
      
      // authenticate() 함수가 호출 되면 인증 프로바이더가 유저 디테일 서비스의
      // loadUserByUsername(토큰의 첫번째 파라메터) 를 호출하고
      // UserDetails를 리턴받아서 토큰의 두번째 파라메터(credential)과
      // UserDetails(DB값)의 getPassword()함수로 비교해서 동일하면
      // Authentication 객체를 만들어서 필터체인으로 리턴해준다.
      
      // Tip: 인증 프로바이더의 디폴트 서비스는 UserDetailsService 타입
      // Tip: 인증 프로바이더의 디폴트 암호화 방식은 BCryptPasswordEncoder
      // 결론은 인증 프로바이더에게 알려줄 필요가 없음.
      Authentication authentication = 
            authenticationManager.authenticate(authenticationToken);
      
      PrincipalDetails principalDetailis = (PrincipalDetails) authentication.getPrincipal();
      System.out.println("Authentication : "+principalDetailis.getUser().getUsername());
      //authentication객체가 session영역에 저장을 해야하고 그 방법이 return 해주면됨
      //그 이유는 권한 관리를 시큐리티가 대신 해주기 때문에 편하려고 하는거임
      //굳이 JWT 토큰을 사용하면서 세션을 만들 이유가 없지만 단지 권한처리 때문에 세션에 넣어주는것
      return authentication;
   }

   //위의 함수가 실행이 종료되면(인증이 정상으로 되면) 이어서 successfulAuthentication가 실행됨
   //JWT토큰을 만들어서 requset요청한 사용자에게 JWT토큰을 reponse해줌
   // JWT Token 생성해서 response에 담아주기
   @Override
   protected void successfulAuthentication(HttpServletRequest request, HttpServletResponse response, FilterChain chain,
         Authentication authResult) throws IOException, ServletException {
      
      PrincipalDetails principalDetailis = (PrincipalDetails) authResult.getPrincipal();
      
      //RSA방식은 아니고 Hash암호 방식
      String jwtToken = JWT.create()
            .withSubject(principalDetailis.getUsername()) //크게 중요하지 않음
            .withExpiresAt(new Date(System.currentTimeMillis()+JwtProperties.EXPIRATION_TIME)) //언제까지 유효할지 만료시간
            .withClaim("id", principalDetailis.getUser().getId())
            .withClaim("username", principalDetailis.getUser().getUsername())
            .sign(Algorithm.HMAC512(JwtProperties.SECRET));
      
      response.addHeader(JwtProperties.HEADER_STRING, JwtProperties.TOKEN_PREFIX+jwtToken);
   }
   
}
```

#### JwtProperties.java

```java
public interface JwtProperties {
   String SECRET = "cos"; // 우리 서버만 알고 있는 비밀값
   int EXPIRATION_TIME = 864000000; // 10일 (1/1000초)
   String TOKEN_PREFIX = "Bearer ";
   String HEADER_STRING = "Authorization";
}
```
