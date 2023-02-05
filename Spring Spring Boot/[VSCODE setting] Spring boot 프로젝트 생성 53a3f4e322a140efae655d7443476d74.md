# [VSCODE setting] Spring boot 프로젝트 생성

생성일: 2022년 1월 26일 오후 6:34

### VSCODE setting

[](https://limjunho.github.io/2021/08/06/VSC-spring-boot.html)

루트에너지 클론 사이트를 만들기 위해 생성했다

vscode 에서 스프링 부트 프로젝트를 생성하는건 크게 어렵지 않다

Ctrl + Shift + P 를 눌러 커맨드 팔레트를 열어 **‘Spring initalizr: Create a Gradle Project’**
를 선택

1. **Spring Boot version 선택:** 2.4.1(2021-01-05 기준)
2. **Project language 선택**: Kotlin
3. **Group Id 등록**: ex) io.honeymon.boot.springboot.vscode
4. **Artifact Id** 등록: spring-boot-of-vs-code
5. **Packaging type** 선택: JAR
6. **Java Version** 선택: 11

- gradle
- java 8
- spingboot 2.6.3

초기 생성 설정

```jsx
plugins {
	id 'org.springframework.boot' version '2.6.3'
	id 'io.spring.dependency-management' version '1.0.11.RELEASE'
	id 'java'
}

group = 'com.rootenergy.clone'
version = '0.0.1-SNAPSHOT'
sourceCompatibility = '8'

configurations {
	compileOnly {
		extendsFrom annotationProcessor
	}
}

repositories {
	mavenCentral()
}

dependencies {
	implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
	implementation 'org.springframework.boot:spring-boot-starter-web'
	compileOnly 'org.projectlombok:lombok'
	developmentOnly 'org.springframework.boot:spring-boot-devtools'
	runtimeOnly 'mysql:mysql-connector-java'
	annotationProcessor 'org.springframework.boot:spring-boot-configuration-processor'
	annotationProcessor 'org.projectlombok:lombok'
	testImplementation 'org.springframework.boot:spring-boot-starter-test'
}

tasks.named('test') {
	useJUnitPlatform()
}
```