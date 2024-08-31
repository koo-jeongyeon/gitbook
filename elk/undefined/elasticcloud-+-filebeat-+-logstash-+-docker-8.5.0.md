# ElasticCloud + filebeat + logstash + docker 설정 (버전8.5.0)

무료버전 elk에 대한 자료만 많아서 cloud일때 filebeat & logstash 와 elk 연결하는건 공식문서를 많이 참고해야함, 본인은 filebeat & logstash는 도커에, elk는 elastic cloud를 사용하였음 진행하면서 logstash는 아예 따로 관리해주나 싶었는데 역시 서버는 따로 만들어 줘야 하지만 설정파일을 cloud에서 설정해서 관리하기 쉽게 되어있는것을 확인했음

#### 필수 참고

[Run Filebeat on Docker | Filebeat Reference \[8.5\] | Elastic](https://www.elastic.co/guide/en/beats/filebeat/current/running-on-docker.html)

[Configuring Logstash for Docker | Logstash Reference \[8.5\] | Elastic](https://www.elastic.co/guide/en/logstash/current/docker-config.html)

### 기본 로컬 연결 테스트

* Dockerfile로만 구성하여 테스트함
* filebeat와 logstash간의 연동 테스트

#### 파일구성 (호스트)

* filebeat 폴더 아래 생성

`filebeat/Dockerfile`

`filebeat/filebeat.yml`

* logstash 폴더 아래 생성

`logstash/Dockerfile`

`logstash/config/logstash.yml`

`~~logstash/pipeline/logstash.conf~~`

#### 도커에 네트워크 추가,확인

```yaml
docker network create elastic
docker network ls
```

#### elasticCloud 에서 logstash 파이프라인 관리

* 파이프라인 관리를 elasticCloud에서 logstash.yml 의 내용을 정의해줌으로써 가능함 (중앙집중식관리)

[Centralized Pipeline Management | Logstash Reference \[8.5\] | Elastic](https://www.elastic.co/guide/en/logstash/current/logstash-centralized-pipeline-management.html)

아래경로에서 파이프라인 생성

**Management > logstash Pipelines > Create Pipeline**

```json
input {
  beats {
    port => 5044
  }
}

filter {
}

output {
  elasticsearch {
    cloud_id => "클라우드ID"
    ssl => true
    ilm_enabled => false
    user => "elastic" 
    password => "your_cluster_password"
    index => "applog-%{+YYYY.MM.dd}"
  }
  stdout { codec => rubydebug }
}
```

* user와 password에 사용자ID 와 비번 입력 \[elastic cluster 생성시에 자동으로 발급받음]
* cloud\_id 에 클라우드ID 입력 \[elastic cloud 메인에 있음]
* 5044포트로 filebeat 에게서 로그파일 받음
* 위 내용을 pipeline에 작성 이름이랑 설명작성후 나머진 그대로 두고 생성

그리고 아래 경로에서 `logstash_admin` Role로 사용자 생성해줘야함

**Management > Security > Users**

#### logstash 로컬에 파일 생성

`logstash/config/logstash.yml`

```yaml
xpack.management.enabled: true
xpack.management.pipeline.id: ["pipeline1"] # ID of your pipeline
xpack.management.elasticsearch.cloud_id: #CLOUD_ID작성
xpack.management.elasticsearch.cloud_auth: #위에서생성한유저:비밀번호
```

* 파이프라인이름, 클라우드id, 위에서 logstash\_admin role을 준 유저와 비밀번호 입력

`logstash/Dockerfile`

```yaml
FROM docker.elastic.co/logstash/logstash:8.5.0
RUN rm -f /usr/share/logstash/pipeline/logstash.conf
COPY pipeline/ /usr/share/logstash/pipeline/
COPY config/logstash.yml /usr/share/logstash/config/logstash.yml
```

* 도커파일 작성

#### logstash 이미지빌드

```
sudo docker build -t logstash:8.5.0 /Applications/Ddrive/logstash
```

#### logstash 컨테이너 생성

```
sudo docker container run -it --name logstash \\
--network elastic -p 5044:5044 \\
logstash:8.5.0
```

#### filebeat 로컬에 파일 생성

`filebeat/filebeat.yml`

```yaml
filebeat.inputs:
- type: log
  enabled: true
  paths:
    - /logs/*.log

output.logstash:
  hosts: ["logstash:5044"]
```

* 본인은 logstash와의 연결먼저 확인하기 위함
* /logs 의 경로는 도커 컨테이너 에서의 위치임, 로컬X

`filebeat/Dockerfile`

```yaml
FROM docker.elastic.co/beats/filebeat:8.5.0
COPY filebeat.yml /usr/share/filebeat/filebeat.yml
```

#### filebeat 이미지빌드

```yaml
sudo docker build -t filebeat:8.5.0 /Applications/Ddrive/filebeat
```

#### filebeat 컨테이너 생성

```yaml
sudo docker container run -it --name filebeat \\
--network elastic \\
-v /Applications/Ddrive/logs:/logs \\
filebeat:8.5.0
```

* \-v : volume 설정
  * /Applications/Ddrive/logs : 로그파일이 있는 로컬위치
  * /logs : 컨테이너 위치

참고

[5장. 도커 볼륨 이용하기](https://velog.io/@ckstn0777/%EB%8F%84%EC%BB%A4-%EB%B3%BC%EB%A5%A8)

[ELK filebeat 설치(docker)](https://darksharavim.tistory.com/560)

[\[Docker-elk\] docker-filebeat 세팅하고 띄우기](https://yonikim.tistory.com/22)
