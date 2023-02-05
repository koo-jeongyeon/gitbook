# 스프링 IoC컨테이너와 bean

생성일: 2022년 6월 15일 오전 11:45

흝어져있는 기억의 조각을 짜맞추고 여러 사람들의 글과 강의를 통한 중요한 개념 정리를 시작한다.


스프링 IoC컨테이너와 빈

- 이는 스프링 프레임워크의 3가지 핵심 프로그래밍 모델 중 하나인 의존성주입에 대한 이야기다.
- 스프링을 개발하다 보면 필연적으로 어노테이션을 쓰면서, 디비를 연결하면서 Bean에 관한 의문을 품게 된다. 그러다보면 IoC 컨테이너, applicationcontext 와같은 이런저런 개념을 보게 되지만 그게 도대체 무엇인지 몰라 오히려 더 헤메게된다. 버그는 해결되지 않고 개념도 머릿속에 없다.
- 그런 불상사는 더이상 없어야 한다. 그렇다면 근본을 공부하자

스프링에선 applicationcontext 라는 **IoC컨테이너**를 통해 **Bean**을 관리함.

IoC컨테이너? Bean? 잘 모르겠지만 이를 위한 대표적인 동작 원리로 DI가 있음

### DI(의존성주입)은 왜 사용할까?

햄버거 가게 예시

- 어떤 블로거의 햄버거 예시가 아주 좋아서 가져왔다. **꼭 직접 따라쳐라 기억에남게**
- 아래 코드에선 햄버거 레시피(HamBurgerRecipe)가 변화할때 요리사(BurgerChef)는 햄버거 만드는 방법을 수정해야한다(= HamBurgerRecipe클래스가 BurgerChef를 생성할때 생성자로 생성되어 아주 강하게 엮여있다. 만약 치즈버거레시피 CheeseBurgerRecipe 로 만드는 방법을 바꾸고 싶다면? 변수와 생성자가 다른 BurgerChef2 클래스를 또 만들어줘야 할거다 매우 이상하다.)

```java
class BurgerChef {
	private HamBurgerRecipe hamBurgerRecipe;
	public BugerChef(){
		hamBurgerRecipe = new HamBurgerRecipe();
	}
}

class HamBurgerRecipe {
	...
}
```

- 더 다양한 버거레시피를 의존 받을 수 있게 하려면 인터페이스를 사용한다. 클래스와 클래스가 강하게 결합되지 않게 사이에 인터페이스를 두는거다. (상속을 쓰지 않는 이유는 상속은 제약이 많고 확장성이 떨어지기 때문이다. 몰론 코딩에 이건써야한다 저건쓰지 말아야한다하는 절대적인 기준같은건 없겠지만….)
- 참고로 아래 코드가 이해되지 않는다면 **인터페이스의 다형성**을 알아야한다.
- 인터페이스 다형성(참고용)
    
    [https://wikidocs.net/269](https://wikidocs.net/269)
    
    매우 잘 설명되어 있다
    

```java
class BurgerChef {
	private BurgerRecipe burgerRecipe;
	public BugerChef(){
		 burgerRecipe = new HamBurgerRecipe();
		//burgerRecipe = new CheeseBurgerRecipe();
	}
}

interface BurgerRecipe {
	newBurger();
	... 다양한 메서드 
}

class HamBurgerRecipe implements BurgerRecipe {
	public Burger newBurger(){
		return new HamBurger();
	}
	...
}

class CheeseBurgerRecipe implements BurgerRecipe {
	public Burger newBurger(){
		return new CheeseBurger();
	}
	...
}
```

- 하지만 여전히 여러가지 버거를 만들려면 요리사 객체 생성시에 생성자를 변경해주어야한다. 여전히 비슷한 이름의 클래스를 여러개 만들어줘야한다… 여전히 하나의 하나의 음식만 만들수있는 요리사다…
- 지금 어떤 버거레시피를 만들지를 요리사가 생성될때 직접 정하고 있다. 그렇다면 요리사가 생성될때 어떤버거를 만들지 버거 가게의 사장님(쉐프)이 정한다면 어떨까???
- 코드를 보면 알겠지만 이렇게 하면 이제 요리사 클래스를 직접 수정하는 일은 없다. **이게 의존성주입(의존관계를 외부에서 결정하고 주입하는 것)이다.**
    - 생성자를 이용한 방법
    - *스프링4 부터는 생성자 주입을 강력히 권장한다고 한다*
    
    ```java
    class BurgerChef {
    	private BurgerRecipe burgerRecipe;
    	
    	public BurgerChef(BurgerRecipe burgerRecipe){
    		this.burgerRecipe = burgerRecipe;
    	}
    }
    
    class BurgerRestaurantOwner{
    	private BurgerChef burgerChef = new BurgerChef(new HamburgerRecipe());
    	public void changeMenu(){
    		burgerChef = new BurgerChef(new CheeseBurgerRecipe());
    	}
    }
    ```
    
    - 메서드를 이용한 방법
    
    ```java
    class BurgerChef {
    	private BurgerRecipe burgerRecipe;
    	
    	public void setBurgerRecipe(BurgerRecipe burgerRecipe){
    		this.burgerRecipe = burgerRecipe;
    	}
    }
    
    class BurgerRestaurantOwner{
    	private BurgerChef burgerChef = new BurgerChef();
    	public void changeMenu(){
    		burgerChef.setBurgerRecipe(new CheeseBurgerRecipe());
    	}
    }
    ```
    

- 팩토리 패턴의 DI 방법 또한 중요한듯하나 나중에 추가하겠다. ~~아직이해불가함.~~
    - 스프링의 beanfactory이 이와같은 방식으로 구현되어있다고함.

### 의존성 주입의 장점

- 위와 같이 사장이 직접 버거레시피를 정하도록 하면 요리사와 레시피와의 의존성이 줄어든다
    - 버거레시피가 추가,수정되도 요리사 클래스는 변하지 않는다.. 요리사를 사용하는 사장이 어떤 버거 레시피로 바꿔줄지 정해주면 그만이다
- 버거레시피를 인터페이스(BurgerRecipe)로 별도로 만들어줌으로써 다른클래스에서 재사용성이 높아진다
- 테스트하기도 좋다고함 버거레시피랑 요리사의 테스트를 분리해서 진행
- 가독성도 높아짐

### 스프링에서는 어떻게 활용되나?

- **의존성을 주입시켜줄 버거사장은 IoC컨테이너이고 이것이 관리하는 요리사(객체)를 Bean이라고 할 수 있다.**
    - 이해가 가는가? 스프링에서는 저런 객체들을 관리하는 IoC컨테이너가 존재하는거다. IoC를 제어의 역전이라고 하는 이유는 개발자가 직접 객체를 생성해주는게 아니라 객체의 생성과 생명주기를 컨트롤 하는 관리주체가 스프링이기 때문에 제어권을 다른 주체에게 넘긴다고 해서 **제어의 역전**이라고 한다.
    - 그리고 이러한 IoC컨테이너는 스프링 컨테이너 DI컨테이너등 여러가지 용어로 사용된다만 문서에는 정확히 Spring Bean Container라고 작성되있다고 한다. 근데 대부분 IoC컨테이너라고함.

- 일반객체와 Bean 객체의 구분?
    - 일반객체와 Bean은 어떤 영역에 생성되어 관리되는지에 따라 구분된다. 둘다 JVM영역에 있지만 Bean은 그중에서도 IoC 컨테이너에 속하는 것이다.
    - 그리고 스프링에서 DI를 하기 위해서는 의존되는 객체(요리사) 사용되는 객체(레시피) 모두 Bean 객체여야 가능하다.

- Bean등록
    - 빈객체로 등록하는 방법은 여러가지다. xml이 원초적인 방법이고 그 이후 javaConfig 설정이 나왔다.
    
    **[XML 방식]**
    
    **1. xml로 모든 Bean을 설정**
    
    ```xml
    <?xml version="1.0" encoding="UTF-8" ?>
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    
        <bean id="demoService" class="com.example.demo.DemoService">
            <property name="repository" ref="demoRepository"/>
            <property name="count" value="42"/>
        </bean>
        <bean id="demoRepository" class="com.example.demo.DemoRepository"/>
    </beans>
    ```
    
    - demoService에 repository와 count를 주입하도록 설정함
    - id : 빈 이름 설정, 자바코드에서 getBean() 주입설정에서 ref속성으로 빈을 참조하는 경우에 씀
    - class : 필수속성, 빈으로 설정할 클래스 설정
    - property : 객체 또는 값을 주입받는 태그, value로 값, ref로 객체를 주입했다.
    
    ```java
    package com.example.demo;
    
    public class DemoService {
        DemoRepository repository;
        int count;
    
        public void setRepository(DemoRepository demoRepository) {
            this.repository = demoRepository;
        }
        public void setCount(int count) {
            this.count = count;
        }
    }
    
    public class DemoRepository {
        public String name = "DemoRepository!";
    }
    ```
    
    - DemoService를 보면 의존성주입을 위한 setter가 있다.
    
    ```java
    package com.example.demo;
    
    import org.springframework.context.ApplicationContext;
    import org.springframework.context.support.ClassPathXmlApplicationContext;
    
    public class DemoApplication {
    
        public static void main(String[] args) {
            ApplicationContext ctx = new ClassPathXmlApplicationContext("application.xml");
    
            // class로 Bean 가져오기
            DemoService service = ctx.getBean(DemoService.class);
            System.out.println("1 " + service.repository.name);
    
            // 이름으로 Bean 가져오기
            DemoService service2 = (DemoService) ctx.getBean("demoService");
            System.out.println("2 " + service.repository.name);
    
            // Bean 이름 출력하기
            String[] beanNames = ctx.getBeanDefinitionNames();
            for (String name: beanNames) {
                System.out.println(name);
            }
        }
    }
    
    //출력결과
    /*
    1 DemoRepository!
    1 42
    2 DemoRepository!
    2 42
    demoService
    demoRepository
    */
    ```
    
    - ApplicationContext 객체가 등장했다. 여러가지 방식으로 Bean을 가져다 사용한다.
        - ApplicationContext는 아래에서 더 구체적으로 설명하겠다.
        
    
    2**. xml에서 component scan사용**
    
    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xmlns:context="http://www.springframework.org/schema/context"
           xsi:schemaLocation="http://www.springframework.org/schema/beans
               http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
               http://www.springframework.org/schema/context
               http://www.springframework.org/schema/context/spring-context-3.0.xsd">
        <context:component-scan base-package="com.example.demo"/>
    </beans>
    ```
    
    - component-scan에서 지정한 패키지 기준으로 @Component 어노테이션이 붙은 클래스를 검색해 빈으로 추가한다.
    - 사실 @Controller, @Service, @Repository도 컨테이너에 빈으로 등록하기 위한 어노테이션일뿐이다.(같은기능) 구분하기 쉽게 이름을 달리 했을뿐,,,
    
    ```java
    package com.example.demo;
    
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.stereotype.Component;
    
    @Component
    public class DemoService {
    
        @Autowired
        DemoRepository repository;
        int count;
    
        public void setCount(int count) {
            this.count = count;
        }
    }
    
    @Component
    public class DemoRepository {
        public String name = "DemoRepository!";
    }
    
    ```
    
    - @Autowired : 주입대상이 되는 bean을 컨테이너서 찾아서 주입해줌, DemoRepository가 Bean 으로 컨테이너에 등록되있으니까 같은 클래스타입인걸 찾아서 알아서 객체를 주입해주었음
    - xml설정 파일에 대한 더 다양한 방식
        
        [https://atoz-develop.tistory.com/entry/Spring-스프링-XML-설정-파일-작성-방법-정리](https://atoz-develop.tistory.com/entry/Spring-%EC%8A%A4%ED%94%84%EB%A7%81-XML-%EC%84%A4%EC%A0%95-%ED%8C%8C%EC%9D%BC-%EC%9E%91%EC%84%B1-%EB%B0%A9%EB%B2%95-%EC%A0%95%EB%A6%AC)
        
    
    **[javaConfig 방식]**
    

(작성중)

### 참고

[https://scshim.tistory.com/32](https://scshim.tistory.com/32)

[https://tecoble.techcourse.co.kr/post/2021-04-27-dependency-injection/](https://tecoble.techcourse.co.kr/post/2021-04-27-dependency-injection/)

[https://mangkyu.tistory.com/150](https://mangkyu.tistory.com/150)

[https://dev-coco.tistory.com/70](https://dev-coco.tistory.com/70)

[https://melonicedlatte.com/2021/07/11/232800.html](https://melonicedlatte.com/2021/07/11/232800.html)

[spring - 1주차 IoC와 DI](https://www.slipp.net/wiki/pages/viewpage.action?pageId=25527606)

[https://atoz-develop.tistory.com/entry/Spring-스프링-XML-설정-파일-작성-방법-정리](https://atoz-develop.tistory.com/entry/Spring-%EC%8A%A4%ED%94%84%EB%A7%81-XML-%EC%84%A4%EC%A0%95-%ED%8C%8C%EC%9D%BC-%EC%9E%91%EC%84%B1-%EB%B0%A9%EB%B2%95-%EC%A0%95%EB%A6%AC)

[https://johngrib.github.io/wiki/spring-bean-config-xml/](https://johngrib.github.io/wiki/spring-bean-config-xml/)