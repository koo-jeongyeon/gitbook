# Optional

생성일: 2022년 10월 28일 오전 9:36

에러처리를 아주 깔끔하게 하기에 필요한것같은데 이거 어떻게 쓰냐

일단 간략한건 자바8 이후로 등장했고

Optional 이라는 객체는 null 을 담을수있다는거고 그걸로 null 에러처리를 할수있다

왜냠 스프링에선 어떤건 null 이 안담겨서 널이냐 아니냐로 if 문 분기처리 해도 에러 처리를 할수없음 근데 그 어떤것들이 어떤상황인지 깊게 본적이 없네

그리고 jpa 에선 기본으로 저 Optional 에 담는다네 그래서 jpa 에선 쉽게 쓸수 있는것 같은데

[자바8 Optional 3부: Optional을 Optional답게](https://www.daleseo.com/java8-optional-effective/)

[](https://homoefficio.github.io/2019/10/03/Java-Optional-%EB%B0%94%EB%A5%B4%EA%B2%8C-%EC%93%B0%EA%B8%B0/)