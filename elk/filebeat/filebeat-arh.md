# filebeat 아키텍쳐

_Prospectors, Harvesters, Spooler_라는 주요 구성 요소

* _Prospectors :_ 로그를 읽을 파일 목록을 구분하는 역할을 담당한다. 여러 파일 경로를 설정하면 로그를 읽을 파일을 식별하고 각 파일에서 로그를 읽기 시작한다
* _Harvesters :_ 파일 컨텐츠, 즉 이벤트 데이터(로그)를 읽는 역할을 담당한다. 파일을 행 단위로 읽고 출력으로 보낸다. 하나의 _Harvester_가 개별로 파일을 담당하며 파일을 열고 닫는다. 읽어올 파일 수가 여러개가 되면 그에 따라 Harvester도 여러개가 되는 것이다.
* _Spooler :_ 이벤트를 집계하고 설정한 출력으로 전달한다.

파일비트가 지원하는 Input 타입은 log와 stdin이 있다

의문 : 물리적인 파일을 읽는데 어디까지 읽었고, 출력은 어디까지 보냈고 이런 정보를 어디서 유지하고 있는 것일까?

이러한 정보는 Harvester가 offset으로 디스크에 주기적으로 기록하며 이는 레지스트리 파일에서 관리한다. 엘라스틱서치,카프카,레디스 같은 출력 부분 미들웨어 시스템에 문제가 발생하면 파일비트는 마지막으로 보낸 행을 기록하고, 문제가 해결될때까지 계속 데이터를 수집하고 있는다. 이러한 관리 덕분에 파일비트를 내렸다 다시 올려도 데이터의 위치를 기억한 상태에서 기동되게 된다. 또한 Harvester 같은 경우 출력에게 데이터를 보낸 후에 출력 부분에서 데이터를 잘 받았다는 응답을 기다리는데 해당 응답을 받지 않은 경우 다시 데이터를 보내게됨으로 반드시 한번은 데이터 손실없이 보내게 된다.

참고

[ELK Stack - Filebeat(파일비트)란? 간단한 사용법](https://coding-start.tistory.com/187)

docker compose 로 배포시 offset 이 저장된 레지스트리가 유지가 안되어 (아예 새로 깔게되니까) log파일을 처음부터 싹 다 다시 읽어보냈는데, 저부분은 volume 으로 설정하면 유지가 되지 않을까 생각하여 해보니까 잘됨

나와 같은 사람…

[Running Filebeat in docker with persistent registry file?](https://discuss.elastic.co/t/running-filebeat-in-docker-with-persistent-registry-file/191595)
