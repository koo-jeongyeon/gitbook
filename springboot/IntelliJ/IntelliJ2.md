# Spring boot 프로젝트 생성

생성일: 2021년 2월 19일 오후 5:59

### IntelliJ setting

[https://velog.io/@codemcd/Spring-Boot-JPA-IntelliJ로-웹-구현하기-1-xdk56qmpre](https://velog.io/@codemcd/Spring-Boot-JPA-IntelliJ%EB%A1%9C-%EC%9B%B9-%EA%B5%AC%ED%98%84%ED%95%98%EA%B8%B0-1-xdk56qmpre)

참고

디팬던시는

Spring web

Spring Data JPA

mssql 

lombok 

이런거 해주고 

- 메이븐 레파지토리

[https://mvnrepository.com/artifact/org.projectlombok/lombok/1.18.12](https://mvnrepository.com/artifact/org.projectlombok/lombok/1.18.12)

build.gradle에 

```
dependencies {
    // https://mvnrepository.com/artifact/com.microsoft.sqlserver/mssql-jdbc
    implementation group: 'com.microsoft.sqlserver', name: 'mssql-jdbc', version: '6.4.0.jre8'
    runtimeOnly 'com.microsoft.sqlserver:mssql-jdbc' // MSSQL
    // https://mvnrepository.com/artifact/org.projectlombok/lombok
    compileOnly group: 'org.projectlombok', name: 'lombok', version: '1.18.12'

    implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
    implementation 'org.springframework.boot:spring-boot-starter-web'
    providedRuntime 'org.springframework.boot:spring-boot-starter-tomcat'
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
}
```

다른디비

```
runtimeOnly 'org.mariadb.jdbc:mariadb-java-client' // MariaDB
runtimeOnly 'com.h2database:h2' // H2
runtimeOnly 'com.microsoft.sqlserver:mssql-jdbc' // MSSQL
runtimeOnly 'mysql:mysql-connector-java' // MYSQL
runtimeOnly 'org.postgresql:postgresql' // PostgreSQL
```