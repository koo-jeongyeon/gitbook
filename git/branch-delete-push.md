# 첫번째 커밋 삭제(브런치삭제) 후 원격저장소에 강제 push

{% hint style="info" %}
브런치 삭제 후 스테이징된 코드 내역들을 언스테이징하게 돌려놓지 않고 커밋하면 예전 코드로 커밋되서 코드 다 날아감!!!! (이유를 이것으로 추정하고 있음, 나중에 다시 시도해볼 예정)
{% endhint %}

* 원격 저장소의 커밋을 완전히 갈아엎기위함

순서 : 로컬에서 커밋 내역 삭제 > 다시 커밋 후 > 원격저장소에 내역 강제 PUSH

커밋을 삭제하려면 reset 으로 하면 되지만, 첫번째 커밋을 삭제해야해서

아래와 같은 방식으로 진행함, 첫번째 커밋삭제 아니면 reset명령어로 삭제하고 add부터 하면됨

브런치 삭제

```jsx
git update-ref -d HEAD //HEAD 를 가르키고있는 브런치를 삭제함 
git rm --cached -r <파일네임> //원격저장소에 있는 파일을 삭제 (근데 굳이 안해도됨)
```

다시 브런치 생성

```jsx
git branch -M main
```

다시 add하고 커밋

* 만약 gitignore에 새로 파일 등록하고 적용되게 하고 싶으면 이전에(이전커밋) 스테이징된 그 파일을 빼줘야함… 명령어는 추후 찾아서 기록할예정 (일단 IDE로 빼줬음)

```jsx
git add *
git commit -m "커밋내용"
```

강제로 push

* push가 안되고 pull 받으라고 할수도 있음(원격저장소랑 로컬저장소 내용이 달라서) 이때 pull명령어를 쳤는데 (예시 : git pull origin main) 리버스를 하니마니 하는 내용이 뜰수 있는데 그내용은 나중에 다시 업로드할것임 (일단 리버스 true로 함)

```jsx
git push -f origin +main 
```

여기서 +main 으로 플러스를 붙여주는 이유는 붙여주지 않음 오류가 날수있는데

데이터 유실 등 문제가 있을 수 있는 부분이 있어 git에서 처리 되지 않도록 에러를 띄우는 것이라고함. 플러스를 붙여주면 무시하고 강제로 푸쉬 할수있음
