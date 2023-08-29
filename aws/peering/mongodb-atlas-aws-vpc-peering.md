# mongodb atlas AWS vpc peering

* 아틀라스랑 AWS vpc 피어링 연결

[MongoDB Atlas와 AWS를 연계하여 구축하기](https://mystria.github.io/archivers/Config-MongoDB-Atlas-with-AWS)

[Set Up a Network Peering Connection](https://www.mongodb.com/docs/atlas/security-vpc-peering/)

[https://www.youtube.com/watch?v=8NITVf0L5X0](https://www.youtube.com/watch?v=8NITVf0L5X0)

* 아틀라스 CLI 설치

brew install mongodb-atlas

* 연결

atlas auth login 로 로그인 하고 코드 입력하여 (CLI에 표시됨) 연결

Successfully logged in as 표시되면 성공임

* AWS 연결
* 피어링할 VPC 를 선택하고 DNS 호스트 이름 편집, DNS 확인 편집 활성화

<figure><img src="../../.gitbook/assets/스크린샷 2022-10-21 오전 10.25.55.png" alt="" width="244"><figcaption></figcaption></figure>

* 아틀라스에서 peering 탭 피어링 커넥션 추가
* AWS의 계정 ID 등 필요한 항목 입력
* 피어링을 생성하고 AWS 의 피어링 연결 목록에서 생성된 피어링 요청수락 하기
  * cli로 피어링을 생성하면 아틀라스에는 안보이는듯함
* AWS 에서 라우팅 테이블 생성
* 보안그룹 생성해야되는데 나는 디폴트로 있는 vpc 를 사용해서 이미 존재
* 아틀라스의 ip access list 에서 시큐리티 그룹 추가함

host 명령어로 아틀라스의 디비 도메인? 치면 아이피나옴 → 이게 aws ip 겠지? 연결해보면 될듯
