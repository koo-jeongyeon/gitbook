# [Error] cannot deserialize from Object value (no delegate- or property-based Creator)

생성일: 2022년 10월 28일 오전 9:17
선택: Error

Json 을 object 로 변경해주는건데

cannot deserialize from Object value (no delegate- or property-based Creator)

이에러가 빈생성자가 없는 모델을 생성하는 방법을 모릅니다라는 뜻

모델에 빈생성자를 추가해줘야한다

롬복을 쓰면 `@NoArgsConstructor` 추가해주면됨