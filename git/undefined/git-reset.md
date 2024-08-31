# git reset



* reset 명령은 HEAD 브랜치를 현재 브랜치로 가리킨채 커밋을 바꾼다

soft : 가장 최근의 commit 명령을 되돌림, 커밋내용들은 staged 상태로 되돌린다

```
git reset --soft HEAD~
```



mixed : 아무 옵션도 주지 않으면 기본적으로 mixed 로 실행, commit 명령도 되돌리고 add 명령도 되돌린다. 커밋내용들은 unstaged 상태로 돌아간다

```
git reset --mixed HEAD~
```



hard : 리셋명령은 어떻게 사용해도 간단히 결과를 되돌릴수있지만 이 옵션은 되돌리는게 불가능하다&#x20;

* 복원하고 싶으면 reflog 를 이용해 할 수 있지만 커밋한 적이 없는 건 복원불가?

```
git reset --hard HEAD~
```





