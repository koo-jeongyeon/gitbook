# 이미지 빌드 관련 문제상황

* AWS사용시 인스턴스 유형에 따라 다른 방식으로 이미지 빌드를 해줘야 하였음

알고보니 기존의 서버는 보통 x86 또는 AMD CPU 를 사용하여 실행 하였으나 현재 내가 사용중인 MAC m1칩은 ARM 기반 프로세서라는 차이가 있었음

그래서 ARM에서 그냥 build -t로 이미지를 빌드하면 aws의 AMD 기반 서버에선 동작하지 않음

때문에 AMD에 배포한다면 아래 명령어로 이미지 생성

```jsx
sudo docker buildx build --platform=linux/amd64 -t [경로]
```

로컬이나 ARM에 배포한다면 아래 명령어로 이미지 생성

```jsx
sudo docker build -t [경로]
```

Refer

[\[Docker\] Docker Buildx를 통한 Multi-architecture 이미지 빌드(x86, ARM)](https://kimjingo.tistory.com/115)
