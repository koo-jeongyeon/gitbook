# \[에러] 스프링 mysql 8 연결 에러

디비 연결 에러의 연속, 보안이 강화되었기 때문인것같다

* public key retrieval is not allowed

allowPublicKeyRetrieval=true 설정 추가 (jdbc연결주소에)

* caching\_sha2\_password authentication plugin

8.0 부터는 인증 플러그인을 지원, 보안이 강화됨 caching\_sha2\_password를 사용하려면 ssl 보안 연결을 사용하거나 rsa보안을 적용한 비암호 연결을 사용해야한다고함

이를 원하지 않을시 그냥 구식 비번 방식으로 변경하는 명령어를 씀

ALTER USER 'yourusername'@'localhost' IDENTIFIED WITH mysql\_native\_password BY 'youpassword';

아니면 둘다 사용하지 않게 jdbc에 설정

jdbc.url=jdbc:mysql://localhost:3306/spring\_security?allowPublicKeyRetrieval=true\&useSSL=false\&serverTimezone=Asia/Seoul

이경우는 보안에 취약함

* The server time zone value '´ëÇÑ¹Î±¹ Ç¥ÁØ½Ã' is unrecognized or represents more than one time zone. You must configure either the server or JDBC driver (via the serverTimezone configuration property) to use a more specifc time zone value if you want to utilize time zone support.

이건 원래 5.0에서도 에러나는 부분임 serverTimezone=UTC 를 jdbc에추가

* unknown system variable 'query\_cache\_size'

pom.xml 에서 mysql-connerctor-java 버전을 8.0.11 로 변경

매우 많은 에러와 싸워이겼다.....

참고

[https://deviscreen.tistory.com/85](https://deviscreen.tistory.com/85)

[https://kogle.tistory.com/87](https://kogle.tistory.com/87)
