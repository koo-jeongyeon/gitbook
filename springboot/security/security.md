# security 기본

본 내용은 최주호님의 스프링부트 시큐리티 & JWT 강의를 듣고 정리한 내용 입니다.



참고한 깃허브

직접 코드를 수정하고 실행해보면서 테스트 해보는게 이해하기 제일 좋습니다.

{% embed url="https://github.com/codingspecialist/Sringboot-Security-Basic-V1" %}

## 기본형태

제일 기본적인 형태를 보려면 Config쪽에 WebMvcConfig 만 남기고 주석처리, controller 에는 아래 / 경로만 설정해준다.

```java
@GetMapping({ "", "/" })
public @ResponseBody String index() {
   return "인덱스 페이지입니다.";
}
```

시큐리티 의존성을 설치 해주면 처음엔 기본적으로 [`http://localhost:8080/login`](http://localhost:8080/login)으로 이동했을때 로그인 페이지가 생김, application.yml 에 아래처럼 시큐리티 설정을 해주면 해당 계정으로 로그인이 가능하다.

<pre><code>spring:
<strong>  security:
</strong>    user:
      name: manager
      password: 1234
</code></pre>

로그인을 하면 / 경로로 이동한다.&#x20;



## 로그인 및 권한설정

/ 경로를 로그인한 유저만 접근가능하고, /admin 경로는 어드민만, /manager 경로는 매니저만 접근 가능하도록 권한설정을 해주는 방법

config 아래 SecurityConfig 를 생성하여 시큐리티 설정을 해준다. 원래는 /login 으로 이동하면 스프링 시큐리티가 낚아 챘는데 설정을 해주면 작동안함

```java
@Configuration // IoC 빈(bean)을 등록
@EnableWebSecurity // 필터 체인 관리 시작 어노테이션, 
public class SecurityConfig extends WebSecurityConfigurerAdapter{
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.csrf().disable();
        http.authorizeRequests()
                .antMatchers("/user/**").authenticated() //이주소로 들어오면 인증이 필요하다.
                .antMatchers("/manager/**").access("hasRole('ROLE_ADMIN') or hasRole('ROLE_MANAGER')") //이 권한이 있는 사람만 접근가능
                .antMatchers("/admin/**").access("hasRole('ROLE_ADMIN')") //이 권한이 있는 사람만 접근가능
                .anyRequest().permitAll() //저 주소가 아니면 다 권한 허용
                .and()
                .formLogin()
                .loginPage("/login")
                .loginProcessingUrl("/loginProc") //로그인 주소가 호출이 되면 시큐리티가 낚아채서 로그인을 진행해준다.
                .defaultSuccessUrl("/"); //로그인 성공시 메인페이지로 이동
    }
}
```

* @EnableWebSecurity : 스프링 시큐리티 필터가 스프링 필터 체인에 등록이 됨
* 위와 같은 방식으로 특정 경로에 권한 설정을 해줄수있다. loginPage() 설정으로 인해 권한없는 페이지로 이동시 에러페이지가 뜨는게 아니라 로그인 경로로 이동됨
* loginProcessingUrl로 인해서 시큐리티가 로그인을 대신 진행해주어 컨트롤러에 따로 만들 필요가 없다.

### 패스워드

패스워드를 꼭 암호화 해주어야 하기 때문에 SecurityConfig에 아래와 같은 코드도 설정해준다.

```java
@Bean
public BCryptPasswordEncoder encodePwd() {
   return new BCryptPasswordEncoder();
}
```

컨트롤러에서 회원가입시에 아래처럼 암호화해주면 된다.

```java
@PostMapping("/joinProc")
public String joinProc(User user) {
   System.out.println("회원가입 진행 : " + user);
   String rawPassword = user.getPassword();
   String encPassword = bCryptPasswordEncoder.encode(rawPassword);
   user.setPassword(encPassword);
   user.setRole("ROLE_USER");
   userRepository.save(user);
   return "redirect:/";
}
```

### UserDetails

* 시큐리티가 낚아채서 로그인을 진행 시킬때 로그인이 완료가 되면 시큐리티 session을 만들어낸다. 시큐리티의 세션이 존재함. (Security ContextHolder)
* 세션에 들어갈수있는 오브젝트 => Authenticaion 타입 객체로 정해져 있다.&#x20;
* Authenticaion 안에 User 정보가 있어야 됨&#x20;
* User 오브젝트 타입 => UserDetails 타입 객체로 정혀져 있다.&#x20;

`UserDetails` 를 구현하는 `PrincipalDetails` 가 필요하다.

```java
@Data
public class PrincipalDetails implements UserDetails{

	private User user;

	public PrincipalDetails(User user) {
		super();
		this.user = user;
	}
	
	@Override
	public String getPassword() {
		return user.getPassword();
	}

	@Override
	public String getUsername() {
		return user.getUsername();
	}

	//계정 만료
	@Override
	public boolean isAccountNonExpired() {
		return true;
	}

	//계정 잠김
	@Override
	public boolean isAccountNonLocked() {
		return true;
	}

	//계정 기간
	@Override
	public boolean isCredentialsNonExpired() {
		return true;
	}

	//계정 활성화 
	@Override
	public boolean isEnabled() {
		return true;
	}

	//해당 유저의 권한을 리턴하는 곳
	@Override
	public Collection<? extends GrantedAuthority> getAuthorities() {
		Collection<GrantedAuthority> collet = new ArrayList<GrantedAuthority>();
		collet.add(()->{ return user.getRole();});
		return collet;
	}
	
}
```

#### getAuthorities

`getAuthorities` 는 해당 유저의 권한을 리턴하는데 현재 유저의 권한은 role로 스트링타입이다. 근데 타입이 정해져있으니 `Collection<GrantedAuthority>` 객체를 생성해주어야한다. (ArrayList는 Collection의 자식)

#### isEnabled&#x20;

사이트에서 1년동안 로그인 안하면 휴면 계정으로 변경할때, 컬럼에 login할때마다 날짜를 저장하는 컬럼을 두고 그걸로 현재시간과 로그인 시간의 차이가 1년을 초과했을때 리턴은 false로 하면됨

### UserDetailsService

* 로그인시 스프링은 IoC컨테이너에서 UserDetailsService 빈을 찾아 loadUserByUsername을 호출함
* username 파라미터를 가져옴(클라이언트에서 넘겨준 이름 그대로)
* UserDetails 로 리턴된 값은 Authentication 내부로 들어감. 그리고 그 객체는 또 세션으로 들어가니 다음과 같은 모양새가 됨. `시큐리티 session(내부 Authentication(내부 UserDetails))`

```java
// 시큐리티 설정에서 loginProcessingUrl 걸어놨기 때문에 로그인 요청이 오면 자동으로
// UserDetailsService 타입으로 IoC되어있는 loadUserByUsername 함수가 실행됨 (규칙임)
@Service
public class PrincipalDetailsService implements UserDetailsService{

   @Autowired
   private UserRepository userRepository;

   //알아서 다해줌 
   @Override
   public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
      User user = userRepository.findByUsername(username);
      if(user == null) {
         return null;
      }
      return new PrincipalDetails(user);
   }

}
```

### 특정주소에 직접 권한걸기

SecurityConfig 에 아래와같은 어노테이션을 걸어주면 특정어노테이션이 활성화가 된다.

securedEnabled는 @Secured 를 활성화 하고

prePostEnabled는 @PreAuthorize 와 @PostAuthorize 를 활성화 시킴

```java
@EnableGlobalMethodSecurity(prePostEnabled = true, securedEnabled = true) // 특정 주소 접근시 권한 및 인증을 위한 어노테이션 활성화
public class SecurityConfig extends WebSecurityConfigurerAdapter{
```



그리고 아래와 같이 사용한다.

```java
//권한 하나만 걸고싶으면 secured
@Secured("ROLE_ADMIN")
@GetMapping("/info")
public @ResponseBody String info() {
   return "개인정보";
}

//권한 여러개를 걸고싶으면 preAuthorize
@PreAuthorize("hasRole('ROLE_MANAGER') or hasRole('ROLE_ADMIN')") //data메서드 실행되기 전 실행
@PostAuthorize("hasRole('ROLE_MANAGER') or hasRole('ROLE_ADMIN')") //data메서드 실행후에 실행됨, 잘 안씀
@GetMapping("/info")
public @ResponseBody String data() {
   return "데이터 정보";
}
```



## websecurityconfigureradapter deprecated 문제

이 문제로 인해 SecurityConfig를 다르게 변경해주어야한다.



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
            .httpBasic().disable() //기본인증방식 - 보안에 안좋음
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

   // corsConfig : @CrossOrigin 같은 어노테이션을 걸어주는건 인증이 없을때, 인증이 있을땐 필터에 아래처럼 등록해줘야한다.
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

}j
```



