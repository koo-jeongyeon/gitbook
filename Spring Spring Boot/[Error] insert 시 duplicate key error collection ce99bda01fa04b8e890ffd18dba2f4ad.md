# [Error] insert 시 duplicate key error collection

생성일: 2022년 10월 28일 오전 9:29
선택: Error

duplicate key error collection 이 에러는 index 가 중복되기 때문에 insert 하는 순간 에러가 나고 null 을 반환하지 않는다

그래서 select 로 해당 인덱스의 값이 있나 확인하고 없으면 Insert 함

옵셔널을 사용하면 뭔가 좋을것같은데 잘 모르겠음; 일단 위의 방식으로 ?

그리고 디비에서 에러날때 서버에러 날때,,, 에러처리 어떻게하냐

[INSERT 시 Duplicate entry 에러가 발생한다면? : INSERT INTO ON DUPLICATE KEY UPDATE](https://ojava.tistory.com/148)