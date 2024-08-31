# Docker 기초

## 도커 컨테이너 실행

```
sudo docker container run -it --name ubuntu ubuntu:latest
```

* 위와 같이 `run` 으로 컨테이너를 실행함
* `ubuntu:latest` 이부분이 이미지고 로컬에서 찾다가 없으면 hub(=레지스트리)에서 찾아옴
* 실제 운영환경의 이미지는 베이스가 되는 이미지와 설정파일, 그리고 명령들을 모아서 한데 `빌드`한것임
* 그리고 그 이미지로 컨테이너를 실행함
* `pull` 하면 이미지를 받아오고
* `ls` 하면 이미지 목록을 보고
* `inspect` 하면 이미지 정보를 보고
* 특이한건 `container commit` 하면 컨테이너에서 이미지를 생성한다
* `container export` 로 실행중인 컨테이너에서 파일을 생성해서 `import` 로 그 파일을 이미지로 생성할수도 있음

## 도커 파일 실행

* 어떤 이미지를 베이스로 사용할지, 어떤포트를 열지 어떤 설정을 할지 작성함
* 이런 파일의 내용을 기반으로 `빌드`를 하면 그 결과물로 이미지가 나옴

```bash
sudo docker build -t sample:1.0 /home/(USER)/docker
```

위와같이 `sudo docker build -t` 를 사용해 빌드함
