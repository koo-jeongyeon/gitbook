# filter



기본적인 필터구현에 관한 내용

아래처럼 필터를 구현하면 doFilter 를 오버라이드한다.&#x20;

필터에 걸려서 프로그램이 끝나지 않도록 filterChain.doFilter 로 리퀘스트와 리스펀스를 넘겨주어야한다.&#x20;

```java
import javax.servlet.*;
import java.io.IOException;

public class myfilter implements Filter {

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) 
            throws IOException, ServletException {
        System.out.println("filter");
        filterChain.doFilter(servletRequest,servletResponse);
    }
}
```



해당 필터를 시큐리티에도 아래와 같은 방식으로 등록할 수도 있지만

```java
@Bean
SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
   return http
         .addFilterBefore(new myfilter(), BasicAuthenticationFilter.class)
         //필터 등록하였음, BasicAuthenticationFilter보다 먼저 동작하도록 설정한것임
```



따로 클래스를 만들어서 필터를 관리할 수도 있다.

아래처럼 필터의 순서를 정하여 여러개로 등록이 가능하다.

```java
@Configuration
public class FilterConfig {

    @Bean
    public FilterRegistrationBean<myfilter> filter(){
        FilterRegistrationBean<myfilter> bean = new FilterRegistrationBean<>(new myfilter());
        bean.addUrlPatterns("/*");
        bean.setOrder(0); //낮은 번호가 필터중에서 가장먼저 실행됨
        return bean;
    }
}
```



다만, **FilterConfig는 시큐리티에 건 필터보다 이후**에 실행된다.



원래 일반적인 웹환경에서 브라우저가 서버에게 요청을 보내면 DispatcherServlet이 요청을 받기 이전에 서블릿 필터를 거치게 된다. 스프링 시큐리티도 서블릿 필터로 작동하여 인증,권한처리를 진행한다.

시큐리티와 관련한 서블릿 필터는 실제로 연결된 여러 필터들로 구성되어있어 이런 모습때문에 체인이라는 표현을 쓴다. 해당 필터의 역할과 흐름을 알아야 필터를 커스터 마이징 가능하다.

SecurityFilterChain 의 구조 [https://siyoon210.tistory.com/32](https://siyoon210.tistory.com/32)

<figure><img src="https://slack-imgs.com/?c=1&#x26;url=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F226F4C3A589AA1871D" alt=""><figcaption></figcaption></figure>

