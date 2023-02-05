# lombok

생성일: 2022년 6월 23일 오후 4:15

참고 [https://projectlombok.org/features/all](https://projectlombok.org/features/all)

Val

* 타입을 명시적으로 선언하지 않아도 자동으로 잡아줌, final 이 자동으로 적용됨

사용한것 : val example = **new** ArrayList();

사용안한것 : **final** ArrayList example = **new** ArrayList();

Var

* 위의 기능에서 final 만 빠짐

@NonNull

* Null 체크 로직을 자동으로 생성해줌

사용

```java
import lombok.NonNull;

public class NonNullExample extends Something {
  private String name;
  
  public NonNullExample(@NonNull Person person) {
    super("Hello");
    this.name = person.getName();
  }
}
```

미사용

```java
import lombok.NonNull;

public class NonNullExample extends Something {
  private String name;
  
  public NonNullExample(@NonNull Person person) {
    super("Hello");
    if (person == null) {
      throw new NullPointerException("person is marked non-null but is null");
    }
    this.name = person.getName();
  }
}
```

**@NoArgsConstructor**

* 파라미터 없는 생성자 생성

**@RequiredArgsConstructor**

* 초기화 되지 않은 모든 final 필드, @NonNull로 마크돼있는 모든 필드들에 대한 생성자를 자동으로 생성해줌

**@AllArgsConstructor**

* 클래스의 존재하는 모든 필드의 생성자를 생성함

@Data

* @Getter / @Setter / @ToString / @RequiredArgsConstructor / **@EqualsAndHashCode**
* 위의 어노테이션이 다 제공됨

@Value

* @Value는 Immutable Class을 생성해준다.  @Data와 비슷하지만 모든 필드를 기본적으로 Private 및 Final로 로 하고, Setter 함수를 생성하지 않고, Class또한 Final로 지정하는 것만 빼고 동일하다.

**@EqualsAndHashCode**

* 아래 내용 참고하기

[equals와 hashCode 사용하기 ( +lombok)](https://jojoldu.tistory.com/134)

@**Builder**

[Lombok @Builder의 동작 원리](https://velog.io/@park2348190/Lombok-Builder%EC%9D%98-%EB%8F%99%EC%9E%91-%EC%9B%90%EB%A6%AC)

[\[자바\] 자주 사용되는 Lombok 어노테이션](https://www.daleseo.com/lombok-popular-annotations/)
