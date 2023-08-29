# mongoDB export import

#### dump 하기

```yaml
mongodump --uri "mongodb+srv://<접근계정>:<패스워드>@<atlas클러스터주소>/<database명>" --out <저장할로컬경로>
```

#### dump 한 파일 복구

```yaml
mongorestore --uri "mongodb+srv://<계정명>:<패스워드>@<atlas클러스터주소>" --drop <초기화할database명-선택사항> --db <생성및주입할database명> <로컬경로및파일명>
```

참고

[https://realwater87.tistory.com/6](https://realwater87.tistory.com/6)

[https://jackinstitute.tistory.com/247](https://jackinstitute.tistory.com/247)
