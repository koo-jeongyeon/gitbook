# 로드벨런서에 logstash 세팅

구조

Filebeat —> NLB (network load balancer) —> Logstash

ELK에 로그스태치를 세팅

* 파일비트 데이터를 전송할 때 로드벨런서 아래 인스턴스에 직접 연결할 수는 없다. IP가 계속 바뀌기 때문
* 로드벨런서에 연결 해줘야하는데 프로토콜을 TCP 연결로 해주어야한다.

### 환경 생성

환경을 만들고 **추가옵션구성**을 설정한다.

<figure><img src="../.gitbook/assets/스크린샷 2022-12-27 오후 2.40.45.png" alt=""><figcaption></figcaption></figure>

* 구성을 사용자 지정 구성으로 설정

<figure><img src="../.gitbook/assets/스크린샷 2022-12-27 오후 2.41.08.png" alt=""><figcaption></figcaption></figure>

* 용량을 로드 밸런싱 수행으로 설정

<figure><img src="../.gitbook/assets/스크린샷 2022-12-27 오후 2.42.05.png" alt=""><figcaption></figcaption></figure>

* 이 부분이 중요하다. 로드밸런서는 환경 생성시에 유형을 선택할 수 있는데 기본인 ALB로 선택하면 리스너 프로토콜을 TCP로 지정할 수 없어서 파일비트에서 로드밸런서로 데이터를 보내면 프로토콜 에러가 난다.
* NLB로 설정하고 리스너와 프로세스를 아래처럼 설정한다.

<figure><img src="../.gitbook/assets/스크린샷 2022-12-27 오후 2.45.56.png" alt=""><figcaption></figcaption></figure>

* logstash 는 9600으로 헬스체크가 가능하다. 때문에 9600과 파일비트 데이터를 수신하는 5044 포트를 설정한다.

참고

보안그룹은 위에를 설정하면 자동으로 설정이 되었는데 혹시 안되었다면 직접 포트를 열어주어야 할것이다.

프록시 서버도 필요없으니까 없음으로 설정하자

### filebeat

```jsx
filebeat.inputs:
- type: log
  enabled: true
  paths:
    - /static/logs/*.log //로그경로
  fields:
    index_name: "olobo_app_log" //logstash에 전송할 필드

output.logstash:
  hosts: ["XXXX.elb.us-east-1.amazonaws.com:5044"] //NLB도메인
  ttl: 120s 
```

* ttl 은 로그스태시 호스트로의 커넥션 유지 시간을 설정한다. 로드밸런싱을 적용할 때 유용하다.
* 로그스태시로의 커넥션은 sticky하기 때문에, 로드밸런싱의 목적인 커넥션의 분산이 고르지 못하게 이루어질 수 있다. 이 옵션을 설정함으로써 일정 시간 후 커넥션을 끊었다가 다시 맺도록 하여, 커넥션이 고루 분산되도록 할 수 있다.

refer.

[Setting up a Load-balanced Logstash behind and AWS ELB](https://medium.com/@manoj.senguttuvan/setting-up-a-load-balanced-logstash-behind-and-aws-elb-cc793bf9fda4)
