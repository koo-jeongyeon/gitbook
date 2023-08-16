# 1강.신경망의 개요

기호 인공지능

연결주의 인공지능 → 배울거

인공 신경망

* 인간과 동물의 두뇌를 구성하는 신경 시스템의 원리를 바탕으로 설계된 계산 시스템

1943년 임계치 논리 라는 알고리즘을 바탕으로 한 신경망 계산 모델

1949년 도널드 헵이 헵의 학습 이론 제시

1957년 프랭크 로젠블이 퍼셉트론 학습 모델 제안 ?

* 마크 1 퍼셉트론
* 당시에 큰 관심 끔
* 당시에 마빈 민스카이 는 한계를 지적함 (1969년) XOR도 해결 못한다 해서 찬물을 끼얹음

1974년 1986년 폴 워버스 , 데이비드 등에 의해 다층 퍼셉트론 구조의 학습 방법 발표됨

* 역전파 알고리즘 → 근간이됨

신경망의 층과 표현력

* 한개의 층만으로 인식할때
  * 인식율이 떨어짐

<figure><img src="../../.gitbook/assets/스크린샷 2023-08-16 오후 7.14.30.png" alt=""><figcaption></figcaption></figure>

* 여러개의 층으로 인식할때
  * 중간에 있는게 뉴런역할
  * 중간층에 있는것 덕분에 인식율이 더 높아짐

<figure><img src="../../.gitbook/assets/스크린샷 2023-08-16 오후 7.17.45.png" alt=""><figcaption></figcaption></figure>

많은 층으로 구성하면 더욱 높은 차원의 표현 가능함

→ 심층 신경망

딥러닝은?

* 심층 신경망을 학습하기 위해 활용되는 기계학습 알고리즘
* 데이터가 많이 필요함
* 불안정한 경사 문제
* 과적합 문제
* 방대한 계산량

#### 신경망의 기본구조

인공 뉴런 (빨간 박스가 뉴런역할)

<figure><img src="../../.gitbook/assets/스크린샷 2023-08-16 오후 7.25.30.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/스크린샷 2023-08-16 오후 7.29.46 (2).png" alt=""><figcaption></figcaption></figure>

* u라는 입력을 뭔가 조건을 거쳐서 출력해줌 활성함수가
  * 0보다 작으면 출력을 억제하고 0보다 크면 출력을 내도록 설계함
* 저 바이어스에 따라 활성함수를 거칠때 활성화 되는지 안되는지 조정됨
  * 출력이 -0.5 인데 b가 1이면 0.5로 출력되니까 활성화 되겠지

계단함수

* 미분이 안되는 한계로 초기에나 쓰고 안쓰는 함수

<figure><img src="../../.gitbook/assets/스크린샷 2023-08-16 오후 7.32.36.png" alt=""><figcaption></figcaption></figure>

시그모이드 함수

* S자 형태라 미분가능함 → 미분이 가능하다는건 매우 유용함
* 층의 깊이가 깊어지면 경사 소멸이라는 문제가 생긴다함
* 대표는 로지스틱 함수

<figure><img src="../../.gitbook/assets/스크린샷 2023-08-16 오후 7.34.20.png" alt=""><figcaption></figcaption></figure>

ReLU

* 0일때 미분불가
* Dying ReLU 죽어있는 상태의 뉴런이 된다?
* ReLU의 변형
  * Leaky ReLU
  * PReLU
  * GELU
  * Swish

<div>

<img src="https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1d4527f5-b4e5-4f99-95b3-e361a0534d12/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-16_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_7.37.31.png" alt="">

 

<figure><img src="../../.gitbook/assets/스크린샷 2023-08-16 오후 7.37.31.png" alt=""><figcaption></figcaption></figure>

</div>

#### 단층 피드포워드 신경망

* 입력층에서 출력층 방향으로 뉴런 층의 연결이 이루어지는 구조
* 하나의 층으로 구성됨

<div>

<img src="https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a5a8d7d5-d9a3-4a75-9c99-55210f9851d9/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-16_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_8.32.57.png" alt="">

 

<figure><img src="../../.gitbook/assets/스크린샷 2023-08-16 오후 8.32.57.png" alt=""><figcaption></figcaption></figure>

</div>

지도학습 방식

* 입력과 레이블(출력)

학습 대상 파라미터

* 입력이 뉴런에 전달되는 연결의 가중치(W)
* 바이어스(b)

퍼셉트론의 학습

* 파라미터 초기화
* 모든 학습표본을 대상으로 파라미터 업데이트 반복

피셔의 붓꽃 데이터 집합

<div>

<img src="https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b97df334-fdd9-4e2d-bd10-acc250b7fa18/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-16_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_8.42.36.png" alt="">

 

<figure><img src="../../.gitbook/assets/스크린샷 2023-08-16 오후 8.42.36.png" alt=""><figcaption></figcaption></figure>

</div>

* 실습 프로그램 구성
  * `prepare_data(target)`
    * 특징 : 꽃잎의 길이와 폭
    * 레이블 : target 에 지정된 종류의 붓꽃이면 1 그외면 0
  * `step(x)`
    * 단위 계단 함수 : 0이상이면 1 그렇지 않으면 0
  * `visualize(...)`
    * 결과의 그래프를 출력하는 시각화 함수
    * 파라미터
      * net : 학습된 퍼셉트론
      * x,y : 특징 및 레이블의 배열
      * multi\_class : 3개 이상의 클래스로 분류하는 경우 true
      * labels : 클래스 레이블 리스트
      * class\_id : 클래스 이름을 출력할 스트링 리스트
      * colors : 클래스를 구분할 색상 리스트
      * xlabel, ylabel : x,y 축에 표시할 레이블
      * legend\_loc : 범례를 표시할 위치
  * `class Perceptron` 클래스 선언
    * 퍼셉트론 객체를 만들기 위한 클래스
    * 인스턴스 변수
      * self.dim : 입력층 입력의 수
      * self.activation : 활성함수
      * self.w : 가중치
      * self.b : 바이어스
    * 메서드 목록
      * init : 초기화
      * printW(self) : 가중치 및 바이어스 값 출력
      * predict(self,x) : 입력표본 x에 대한 퍼셉트론의 출력
      * fit(self, X, y, N, ephocs, eta=0.01) : 주어진 학습표본 집합을 이용해 퍼셉트론 객체를 훈련함

라이브러리

* matplotlib.pyplot : 시각화를 하기위한 함수들이 제공됨
* numpy : 파이썬에서 많이 사용됨 여러 수학적인 계산
* sklearn.datasets : 여러 데이터 제공

<figure><img src="../../.gitbook/assets/스크린샷 2023-08-16 오후 8.55.27.png" alt=""><figcaption></figcaption></figure>

3 : 0부터 3까지 있는데 2부터 끝까지(3까지) 뽑아서 슬라이스 함

5 : target 엔 0, 1, 2 값이 있음 (setosa 부터 0)

10 : 레이블값이 타겟과 같으면 1 true 틀리면 0 false

<div>

<img src="https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6cba01b2-9746-4dab-a888-2e8d4a860e1c/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-16_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_8.58.37.png" alt="">

 

<figure><img src="../../.gitbook/assets/스크린샷 2023-08-16 오후 8.58.37.png" alt=""><figcaption></figcaption></figure>

</div>

<div>

<img src="https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a948e017-dc78-42da-9835-28105d54b1f0/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-16_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.03.12.png" alt="">

 

<figure><img src="../../.gitbook/assets/스크린샷 2023-08-16 오후 9.03.12.png" alt=""><figcaption></figcaption></figure>

</div>

7,8 : 난수를 만들어서 초기화

<div>

<img src="https://s3-us-west-2.amazonaws.com/secure.notion-static.com/fd0abf60-8207-4216-9c67-9e23a426c1e5/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-16_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.03.40.png" alt="">

 

<figure><img src="../../.gitbook/assets/스크린샷 2023-08-16 오후 9.03.40.png" alt=""><figcaption></figcaption></figure>

</div>

<div>

<img src="https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5de23d4d-23dd-4e6d-b66b-1338cbc97a2a/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-16_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.05.52.png" alt="">

 

<figure><img src="../../.gitbook/assets/스크린샷 2023-08-16 오후 9.05.52.png" alt=""><figcaption></figcaption></figure>

</div>

활성함수식 사용함

<div>

<img src="https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9bb4007c-c896-4165-a9d8-d5875e3ed65f/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-16_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.08.37.png" alt="">

 

<figure><img src="../../.gitbook/assets/스크린샷 2023-08-16 오후 9.08.37.png" alt=""><figcaption></figcaption></figure>

</div>

이제 섞고 반복시켜서 학습 시킴

<div>

<img src="https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e2e4e916-f3e4-4591-b3b0-2fb5d23cb7f2/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-16_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.09.52.png" alt="">

 

<figure><img src="../../.gitbook/assets/스크린샷 2023-08-16 오후 9.09.52.png" alt=""><figcaption></figcaption></figure>

</div>

<div>

<img src="https://s3-us-west-2.amazonaws.com/secure.notion-static.com/29d0a1bc-61a6-4884-baca-20a20dac5e3f/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-16_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.17.59.png" alt="">

 

<figure><img src="../../.gitbook/assets/스크린샷 2023-08-16 오후 9.17.59.png" alt=""><figcaption></figcaption></figure>

</div>

loss 값은 0에 가까워야 정확함

<div>

<img src="https://s3-us-west-2.amazonaws.com/secure.notion-static.com/370bb94d-5caf-48cb-8442-9aa4745928f1/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-16_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.26.51.png" alt="">

 

<figure><img src="../../.gitbook/assets/스크린샷 2023-08-16 오후 9.26.51.png" alt=""><figcaption></figcaption></figure>

</div>

데이터의 최소 최대 범위를 좌표값으로 나타냄

<div>

<img src="https://s3-us-west-2.amazonaws.com/secure.notion-static.com/42386679-8934-4461-9149-0f7081ea1c10/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-16_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.28.49.png" alt="">

 

<figure><img src="../../.gitbook/assets/스크린샷 2023-08-16 오후 9.28.49.png" alt=""><figcaption></figcaption></figure>

</div>

가로 꽃잎길이 세로 꽃잎의 폭

어느 부분이 setosa에 포함되는지 범위를 볼수있음

<div>

<img src="https://s3-us-west-2.amazonaws.com/secure.notion-static.com/32d6a82f-4087-44c1-a961-539d372b90f8/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-16_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.28.27.png" alt="">

 

<figure><img src="../../.gitbook/assets/스크린샷 2023-08-16 오후 9.28.27.png" alt=""><figcaption></figcaption></figure>

</div>

<div>

<img src="https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1119c873-a1bd-433a-ac20-0346e70355e0/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-16_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.34.05.png" alt="">

 

<figure><img src="../../.gitbook/assets/스크린샷 2023-08-16 오후 9.34.05.png" alt=""><figcaption></figcaption></figure>

</div>

<div>

<img src="https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2580d87e-e321-45dd-acbf-514a4503949d/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-16_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.34.37.png" alt="">

 

<figure><img src="../../.gitbook/assets/스크린샷 2023-08-16 오후 9.34.37.png" alt=""><figcaption></figcaption></figure>

</div>

<div>

<img src="https://s3-us-west-2.amazonaws.com/secure.notion-static.com/39a94600-dbcc-4a4b-904a-8f81921d5e47/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-16_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.36.32.png" alt="">

 

<figure><img src="../../.gitbook/assets/스크린샷 2023-08-16 오후 9.36.32.png" alt=""><figcaption></figcaption></figure>

</div>

<div>

<img src="https://s3-us-west-2.amazonaws.com/secure.notion-static.com/44edb110-f5ed-4a31-95c5-8f6d30cde6e0/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-16_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.43.06.png" alt="">

 

<figure><img src="../../.gitbook/assets/스크린샷 2023-08-16 오후 9.43.06.png" alt=""><figcaption></figcaption></figure>

</div>

이제 훈련을 시키고 시각화를 시킴

<figure><img src="../../.gitbook/assets/스크린샷 2023-08-16 오후 9.42.54.png" alt=""><figcaption></figcaption></figure>

<div>

<img src="https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c90c1e24-ef9d-4aa4-8d41-d11aaa753f0d/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-16_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.44.38.png" alt="">

 

<figure><img src="../../.gitbook/assets/스크린샷 2023-08-16 오후 9.44.38.png" alt=""><figcaption></figcaption></figure>

</div>

<div>

<img src="https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0d5673f9-1beb-40bd-80d2-38cca29b7de1/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-16_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.46.04.png" alt="">

 

<figure><img src="../../.gitbook/assets/스크린샷 2023-08-16 오후 9.46.04.png" alt=""><figcaption></figcaption></figure>

</div>

<div>

<img src="https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2b8bcd13-a2fd-4570-a48a-3a0b467623ba/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-16_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.51.04.png" alt="">

 

<figure><img src="../../.gitbook/assets/스크린샷 2023-08-16 오후 9.51.04.png" alt=""><figcaption></figcaption></figure>

</div>

* 가중치의 초기값이 달라서 매번 실행할 때마다 좀 다를수 있음
* 분류는 잘 되는데 경계의 형태는 좀 달라짐
* 퍼셉트론은 단위 계단 함수를 사용하기 때문에 일반화 오류가 발생할 여지가 크다

#### 정리하기

* 인공신경망은 인간과 동물의 두뇌를 구성하는 생물학적 신경 시스템의 원리를 바탕으로 설계된 계산 시스템이다
* 신경망을 많은 수의 층으로 구성하면 더욱 높은 차원의 표현을 할 수 있다
* 딥러닝은 심층 신경망을 학습하기 위해 활용되는 기계학습 알고리즘이다
* 기본적인 뉴런의 구조는 다수의 입력이 각각 가중치를 적용하여 전달되면 이를 합산한 후 활성함수를 거쳐 출력을 만들어 내는 형태이다
* 일반적인 활성함수는 비선형 특성을 갖는 함수이며 입력이 0보다 작으면 출력을 억제하고 0보다 크면 출력을 내는 역할을한다
* 피드포워드 신경망은 입력층에서 출력층방향으로 뉴런 층의 연결이 이루어지는 구조이다
* 단층 퍼셉트론은 특정 공간을 선형 결정경계로 분할할 수 있다

#### 연습문제

<div>

<img src="https://s3-us-west-2.amazonaws.com/secure.notion-static.com/068c5a1a-6677-4178-b043-72686a33fca8/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-16_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.59.56.png" alt="">

 

<figure><img src="../../.gitbook/assets/스크린샷 2023-08-16 오후 9.59.56.png" alt=""><figcaption></figcaption></figure>

</div>

<div>

<img src="https://s3-us-west-2.amazonaws.com/secure.notion-static.com/29605c16-47f4-45e4-a83d-1854fcbfaba1/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-16_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_10.01.23.png" alt="">

 

<figure><img src="../../.gitbook/assets/스크린샷 2023-08-16 오후 10.01.23.png" alt=""><figcaption></figcaption></figure>

</div>

아래와 같은 수학문제가 나올수도 있다

<div>

<img src="https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2eb396ad-3988-4142-bc73-438bb8b419aa/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-08-16_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_10.03.20.png" alt="">

 

<figure><img src="../../.gitbook/assets/스크린샷 2023-08-16 오후 10.03.20.png" alt=""><figcaption></figcaption></figure>

</div>
