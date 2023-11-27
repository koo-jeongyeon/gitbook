# Spring boot & Vue.js 웹개발 세팅

## Vue를 쓰는 이유

[웹개발은 어떻게 구성해야 하는가](https://www.softmoa.com/sprt/blog/sprtBlogPost.pem/1846)

이전엔 백엔드에서 대부분의 알고리즘을 처리하고 프론트에선 단순히 UI나 자바스크립트만 사용했지만 최근엔 백엔드 서버의 부담을 덜어주고 더 빠른 비동기식 환경이 요구되며 UI,UX에 대한 기대가 높아져 정말 서버단에서 처리해야하는 부분을 제외하고는 대부분 UI상의 기능을 프론트엔드에서 처리하며 이에 따라 다양한 프론트엔드 프레임워크가 등장하였다.

그중 vue는 가장 간단한 문법을 가지고 웹팩을 통해 빌드시 모든 js,css를 패키지와해 가볍고 캡슐화처럼 보안성이 좋으며 개발환경이 편리하다는 장점이 있다. vue는 그 자체로도 하나의 개발환경을 구축해 테스트까지 가능하므로 분업화시에 장점이 있다.

vue는 기본적으로 비동기를 지향하며 spa를 강력히 밀고있다.

그런데 스프링은 ajax를 포함해 비동기를 지원하지만 편의성을 따지면 페이지 자체는 동기식으로 각각의 페이지를 구성하게 유도하고 내부에서 작동하는 통신을 ajax로 지원한다. 이는 타임리프를 지원하며 더욱 성향이 짙어졌다.

[타임리프 효용성](https://www.inflearn.com/questions/7209/thymeleaf%EC%9D%98-%ED%9A%A8%EC%9A%A9%EC%84%B1%EC%97%90-%EB%8C%80%ED%95%9C-%EC%A7%88%EB%AC%B8%EC%9E%85%EB%8B%88%EB%8B%A4)

[타임리프 설정](https://ellune.tistory.com/24)

백기선님 말에 따르면 서버개발자가 뷰까지 만을어야 하는 경우에 타임리프를 쓰는것이 학습비용면에서 현실적인 선택이라고 하였다.

그래서 스프링부트와 뷰 타임리프를 사용한다. 이렇게 하면 vue에서 빌드시킨 파일을 스프링 부트 서버에서 index.html 을 통해 보여지도록 할수있다.

허나 vue단일 프로젝트의 경우 여러개의 html과 app vue를 만들어 실행이 가능하나, 스프링에선 (아마 vue를 빌드하고 스프링 서버에서 실행시를 말하는듯) 그렇게 하려면 html의 위치를 조정해줘야하고(?) 소스가 늘어난다고 한다.

그래서 스프링에선 기본적으로 mpa를 사용하게끔 하고 필요한 부분만 spa를 사용하는게 가장 이상적이라고 한다.

[mpa와 spa설명](https://velog.io/@taypark/MPA%EC%97%90%EC%84%9C-SPA%EB%A1%9C)

그리고 이때문에 vue의 router기능을 사용한다.

## router

[router 개념 및 설치](https://any-ting.tistory.com/45)

먼저 npm 으로 라우터를 설치한다. 버전을 꼭 명시해서 설치해 줘야하는데 설치가 안되면 --force 옵션을 붙이면 된다.

```
npm install vue-router@3.5.3 --force   
```

이후 src/plugins/router.js 생성 (veutify를 먼저 설치하면 plugins폴더가 생성되어 있을것이다.)

### home과 about의 차이

* home은 사이트를 최초 방문할때 모든 컴퍼넌트를 가지고 오게 하는방식
* About은 사이트를 최초 방문과 상관없이 해당 경로에 접근시 컴퍼넌트를 가져오는 방식

참고로 한단어로 구성했더니 multi-word에러가나서 뒤에 Page를 붙여 두단어로 구성하였음 한단어로 해도 에러가 안나게 설정하고 싶으면 `lintOnSave:false` 를 `vue.config.js` 에 설정해주면됨

```javascript
import Vue from 'vue';
import VueRouter from 'vue-router';
import HomePage from '../views/HomePage.vue';

//Vue와 VueRouter 연결
Vue.use(VueRouter);

//우리가 사용할 route 생성 및 설정
const routes = [
    {
        path: '/', //사이트 url 경로
        name: 'HomePage', //컴퍼넌트 이름
        component: HomePage, //해당경로에 보이는 컴퍼넌트
        //사이트를 최초 방문할 때 모든 컴포넌트를 가지고 오게 하는 방식
    },
    {
        path: '/aboutpage',
        name: 'AboutPage',
        component: () => import('../views/AboutPage.vue'),
        //사이트를 최초 방문과 상관없이 사이트 해당 경로에 접근했을 때 해당 컴포넌트를 가지고 오는 방식
    },
];

//VueRouter에 route를 등록하고 설정한다.
const router = new VueRouter({
    mode: 'history',
    base: process.env.BASE_URL,
    routes,
});

//설정한 VueRouter 내보낸다.
export default router;
```

views 폴더 생성하여 아래에 HomePage.vue 와 AboutPage.vue를 알아서 생성해주고

src/App.vue에 router-link로 테스트 해봅니다.

* router-link 라우팅 역할
* router-view 컴퍼넌트를 렌더링 해주는 부분

```javascript
<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    > 
      <!-- 해당 페이지로 이동 -->
      <router-link to="/">HomePage</router-link> |
      <router-link to="/aboutpage">AboutPage</router-link>
    </v-app-bar>

    <v-main>
      <router-view/>
    </v-main>
  </v-app>
</template>
```

## axios 데이터 바인딩 <a href="#axios-eb-8d-b0-ec-9d-b4-ed-84-b0-eb-b0-94-ec-9d-b8-eb-94-a9" id="axios-eb-8d-b0-ec-9d-b4-ed-84-b0-eb-b0-94-ec-9d-b8-eb-94-a9"></a>

스프링과 뷰 사이 데이터 바인딩을 어떤 방식으로 하는지 알아보기 위해 2개의 레파지토리를 참고 하였다.

[spring boot vuejs](https://github.com/jonashackt/spring-boot-vuejs)

[vue springboot board](https://github.com/jinseogood/vue-springboot-board)

타입스크립트를 사용하고 싶었지만 학습비용을 줄이기 위해 2번째 레파지토리를 참고해 axios의 기본적인 사용과 자바스크립트 문법을 학습하였다.

src/api 폴더 생성하여 아래 backend-api.js 파일 생성

```javascript
import axios, {AxiosResponse} from 'axios'

const axiosApi = axios.create({
    baseURL: `/api/v1`,
    timeout: 1000,
    headers: { 'Content-Type': 'application/json'}
});

function hello(){
    return axiosApi.get(`/hello`);
}

export {
    hello
}
```

* export는 변수나 함수 클래스를 내보내기가 가능하게 하는데 위는 복수의 함수들이 있는 라이브러리 형태의 모듈이 될것이다. import할때 중괄호로 필요한 함수를 선언해주어야한다.
* 필요한 함수만 가져오는게 최적화에 좋고 어디서 어떤게 쓰이는지 명확하기 때문에 유지보수에 도움이 된다.

vue 컴퍼넌트

```javascript
<template>
  <v-container fluid>
    <v-row>
      <v-col cols="12">
        {{ response }}
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
import { hello } from '@/api/backend-api'

export default {
  // eslint-disable-next-line
  name: 'Dashboard',
  data: () => ({
    response: [],
    errors: []
  }),
  mounted() {
    this.gethello()
  },
  methods: {
    gethello(){
      return hello().then(response => {
        this.response = response.data;
      })
    }
  }


}
</script>
<style lang="">

</style>
```

* export default는 해당 모듈엔 개체가 하나만 있다는 사실을 명확히 나타낼수 있다. import시 중괄호 없이 가져온다.

## 빌드

[gradle로 한번에 빌드하기](https://herojoon-dev.tistory.com/25)

gradle build 명령어를 통해 스프링부트와 vue 둘다 한번에 빌드하도록 설정 가능하다.

## 참고

[vue 입문가이드](https://joshua1988.github.io/web-development/vuejs/vuejs-tutorial-for-beginner/)
