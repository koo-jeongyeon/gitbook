# vue의 기본구조 실행순서


## package.json

```json
{
  "name": "frontend", //패키지의 이름
  "version": "0.1.0", //패키지의 버전
  "private": true, // 이 패키지를 공개할건지 비공개할건지 정의
  "scripts": { //이 패키지에서 사용할 명령어를 정의
    "serve": "vue-cli-service serve",
    "build": "vue-cli-service build",
    "lint": "vue-cli-service lint"
  },
```
참고 : [웹팩 러닝가이드](https://yamoo9.gitbook.io/webpack/webpack/config-webpack-dev-environment/webpack-dev-server)
- serve 명령어는 개발 모드로 프로젝트를 실행하는 명령어이다. 웹팩 데브 서버 모듈을 기반으로 하기 때문에 hmr(브라우저를 새로고침하지 않아도 웹팩으로 빌드한 결과물이 실시간으로 반영되게 도와줌)기능을 제공함. 이기능은 프로덕션 레벨에서 실행하기 위한건 아님
- build 명령어는 프로젝트를 배포하기 위한 빌드 명령어고 dist 폴더아래 번들링된 파일이 생성됨.
- lint 명령어는 문법오류나 코드컨벤션을 잡아줌 eslint 플러그인에 의해 검사됨. 자동으로 수정될 수 있는 규칙은 자동으로 수정된다.


```json
  "dependencies": { 
    "core-js": "^3.8.3",  //이런저런 문제를 개선한 폴리필 라이브러리
    "vue": "^2.6.14", //웹 어플리케이션에 빠르고 편리한 구현을 위한 자바스크립트 프레임워크
    "vue-router": "^3.5.3", //사용자 요청 경로에 따라 해당하는 컴포넌트에 매핑하여 렌더링을 결졍해주는 라이브러리
    "vuetify": "^2.6.0", //vue를 기반으로 구현된 머티리얼 디자인에 기반한 UI프레임워크
  },
  "devDependencies": {
    "@babel/core": "^7.12.16",
    "@babel/eslint-parser": "^7.12.16",
    ...
  }
```
- dependencies는 이 프로그램이 실행하기위해 필요한 라이브러리를 정의
- devDependencies는 개발시에 필요한 설치된 라이브러리에대한 정보


```json
"eslintConfig": {
    "root": true,
    "env": {
      "node": true
    },
    "extends": [
      "plugin:vue/essential",
      "eslint:recommended"
    ],
    "parserOptions": {
      "parser": "@babel/eslint-parser"
    },
    "rules": {}
  },
```
- eslintConfig는 lint에 대한 설정을 정의한다.


```json
  "browserslist": [
    "> 1%", //전세계 점유율 1퍼이상의 브라우저 타겟
    "last 2 versions", //최근 두개버전의 브라우저를 타겟
    "not dead" //지원이 중단된 브라우저는 제외
  ]
```
- 이 웹 어플리케이션이 타켓으로 하는 브라우저 정보나 유지보수시 사용해야할 자바스크립트 런타임 환경인 node의 정보를 설정



## 디렉토리 구조

- node_modules : npm install 로 설치한 외부 패키지들이 모여있음
- public : 웹팩의 처리를 받지 않고 퍼블리싱되는 정적 자산을 모아놓음, 아이콘과 같은 전처리 과정이 필요없는 파일이 있음
    - index.html : 어플리케이션의 뼈대가 되는 html
- src
    - assets : 정적 자산을 모아놓음 웹팩의 처리를 받음, 전처리 도구를 사용가능
    - components : Vue컴포넌트들이 모여있는 폴더
    - plugins : vue에 설치한 플러그인 패키지를 모아놓음
    - router : vue router 설정정보, 매핑정보를 담고 있음
    - views : 라우터에 의해 매핑된 컴포넌트(페이지)를 모아놓음 
    - App.vue : vue의 가장 최상위 컴포넌트
    - main.js : 가장먼저 실행되는 자바스크립트 파일, vue 인스턴스 생성
    

## npm start 실행순서

- package.json > index.html

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width,initial-scale=1.0">
    <link rel="icon" href="<%= BASE_URL %>favicon.ico">
    <title><%= htmlWebpackPlugin.options.title %></title>
    <link rel="stylesheet" href="https://fonts.googleapis.com/css?family=Roboto:100,300,400,500,700,900">
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@mdi/font@latest/css/materialdesignicons.min.css">
  </head>
  <body>
    <noscript>
      <strong>We're sorry but <%= htmlWebpackPlugin.options.title %> doesn't work properly without JavaScript enabled. Please enable it to continue.</strong>
    </noscript>
    <div id="app"></div>
    <!-- built files will be auto injected -->
  </body>
</html>
```

`<div id="app"></div>` 에 Vue 컴포넌트들이 마운팅됨

어떻게? main.js 보면 알수있음

- main.js

```javascript
import '@babel/polyfill'
import 'mutationobserver-shim'
import Vue from 'vue'
import App from './App.vue'
import router from './router'
import store from './store'
import vuetify from './plugins/vuetify'

Vue.config.productionTip = false

new Vue({
  router,
  store,
  vuetify,
  render: h => h(App)
}).$mount('#app')
```

여기서 vue인스턴스를 생성한다. 그리고 app을 정의해 index.html 에 vue 컴포넌트를 마운팅 시키는데

.$mount('#app') 보면 app이 마운팅 되는걸 볼수있음

 render: h => h(App) 에 대한 자세한 설명은 여기 참고

[Vuex란? 개념과 예제](https://doozi0316.tistory.com/entry/Vuex-%EA%B0%9C%EB%85%90%EA%B3%BC-%EC%98%88%EC%A0%9C-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0)

- App.vue, 가장 먼저 마운팅되는 컴포넌트

```javascript
<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <div class="d-flex align-center">
        <v-img
          alt="Vuetify Logo"
          class="shrink mr-2"
          contain
          src="https://cdn.vuetifyjs.com/images/logos/vuetify-logo-dark.png"
          transition="scale-transition"
          width="40"
        />

        <v-img
          alt="Vuetify Name"
          class="shrink mt-1 hidden-sm-and-down"
          contain
          min-width="100"
          src="https://cdn.vuetifyjs.com/images/logos/vuetify-name-dark.png"
          width="100"
        />
      </div>

      <v-spacer></v-spacer>

      <v-btn
        href="https://github.com/vuetifyjs/vuetify/releases/latest"
        target="_blank"
        text
      >
        <span class="mr-2">Latest Release</span>
        <v-icon>mdi-open-in-new</v-icon>
      </v-btn>
    </v-app-bar>

    <v-main>
      <router-view/>
    </v-main>
  </v-app>
</template>

<script>

export default {
  name: 'App',

  data: () => ({
    //
  }),
};
</script>
```

index.html 에 `<v-app>` 이 렌더링됨, 그중 `<router-view/>` 에 무엇이 렌더링 되는지는 

router/index.js 를 확인하면 알수있음

index.js, vue router 에 사용될 라우터들이 정의 되어있는 파일

```javascript
import Vue from 'vue'
import VueRouter from 'vue-router'
import HomeView from '../views/HomeView.vue'

Vue.use(VueRouter)

const routes = [
  {
    path: '/',
    name: 'home',
    component: HomeView
  },
  {
    path: '/about',
    name: 'about',
    // route level code-splitting
    // this generates a separate chunk (about.[hash].js) for this route
    // which is lazy-loaded when the route is visited.
    component: () => import(/* webpackChunkName: "about" */ '../views/AboutView.vue')
  }
]

const router = new VueRouter({
  mode: 'history',
  base: process.env.BASE_URL,
  routes
})

export default router
```

/ 로 접근할 경우 HomeView 컴포넌트를 렌더링하고

/about 으로 접근할 경우 AboutView.vue 를 렌더링하는걸 볼수있음