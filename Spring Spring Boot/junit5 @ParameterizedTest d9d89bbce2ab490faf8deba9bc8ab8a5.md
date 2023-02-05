# junit5 @ParameterizedTest

생성일: 2022년 1월 27일 오후 10:24

[https://junit.org/junit5/docs/current/user-guide/](https://junit.org/junit5/docs/current/user-guide/)

```jsx
package com.rootenergy.clone.demo.web;

import static org.junit.jupiter.api.Assertions.assertTrue;

import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.ValueSource;

public class MyFirstJUnitJupiterTests {

  @ParameterizedTest
  @ValueSource(ints = {1, 3, 5, -3, 15, 3}) //배열임 파라미터로 넘겨줌
  void isOdd_ShouldReturnTrueForOddNumbers(int number) {
      assertTrue(MyFirstJUnitJupiter.isold(number));
  } 
}
```

```jsx
package com.rootenergy.clone.demo.web;

public class MyFirstJUnitJupiter {

    public static boolean isold(int num){
        return (num%2==0)?false:true;
    }
}
```

@ParameterizedTest

- 매개변수화된 테스트를 사용하면 다른 인수로 테스트를 여러 번 실행할 수 있습니다. 일반 @Test 메소드처럼 선언되지만 대신 @ParameterizedTest 주석을 사용합니다. 또한 각 호출에 대한 인수를 제공한 다음 테스트 메서드에서 인수를 사용할 소스를 하나 이상 선언해야 합니다.

@ValueSource

- 해당 annotation 에 지정한 배열을 파라미터 값으로 순서대로 넘겨준다.
test method 실행 당 하나의 인수(argument) 만을 전달할 때 사용할 수 있다.
리터럴 값의 배열을 테스트 메서드에 전달한다.

```jsx
  @ParameterizedTest
  @NullAndEmptySource
  @ValueSource(strings = { " ", "   ", "\t", "\n" })
  void nullEmptyAndBlankStrings(String text) {
      assertTrue(text == null || text.trim().isEmpty());
  }
```

 @NullAndEmptySource

- **@NullSource + @EmptySource**
- 주석이 달린 @ParameterizedTest 메서드에 단일 null 인수를 제공합니다.

```jsx
@ParameterizedTest
  @MethodSource("range")
  void testWithRangeMethodSource(int argument) {
      assertNotEquals(9, argument);
  }
  
  static IntStream range() {
      return IntStream.range(0, 20).skip(10);
  }
```

@MethodSource

- 메서드의 리턴값을 매개변수로 테스트가능

@CsvSource : 나중에 해보기

- 엑셀로도 테스트 가능함