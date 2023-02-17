# Spring boot & Vue.js 설치 및 연동

## 세팅
- Spring boot 3.2
- Java 17
- gradle - groovy
- vue2
- 종속성설치시 사용할 패키지 관리자 npm

## 설치

- homebrew로 node npm설치
```java
brew install node
```
- 버전 확인
```java
node -v
npm -v 
```


터미널에서 아래 명령 실행, npm으로 vue-cli 글로벌하게 설치

npm으로 Vue-cli를 global(-g)하게 설치한다는 뜻이다.

글로벌하게 Vue-cli를 설치해두면 이 프로젝트 외 다른 프로젝트에서도 Vue-cli를 사용할 수 있다. vue2로 선택했다. 

### Vue CLI 이란
- 기본 vue 개발 환경을 설정해주는 도구

- 기본적인 vue 프로젝트 세팅을 해주기 때문에 폴더 구조, ESLint, build, webpack 설정 등에 대한 고민을 덜 수 있음

```jsx
npm install -g @vue/cli
```


- 아래 명령어로 vue-cli에서 미리 세팅된 몇가지 템플릿을 사용해 frontend 라는 vue project를 생성해준다.

```jsx
// spring boot project 루트에서 진행
vue create frontend
```


- frontend 폴더로 이동, 서버시작

package.json에 scripts 라는 부분이 보일 것이다.

이 scripts를 통해 npm run으로 실행시킬 수 있는 명령어를 정의/설정해줄 수 있다.

serve, build, lint 가 있는데 각각 소스코드를 실행, 빌드, 소스코드검사 실행을 의미함

serve —open 이런식으로 수정해서 실행하면 바로 브라우저가 열리게 할수도 있음

서버를 종료하려면 Ctrl + c 누르면됨

```jsx
cd frontend
npm run serve
```


만약 기존 프로젝트를 받아와서 실행한다면 아래 명령을 실행해

기존에 깔려있거나 업데이트 되어있는 패키지를 재실행한다.
```jsx
npm ci
```


## 프록시 설정

개발 서버가 2개라 port번호도 2개로 나눠지는데 실제 배포환경에선 vue로 작성한 코드들은 빌드되어 정적인 자원으로 바뀌고(webpack에의해) 요청받는 서버는 스프링 하나가됨

개발을 편하게 하기 위해 vue서버에 프록시 설정을 하여 스프링으로 오게될 요청들을 모두 vue서버에서 받아 스프링으로 보내는 설정을 해준다. vue가 8081포트고 스프링이 8080이면 8081포트로 스프링부트 api를 요청했을때 프록시 설정에 의해 8080포트로 보내준다는 말이다.

그리고 그 정적인 자원은 타겟 디렉토리 설정에 의해 resources/static 으로 이동시킨다. 그렇지 않으면 빌드시마다 이동시켜줘야 해서 번거롭다. 

```jsx
const { defineConfig } = require('@vue/cli-service')
module.exports = defineConfig({
  transpileDependencies: true,
  outputDir: "../src/main/resources/static", // 빌드 타겟 디렉토리
  devServer: {
    proxy: {
      '^/api': {
        // '/api' 로 들어오면 포트 8080(스프링 서버)로 보낸다
        target: 'http://localhost:8080',
        changeOrigin: true
      }
    }
  }
})
```

참고
[HowToCreateProject · kimmy100b/spring-boot-vue Wiki](https://github.com/kimmy100b/spring-boot-vue/wiki/HowToCreateProject)