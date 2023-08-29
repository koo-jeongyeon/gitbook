# git gh

* git gh 는 깃허브 cli 앱이다
* git 만으로 원격저장소 클론 하고 연결하고 이러는 것보다 간단함
* MAC 기준

설치

```jsx
brew install gh
```

인증

```jsx
gh auth login
```

* github 사이트에서 로그인하고 인증함
* 사용할 git 프로토콜은 https 랑 ssh 중 선택할수있음

로그인 상태 확인

```jsx
gh auth status
```

클론

```jsx
gh repo clone <repository>
```

클론 되었으면 해당 폴더로 들어가서 리모트 연결

```jsx
gh repo fork
```

* 이미 되어있을수도 있음

원격연결 된것 확인

```jsx
git remote -v
```
