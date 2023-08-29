# eb 설치 & 명령어


참고

[EB CLI 명령 참조](https://docs.aws.amazon.com/ko_kr/elasticbeanstalk/latest/dg/eb3-cmd-commands.html)

MAC OS 설치 & 버전확인

```jsx
brew install awsebcli
eb --version
```

- beanstalk 애플리케이션 설정

```jsx
eb init
```

- ssh 연결

beanstalk 구성 → EC2 키페어 설정해줘야함

MAC 에도 ssh 넣어놓고 진행

```jsx
eb ssh [환경이름]
```

- 환경 리스트 전체 보기

```jsx
eb list -a
```