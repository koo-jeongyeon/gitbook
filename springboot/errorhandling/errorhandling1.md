# ErrorCode생성 및 ExceptionHandler로 에러처리

생성일: 2022년 10월 26일 오전 11:39

## 비즈니스 에러처리

- 참조에 있는 깃허브 코드와 블로그 내용을 토대로 문서를 작성하고 개발함
    
    [오류코드.pdf](../../image/errorhandling1.pdf)
    
- 문서의 엑셀 링크는 엑세스 할 수 없으니 이미지를 첨부함 (예시)
    
    ![스크린샷 2022-10-27 오전 9.26.02.png](../../image/errorhandling2.png)
    

- `ErrorCode` enum 을 생성
    - 정의한 오류코드들을 enum 으로 구현

```java
import lombok.Getter;

@Getter
public enum ErrorCode {

    ERROR_SUCCESS(200, "success")
    , ERROR_USER(401,"this email is already in use")
		....(생략)

    private final int code;
    private final String msg;

    ErrorCode(int code, String msg){
        this.code = code;
        this.msg = msg;
    }
}
```

- `RuntimeException` 을 상속받는 클래스를 생성해준다

```java
@Getter
public class ApiException extends RuntimeException {
    private final ErrorCode errorCode;
    
    ApiException(ErrorCode errorCode){
        this.errorCode = errorCode;
    }
}
```

- `RuntimeException`
    - 실행중에 발생하는 에러이며 시스템환경적으로나 인풋값이 잘못된 경우나 프로그래머가 의도적으로 에러를 발생시킬 조건에 부합할 때 발생

- @ExceptionHandler 어노테이션 활용한 ApiExceptionHandler 생성
    - value 값으로 어떤 exception을 줄지 정함
    - 내가 보내주고 싶은 정보만 담아서 보낼 수 있음

```java
@RestControllerAdvice
public class ApiExceptionHandler {

    @ExceptionHandler({ApiException.class})
    public ResponseEntity handleException(ApiException ex){
        Map<String, String> errors = new HashMap<>();
        errors.put("error_user_msg", ex.getErrorCode().name());
        errors.put("error_code", ex.getErrorCode().getErrorCode()+"");
        return ResponseEntity.badRequest().body(errors);
    }
}
```

- 클래스를 따로 만들어서 new 로 객체로 보낼수도 있는데 본인은 이런방식이 더 편해서 map 에 담아 보냄
    - 차이가 있는지 확인해봐야겠음

- 에러처리를 주고 싶은곳에 아래처럼 에러를 날리면됨

```java
if (user == null)
	throw new ApiException(ErrorCode.ERROR_USER);
```

- 응답예시

```java
{
    "error_code": "410",
    "error_user_msg": "ERROR_USER_PASSWORD"
}
```

- exception 에러핸들링 설명 끝판왕으로 잘되있음

[https://cheese10yun.github.io/spring-guide-exception/](https://cheese10yun.github.io/spring-guide-exception/)

- 참조

[https://velog.io/@kiiiyeon/스프링-ExceptionHandler를-통한-예외처리](https://velog.io/@kiiiyeon/%EC%8A%A4%ED%94%84%EB%A7%81-ExceptionHandler%EB%A5%BC-%ED%86%B5%ED%95%9C-%EC%98%88%EC%99%B8%EC%B2%98%EB%A6%AC)

[https://pjh3749.tistory.com/273](https://pjh3749.tistory.com/273)