# logback

생성일: 2022년 10월 31일 오후 4:39

### 일반적인 출력

```java
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
        <layout class="ch.qos.logback.classic.PatternLayout">
            <Pattern>[%d{yyyy-MM-dd HH:mm:ss}:%-3relative][%thread] %-5level %logger{36} - %msg%n</Pattern>
        </layout>
    </appender>

    <appender name="FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <file>./logs/logback.log</file>
        <encoder>
            <pattern>[%d{yyyy-MM-dd HH:mm:ss}:%-3relative][%thread] %-5level %logger{35} - %msg%n</pattern> <!-- 해당 패턴 네이밍으로 현재 로그가 기록됨 -->
        </encoder>
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <fileNamePattern>./was-logs/info.%d{yyyy-MM-dd}.%i.log.gz</fileNamePattern> <!-- 해당 패턴 네이밍으로 이전 파일이 기록됨 -->
            <timeBasedFileNamingAndTriggeringPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedFNATP">
                <maxFileSize>100MB</maxFileSize> <!-- 한 파일의 최대 용량 -->
            </timeBasedFileNamingAndTriggeringPolicy>
            <maxHistory>30</maxHistory> <!-- 한 파일의 최대 저장 기한 -->
        </rollingPolicy>
    </appender>

<!--    <root level="INFO">-->
<!--        <appender-ref ref="STDOUT" />-->
<!--    </root>-->

    <!--  file에 찍힐 로그  -->
    <logger name="com.dot3company">
        <appender-ref ref="STDOUT" />
        <appender-ref ref="FILE" />
    </logger>

    <logger name="org.springframework.web">
        <appender-ref ref="STDOUT" />
        <appender-ref ref="FILE" />
    </logger>

</configuration>
```

- <root level="INFO"> 루트의 레벨을 xml 내에 설정해줘야 각 logger 의 레벨을 다양하게 설정할 수 있음

<logger name="com.dot3company" level=”warn” *`additivity*="false"` > 처럼 additivity (상속)을 false 로 하면 루트의 info 가 아닌 warn 로 레벨이 설정이 됨

- xml 이 아닌 프로퍼티즈나 yml 에 루트 설정하면 안되더라

참조

[Logback 으로 쉽고 편리하게 로그 관리를 해볼까요? ⚙️](https://tecoble.techcourse.co.kr/post/2021-08-07-logback-tutorial/)

[[스프링부트 (5)] Spring Boot 로그 설정(1) - Logback](https://goddaehee.tistory.com/206)


## logstash-logback-encoder

[GitHub - logfellow/logstash-logback-encoder: Logback JSON encoder and appenders](https://github.com/logfellow/logstash-logback-encoder)

### JSON 으로 로그파일 출력

[Spring boot LogBack for ELK Stack](https://itcoolly.tistory.com/149)

```java
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
        <encoder class="ch.qos.logback.classic.encoder.PatternLayoutEncoder">
            <pattern>[%d{yyyy-MM-dd HH:mm:ss}] [%thread{10}] [%X{traceId:-}] [%-5level] [%logger{0}:%line] - %msg%n</pattern>
        </encoder>
    </appender>

    <appender name="FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <file>./logs/logback.log</file>
        <encoder class="net.logstash.logback.encoder.LogstashEncoder"/><!-- json 형태로 파일에 로그 남김 -->
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <fileNamePattern>./was-logs/info.%d{yyyy-MM-dd}.%i.log.gz</fileNamePattern> <!-- 해당 패턴 네이밍으로 이전 파일이 기록됨 -->
            <timeBasedFileNamingAndTriggeringPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedFNATP">
                <maxFileSize>100MB</maxFileSize> <!-- 한 파일의 최대 용량 -->
            </timeBasedFileNamingAndTriggeringPolicy>
            <maxHistory>30</maxHistory> <!-- 한 파일의 최대 저장 기한 -->
        </rollingPolicy>
    </appender>

<!--    <root level="INFO">-->
<!--        <appender-ref ref="STDOUT" />-->
<!--    </root>-->

    <!--  file에 찍힐 로그  -->
    <logger name="com.dot3company">
        <appender-ref ref="STDOUT" />
        <appender-ref ref="FILE" />
    </logger>

    <logger name="org.springframework.web">
        <appender-ref ref="STDOUT" />
        <appender-ref ref="FILE" />
    </logger>

</configuration>
```