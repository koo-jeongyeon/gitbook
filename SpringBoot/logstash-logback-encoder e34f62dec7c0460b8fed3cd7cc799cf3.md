# logstash-logback-encoder

생성일: 2022년 12월 16일 오후 2:50

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