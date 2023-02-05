# [Error] intellij Process finished with exit code 1 error

생성일: 2022년 11월 30일 오후 12:13

[Process finished with exit code 1 Spring Boot Intellij](https://stackoverflow.com/questions/46428611/process-finished-with-exit-code-1-spring-boot-intellij)

```java
public static void main(String[] args) {
    try {
        SpringApplication.run(MyApplication.class, args);
    } catch (Exception e) {
        e.printStackTrace(); 
    }
}
```