# API 부하테스트 툴 K6

#### 설치

```yaml
brew install k6
```

#### 스크립트 작성

```jsx
import http from 'k6/http';
import { sleep, check } from 'k6';

export default function () {
  const body = { //담길json };
  const params = { headers: {'Content-Type': 'application/json'} };
  const res = http.post('<http://localhost:8080/api/v1/login>', JSON.stringify(body), params);

  check(res, {
    'response code was 200': (res) => res.status === 200,
  });

  sleep(0);
}
```

#### 쉘 명령어 입력

```jsx
k6 run --vus 10 --duration 30s --out json=out.json script.js
```

vus: 동시 작업자 수

duration: 부하 테스트 시간

out: 출력 포맷 및 파일명

script.js: 실행할 스크립트

api 레퍼런스: [https://k6.io/docs/javascript-api/k6/check-val-sets-tags/](https://k6.io/docs/javascript-api/k6/check-val-sets-tags/)

* 참고

[Load testing for engineering teams | Grafana k6](https://k6.io/)

[REST API 부하테스트 툴 - K6](https://velog.io/@dev\_sprinkler/REST-API-%EB%B6%80%ED%95%98%ED%85%8C%EC%8A%A4%ED%8A%B8-%ED%88%B4-K6)
