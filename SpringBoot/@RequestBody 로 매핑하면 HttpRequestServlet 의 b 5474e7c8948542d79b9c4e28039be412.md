# @RequestBody 로 매핑하면 HttpRequestServlet 의 body 값 비워지는 이유, 해결

생성일: 2022년 12월 7일 오전 11:30

[Controller에서 HttpRequest Body 값은 왜 비워져 있을까?](https://velog.io/@saint6839/Controller%EC%97%90%EC%84%9C-HttpRequest-Body-%EA%B0%92%EC%9D%80-%EC%99%9C-%EB%B9%84%EC%9B%8C%EC%A0%B8-%EC%9E%88%EC%9D%84%EA%B9%8C)

- 간단한 flow

request (요청)

-> 디스패처 서블릿 

-> 컨트롤러의 객체로 값을 바인딩 하는 과정에서 바디 데이터 소비 

-> 컨트롤러의 request body 비워져 있음

- request.getReader()에서 InputStream을 생성하는데, 이걸 tomcat에서 한번만 사용할 수 있도록 막아두어서, 한번 read한 body값은 다시 읽을 수 없게 되어 있었다.

1. `HttpServletRequestWrapper` 상속받아 클래스 생성 재정의 

```java
import org.apache.commons.io.IOUtils;

import javax.servlet.ReadListener;
import javax.servlet.ServletInputStream;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletRequestWrapper;
import java.io.*;

public class RereadableRequestWrapper extends HttpServletRequestWrapper {
    private ByteArrayOutputStream cachedBytes;

    public RereadableRequestWrapper(HttpServletRequest request) {
        super(request);
    }

    @Override
    public ServletInputStream getInputStream() throws IOException {
        if (cachedBytes == null)
            cacheInputStream();

        return new CachedServletInputStream(cachedBytes.toByteArray());
    }

    @Override
    public BufferedReader getReader() throws IOException{
        return new BufferedReader(new InputStreamReader(getInputStream()));
    }

    private void cacheInputStream() throws IOException {
        /* Cache the inputstream in order to read it multiple times. For
         * convenience, I use apache.commons IOUtils
         */
        cachedBytes = new ByteArrayOutputStream();
        IOUtils.copy(super.getInputStream(), cachedBytes);
    }

    /* An input stream which reads the cached request body */
    private static class CachedServletInputStream extends ServletInputStream {

        private final ByteArrayInputStream buffer;

        public CachedServletInputStream(byte[] contents) {
            this.buffer = new ByteArrayInputStream(contents);
        }

        @Override
        public int read() {
            return buffer.read();
        }

        @Override
        public boolean isFinished() {
            return buffer.available() == 0;
        }

        @Override
        public boolean isReady() {
            return true;
        }

        @Override
        public void setReadListener(ReadListener listener) {
            throw new RuntimeException("Not implemented");
        }
    }
}
```

1. `OncePerRequestFilter` 를 상속받은 클래스의 `doFilterInternal` 메서드에서 래핑

```java
@Override
    protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain)
            throws ServletException, IOException {

        RereadableRequestWrapper rereadableRequestWrapper = new RereadableRequestWrapper((HttpServletRequest)request);
				filterChain.doFilter(rereadableRequestWrapper , response);
    }
```

1. AOP 에서 확인해봄
- 래핑해주지 않으면 body 가 비어있지만, 래핑해주면 잘 뜸

```java
@Around("com.dot3company.olobo.api.aop.LoggingAspect.onRequest()")
public Object doLogging(ProceedingJoinPoint pjp) throws Throwable {

HttpServletRequest request = // 5
                ((ServletRequestAttributes) RequestContextHolder.currentRequestAttributes()).getRequest();

        ServletInputStream inputStream = request.getInputStream();
        String messageBody = StreamUtils.copyToString(inputStream, StandardCharsets.UTF_8);

        System.out.println("Http Body = " + messageBody);
        System.out.println("Http Body Length = " + request.getContentLength());
}
```