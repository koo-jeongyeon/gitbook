# security 기본

본 내용은 최주호님의 스프링부트 시큐리티 & JWT 강의를 듣고 정리한 내용 입니다.



참고한 깃허브

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



## 권한설정

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









