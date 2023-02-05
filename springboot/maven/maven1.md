# maven tomcat

생성일: 2021년 11월 24일 오후 3:29

톰캣7로됨, 아래처럼 세팅해주고 톰캣 서버재기동하면 톰캣에 배포 풀림 

pom.xml

```xml
			<plugin>
				<groupId>org.apache.tomcat.maven</groupId>
				<artifactId>tomcat7-maven-plugin</artifactId>
				<version>2.0</version>
				<configuration>
					<url>http://127.0.0.1:8080/manager/text</url>
					<server>TomcatServer</server>
					<path>/${project.build.finalName}</path>
					<username>admin</username>
					<password>password1234</password>  
				</configuration>
			 </plugin>
```

```xml
에러나서 추가함
<dependency>
		    <groupId>org.apache.maven.plugins</groupId>
		    <artifactId>maven-war-plugin</artifactId>
		    <version>2.2</version>
		</dependency>

```

~/.m2/settings.xml

```xml
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0
  http://maven.apache.org/xsd/settings-1.0.0.xsd">

  
   <servers>
      <server>
         <id>TomcatServer</id>
         <username>admin</username>
         <password>password1234</password>
      </server>
   </servers>
</settings>
```

tomcat-users.xml

```xml
<role rolename="manager-script"/>
<user username="admin" password="password1234" roles="manager-script"/>
```