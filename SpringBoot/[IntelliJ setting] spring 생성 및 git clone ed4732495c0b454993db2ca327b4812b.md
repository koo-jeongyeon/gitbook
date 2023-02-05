# [IntelliJ setting] spring 생성 및 git clone

생성일: 2021년 7월 8일 오후 5:00

### IntelliJ setting

참고한 블로그

[https://velog.io/@changyeonyoo/IntelliJ에서-Spring-MVC-환경-구축하기](https://velog.io/@changyeonyoo/IntelliJ%EC%97%90%EC%84%9C-Spring-MVC-%ED%99%98%EA%B2%BD-%EA%B5%AC%EC%B6%95%ED%95%98%EA%B8%B0)

위의 블로그와 동일하게 문제없이 spring 생성 진행하였음

- git 이슈

문제 : github 에 올릴때 git 으로만 올려봤는데 git bash 로 push 하는 부분에서 자꾸 먹통이남

해결 : cmd 창으로 가서 진행하니까 git 로그인 연결 창이 뜨면서 잘 진행됨

git 클론시 상당히 머리아픈 상황들이 있었음

- 이슈

문제 : 톰캣 설정하려고 Artifacts 건드리는데 잘 안됨 뭔가 이상함

해결 : 처음 클론 받았을때 오른족아래에 뜨는 알림 사항을 잘 누르고 창띄워서 ok 눌러줘야함 그래야 설정이 올바르게 되는듯, Artifacts 는 웹 어플리케이션 : expload 이걸로 만들고 directroy 를 web 으로 해줬음 out 에 자동 설정되는데 그건 바로바로 반영이 안됨 web으로 바꿔주고 서버 실행하면 web 아래에 META-INF 랑 WEB-INF 가 자동으로 생성된다

- 톰캣이슈

문제 : 톰캣이 빌드 안되는 이상한 문제

해결 : 톰캣 8 쓰고 있었는데 8에서만 이런 문제가 생기는것같아 7로 바꿔주니까 잘됨

진짜해결 : 아래 형광색 부분이 defaut 로 되어있으면 에러나는거였음

![%5BIntelliJ%20setting%5D%20spring%20%E1%84%89%E1%85%A2%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC%20%E1%84%86%E1%85%B5%E1%86%BE%20git%20clone%20ed4732495c0b454993db2ca327b4812b/Untitled.png](%5BIntelliJ%20setting%5D%20spring%20%E1%84%89%E1%85%A2%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC%20%E1%84%86%E1%85%B5%E1%86%BE%20git%20clone%20ed4732495c0b454993db2ca327b4812b/Untitled.png)

이클립스에도 시도??

범중이는 인텔리제이 깔아주고 체험 시켜주는게 좋을듯

- 클론받고 설정

문제 : iml 파일이 없어 클론 받고 메이븐 X 스프링 X

해결 : pom 의 스프링 설정을 지워서 업데이트하고 다시 입력후 업데이트 치면됨

pom 에 스프링 설정을 했을때와

인텔리제이 자체 스프링을 해줄때의 차이가 있음

난 pom 에 스프링 설정 해줘서 lib 는 없애고 External Libraries 아래에 메이븐이 있음

설정완료된 상태

![%5BIntelliJ%20setting%5D%20spring%20%E1%84%89%E1%85%A2%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC%20%E1%84%86%E1%85%B5%E1%86%BE%20git%20clone%20ed4732495c0b454993db2ca327b4812b/Untitled%201.png](%5BIntelliJ%20setting%5D%20spring%20%E1%84%89%E1%85%A2%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC%20%E1%84%86%E1%85%B5%E1%86%BE%20git%20clone%20ed4732495c0b454993db2ca327b4812b/Untitled%201.png)

![%5BIntelliJ%20setting%5D%20spring%20%E1%84%89%E1%85%A2%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC%20%E1%84%86%E1%85%B5%E1%86%BE%20git%20clone%20ed4732495c0b454993db2ca327b4812b/Untitled%202.png](%5BIntelliJ%20setting%5D%20spring%20%E1%84%89%E1%85%A2%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC%20%E1%84%86%E1%85%B5%E1%86%BE%20git%20clone%20ed4732495c0b454993db2ca327b4812b/Untitled%202.png)

![%5BIntelliJ%20setting%5D%20spring%20%E1%84%89%E1%85%A2%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC%20%E1%84%86%E1%85%B5%E1%86%BE%20git%20clone%20ed4732495c0b454993db2ca327b4812b/Untitled%203.png](%5BIntelliJ%20setting%5D%20spring%20%E1%84%89%E1%85%A2%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC%20%E1%84%86%E1%85%B5%E1%86%BE%20git%20clone%20ed4732495c0b454993db2ca327b4812b/Untitled%203.png)