# 스케쥴링 설정시 에러 상황

생성일: 2021년 5월 25일 오후 2:09

부모앱 설정 당시 아래 에러로 고생함

No thread-bound request found: Are you referring to request attributes outside of an actual web request, or processing a request outside of the originally receiving thread? If you are actually operating within a web request and still receive this message, your code is probably running outside of DispatcherServlet/DispatcherPortlet: In this case, use RequestContextListener or RequestContextFilter to expose the current request.

내가 @Aspect 로 전체 com.app.parent 하위 폴더에 동일한 메서드를 걸어놔서 스케쥴링 호출할때도 RequestContextHolder를 못불러오는데 자꾸 불러오려니 에러가 나는거였음 ㅋ

범위를 com.app.parent.controller 하위로 제한해줌

스케줄링 설정엔 여러 방법이 있지만 어노테이션이 제일 간단함 

```
xmlns:task="[http://www.springframework.org/schema/task](http://www.springframework.org/schema/task)"

[http://www.springframework.org/schema/task](http://www.springframework.org/schema/task) [http://www.springframework.org/schema/task/spring-task-4.0.xsd](http://www.springframework.org/schema/task/spring-task-4.0.xsd)
```

추가해주고

<task:annotation-driven/> 해주고 

component-scan만 제대로 설정해주면됨 

아니면 직접 클래스를 지정하는 방법도 있음 

클래스랑 메서드는 걍 이렇게

```java
@Component
public class SchedulerController {

    @Scheduled(cron="0/10 * * * * ?")
    public void TestScheduler() throws Exception{
        System.out.println("스케쥴링 테스트");
    }
}
```