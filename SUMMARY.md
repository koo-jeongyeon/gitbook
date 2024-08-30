# Table of contents

* [🖐️ Welcome](README.md)

## 📚 백엔드 로드맵 <a href="#backend-loadmap" id="backend-loadmap"></a>

* [메인페이지](backend-loadmap/undefined.md)

## Spring

* [spring boot](springboot/README.md)
  * [security](springboot/security/README.md)
    * [security 기본](springboot/security/security.md)
    * [filter](springboot/security/filter.md)
    * [JWT](springboot/security/jwt.md)
  * [스프링 핵심 원리](springboot/springbase/README.md)
    * [객체지향 설계와 스프링](springboot/springbase/springbase1.md)
    * [스프링IoC컨테이너와 bean](springboot/springbase/springbase2.md)
  * [IntelliJ](springboot/IntelliJ/README.md)
    * [Spring boot 생성 및 git clone](springboot/IntelliJ/IntelliJ1.md)
    * [Spring boot 프로젝트 생성](springboot/IntelliJ/IntelliJ2.md)
  * [vscode](springboot/vscode/README.md)
    * [Spring boot 프로젝트 생성](springboot/vscode/vscode1.md)
  * [scheduling](springboot/scheduling/README.md)
    * [스케쥴링 설정시 에러 상황](springboot/scheduling/scheduling1.md)
  * [paging](springboot/paging/README.md)
  * [에러 핸들링](springboot/errorhandling/README.md)
    * [ErrorCode생성 및 ExceptionHandler로 에러처리](springboot/errorhandling/errorhandling1.md)
    * [Security & JWT 에러처리](springboot/errorhandling/errorhandling2.md)
    * [spring cloud sleuth](springboot/errorhandling/errorhandling3.md)
  * [로그 핸들링](springboot/loghandling/README.md)
    * [logback](springboot/loghandling/loghandling2.md)
    * [HttpRequestServlet 래핑](springboot/loghandling/loghandling1.md)
  * [gradle](springboot/gradle/README.md)
    * [hidetake.ssh 키파일 설정](springboot/gradle/gradle1.md)
  * [maven](springboot/maven/README.md)
    * [maven tomcat](springboot/maven/maven1.md)
  * [lib](springboot/lib/README.md)
    * [lombok](springboot/lib/lombok.md)
    * [tiles](springboot/lib/tiles.md)
  * [API 부하테스트 툴 K6](spring/springboot/api-k6.md)
  * [JPA](spring/springboot/jpa/README.md)
    * [Mybatis / JPA 차이](spring/springboot/jpa/mybatis-jpa.md)
  * [Mybatis](spring/springboot/mybatis.md)
* [spring batch](spring/spring-batch/README.md)
  * [batch](spring/spring-batch/batch/README.md)
    * [Spring Batch 기본개념](spring/spring-batch/batch/batch1.md)

## FRONT

* [vue](front/vue/README.md)
  * [Spring boot & Vue.js 설치 및 연동](front/vue/setting.md)
  * [Spring boot & Vue.js 웹개발 세팅](front/vue/setting2.md)
  * [vue의 기본구조 실행순서](front/vue/context.md)
  * [SPA 이해](front/vue/spa.md)

## JAVA

* [환경설정](java/setting.md)
* [자바의 정석](java/standardofjava/README.md)
  * [generics](java/standardofjava/generics.md)

## DATABASE

* [mongoDB](database/mongodb/README.md)
  * [정규표현식을 사용해 대소문자 구분없이 검색](database/mongodb/find.md)
  * [mongoDB export import](database/mongodb/mongodb-export-import.md)
  * [MAC 설치 및 실행](database/mongodb/mac.md)
* [MYSQL](database/mysql/README.md)
  * [dbeaver 데이터 내보내기 불러오기](database/mysql/dbeaver.md)
  * [\[에러\] 스프링 mysql 8 연결 에러](database/mysql/mysql-8.md)
  * [MAC M1 mysql 설치](database/mysql/mac-m1-mysql.md)
  * [GROUP BY 정리](database/mysql/group-by.md)
  * [테이블 명세서 빠르게 생성](database/mysql/table-xsl.md)

## AWS

* [IAM](aws/iam.md)
* [설치&명령어](aws/setup/README.md)
  * [eb 설치 & 명령어](aws/setup/eb.md)
  * [CLI 설치 & 명령어](aws/setup/cli.md)
* [sam](aws/sam/README.md)
  * [SAM 개념](aws/sam/sam1.md)
  * [SAM Lambda S3이벤트 트리거, MongoDB 접근코드](aws/sam/sam2.md)
  * [SAM intellij 배포](aws/sam/sam3.md)
* [peering](aws/peering/README.md)
  * [mongodb atlas AWS vpc peering](aws/peering/mongodb-atlas-aws-vpc-peering.md)
  * [MongoDB & Lambda VPC peering ,endpoint설정](aws/peering/vpcpeering.md)
* [쉘스크립트](aws/undefined/README.md)
  * [도커 컨테이너 중단시 슬랙 리포팅 및 재실행](aws/undefined/undefined.md)

## DOCKER

* [Docker 기초](docker/basic.md)
* [Docker Container 유용한 명령어](docker/access.md)
* [이미지 빌드 관련 문제상황](docker/undefined.md)
* [MAC M1 도커 실행 원리](docker/mac-m1.md)
* [\[에러\] docker: Error response from daemon: Mounts denied:](docker/docker-error-response-from-daemon-mounts-denied.md)

## ELK

* [로드벨런서에 logstash 세팅](elk/logstash-setup-nlb.md)
* [filebeat 아키텍쳐](elk/filebeat-arh.md)
* [Logstash는 로그를 왜 message라는 field로 저장할까?](elk/logstash-message-field.md)
* [ELK 적용 사례, 로그수집(filebeat/logstash) 설명](elk/elk-filebeat-logstash.md)
* [logstash health check](elk/logstash-health-check.md)
* [Elastic Beanstalk + Elastic Cloud + docker 설정](elk/elastic-beanstalk-+-elastic-cloud-+-docker.md)
* [ElasticCloud + filebeat + logstash + docker 설정 (버전8.5.0)](elk/elasticcloud-+-filebeat-+-logstash-+-docker-8.5.0.md)

## unity

* [캐릭터 이동](unity/playermove.md)
* [카메라](unity/camera.md)

## WORDPRESS

* [Page 2](wordpress/page-2.md)

## git

* [라이프사이클](git/lifecycle.md)
* [defult 브랜치 main 으로 변경](git/defult-main.md)
* [첫번째 커밋 삭제(브런치삭제) 후 원격저장소에 강제 push](git/branch-delete-push.md)
* [git 원격저장소에 remote 방법(vscode로 진행)](git/git-remote-vscode.md)
* [git gh](git/git-gh.md)
* [git reset](git/git-reset.md)
* [git rebase](git/git-rebase.md)

## MAC

* [app store 다운로드 없이 웹에서 Xcode 다운](mac/app-store-xcode.md)
* [맥 초기 개발세팅](mac/setup.md)
* [ubuntu iso 설치 usb 만들기](mac/ubuntu-iso-usb.md)
* [응용프로그램 에러](mac/program-error.md)
* [잠김 파일](mac/lockfile.md)

## CS

* [data structure & algorism](datastructure/README.md)
  * [자료구조의 정의 및 종류](datastructure/datastructure\_definition.md)

## 방통대 <a href="#school" id="school"></a>

* [대학수학의 이해](school/math/README.md)
  * [1강. 수학의 기초(1)](school/math/1-.-1.md)
* [딥러닝](school/deeplearning/README.md)
  * [1강.신경망의 개요](school/deeplearning/step1.md)

## NODE

* [인스타그램 API 활용하여 게시물 슬랙에 리포팅](node/api.md)
