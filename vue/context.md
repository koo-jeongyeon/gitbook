# vue의 기본구조 실행순서


## 디렉토리 구조

<aside>
💡 router, vuetify, vuex 설치이후 디렉토리

</aside>

- public
    - index.html : 어플리케이션의 뼈대가 되는 html
- src
    - assets : 어플리케이션에서 사용되는 이미지등의 자료
    - components : Vue컴포넌트들이 모여있는 폴더
    - plugins : vuetify.js, bootstrap 등의 파일..
    - router : vue router 설정하는 디렉토리
    - views : vuex 다운받은후 생긴 디렉토리인데 잘 모르겠음
    - App.vue : 가장 최상위 컴포넌트
    - main.js : 가장먼저 생성되는 자바스크립트 파일 vue 인스턴스 생성
    

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