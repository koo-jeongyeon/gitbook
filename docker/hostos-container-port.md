# 호스트OS에서 컨테이너 내의 프로세스의 포트 확인

* 컨테이너 리스트 확인 방법

```jsx
sudo docker container ls
```

* 프로세스 아이디 확인

```jsx
docker inspect -f '{{.State.Pid}}' [container_id or name]
```

* nsenter 명령어로 확인

```jsx
sudo nsenter -t [프로세스id] -n netstat -tupln
```

* 참고로 그냥 열려있는 포트 확인

```jsx
netstat -tnlp | grep "LISTEN”
```

Refer

[\[docker\] container 내에서 실행된 특정 프로세스가 몇번 포트로 LISTEN하는지 확인하는 방법](https://m.blog.naver.com/timberx/221595015449)
