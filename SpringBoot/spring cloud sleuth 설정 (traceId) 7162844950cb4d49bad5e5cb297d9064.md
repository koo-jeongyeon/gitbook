# spring cloud sleuth 설정 (traceId)

생성일: 2022년 12월 7일 오전 10:43

[Spring Cloud Sleuth](https://spring.io/projects/spring-cloud-sleuth)

[Spring Cloud Sleuth Reference Documentation](https://docs.spring.io/spring-cloud-sleuth/docs/current/reference/htmlsingle/spring-cloud-sleuth.html#getting-started)

```java
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-dependencies</artifactId>
            <version>${release.train.version}</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>

<dependencies>
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-sleuth</artifactId>
    </dependency>
</dependencies>
```

- 버전별로 잘 설치 해야함

```java

<appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
        <encoder class="ch.qos.logback.classic.encoder.PatternLayoutEncoder">
            <pattern>[%d{yyyy-MM-dd HH:mm:ss}:%-3relative][%thread{10}, %X{traceId:-}, %X{spanId:-}] %-5level %logger{36} - %msg%n</pattern>
        </encoder>
    </appender>
```

- `%X{traceId:-}, %X{spanId:-}` : traceID & spanID 로그로 찍어줌

[](https://www.baeldung.com/spring-cloud-sleuth-get-trace-id)

```java
@Autowired
private Tracer tracer;

Span span = tracer.currentSpan();
if (span != null) {
			log.info("Trace ID {}", span.context().traceId());
      log.info("Span ID {}", span.context().spanId());
}
```

- 이런식으로 컨트롤러에서 받아올 수 있음

참고

[Spring MVC Stack - Logging With AOP + MDC](https://lucas-k.tistory.com/8)

[Spring Boot 개발 시 sleuth, zipkin을 활용한 분산추적환경 구축](https://mr-spock.tistory.com/60)

[[번역] Spring Cloud Sleuth (1) Introduction](https://velog.io/@hanblueblue/%EB%B2%88%EC%97%AD-Spring-Cloud-Sleuth-1-Introduction)

[LINE 광고 플랫폼의 MSA 환경에서 Zipkin을 활용해 로그 트레이싱하기](https://engineering.linecorp.com/ko/blog/line-ads-msa-opentracing-zipkin/)

[Spring Boot 개발 시 sleuth, zipkin을 활용한 분산추적환경 구축](https://mr-spock.tistory.com/60)