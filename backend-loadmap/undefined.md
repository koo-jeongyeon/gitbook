---
icon: location-minus
description: 전체 학습 메인페이지 (2024.08 시작)
---

# 메인페이지

## 백엔드 로드맵

* 로드맵을 참고하여 백엔드 개발에 관한 학습을 하고 기록한다
* 메인페이지에 전부 기록 후 어느정도 내용이 쌓이면 페이지를 분리한다
* 학습중 모르는 개념은 주황 형광표시하고 일단 넘어간뒤 필요할때 학습하고 기록한다
* 학습한 강의나 영상, 글의 출처를 전부 기록한다
* 로드맵 관련 CC. [https://zero-base.co.kr/event/media\_BE\_school\_roadmap](https://zero-base.co.kr/event/media\_BE\_school\_roadmap)

<figure><img src="../.gitbook/assets/백엔드-로드맵-한글판.png" alt=""><figcaption></figcaption></figure>

### 인터넷

* 인터넷의 작동원리
* HTTP란
* 브라우저와 그 작동원리
* DNS와 그 작동원리
* 도메인 이름이란
* 호스팅이란

***

### REST API

#### @RequestParam, @PathVariable, @RequestBody 적절한 사용

#### 1. **`@RequestParam`**: URL의 쿼리 파라미터를 통해 데이터를 전달

**특징**:

* URL 쿼리 스트링으로 데이터를 전달합니다. (예: `?key=value`)
* 주로 **필터링**, **검색**, **선택적 파라미터**에 사용됩니다.
* 여러 개의 필드를 선택적으로 전달할 수 있습니다.

**사용 상황**:

* **필터링**: 다수의 검색 조건이나 필터 조건을 적용할 때.
* **선택적 필드**: 파라미터가 필수가 아닌 경우.
* **간단한 데이터**: 정수나 문자열 같은 간단한 값들을 받을 때.

**예시**:

```java
java코드 복사@GetMapping("/invoices")
public ApiResponse<List<OrderInvoices>> getOrderInvoices(
        @RequestParam(value = "orderId", required = false) Long orderId,
        @RequestParam(value = "customerName", required = false) String customerName) {
    // 필터링 로직
    return new ApiResponse<>(orderInvoiceService.findInvoices(orderId, customerName));
}
```

**URL 예시**: `/invoices?orderId=123&customerName=John`

***

#### 2. **`@PathVariable`**: URL 경로의 일부로 데이터를 전달

**특징**:

* URL 경로에 포함된 값을 전달합니다. (예: `/invoices/{id}`)
* 특정 리소스를 식별할 때 사용합니다.
* **명확하게 하나의 리소스**를 대상으로 작업할 때 적합합니다.

**사용 상황**:

* **고유 식별자**: ID 등으로 리소스를 식별할 때.
* **리소스 조회, 업데이트, 삭제**: 리소스와 관련된 작업을 할 때.

**예시**:

```java
java코드 복사@GetMapping("/invoices/{orderId}")
public ApiResponse<OrderInvoices> getOrderInvoice(@PathVariable Long orderId) {
    // 단일 리소스 조회 로직
    return new ApiResponse<>(orderInvoiceService.findByOrderId(orderId));
}
```

**URL 예시**: `/invoices/123`

***

#### 3. **DTO 객체**: JSON 또는 Form 데이터로 복잡한 객체를 전달

**특징**:

* 요청 데이터를 **JSON 형식** 또는 **폼 데이터**로 받아서 객체로 매핑합니다.
* **복잡한 데이터 구조**를 처리할 수 있습니다.
* 주로 **POST, PUT, PATCH** 등의 요청에서 본문(body)에 데이터를 담아 전송합니다.

**사용 상황**:

* **복잡한 입력 데이터**: 여러 필드로 구성된 데이터를 받을 때.
* **객체 단위의 입력**: 요청 본문에 JSON으로 데이터를 받아야 할 때.
* **생성(create)** 또는 **수정(update)**: 데이터가 여러 필드로 구성된 경우.

**예시**:

```java
java코드 복사@PostMapping("/invoices")
public ApiResponse<OrderInvoices> createOrderInvoice(@RequestBody OrderInvoiceDto orderInvoiceDto) {
    // DTO로 받은 데이터를 처리
    return new ApiResponse<>(orderInvoiceService.create(orderInvoiceDto));
}
```

**JSON 예시**:

```json
json코드 복사{
  "orderId": 123,
  "divName": "배송사",
  "divCode": "ABC123",
  "sheetNo": "123456",
  "registedDate": "2024-09-06"
}
```

***

#### 차이점 요약 및 사용 가이드:

1. **`@RequestParam`**:
   * **데이터 전달 방식**: URL 쿼리 스트링.
   * **주로 사용되는 HTTP 메서드**: `GET` (검색, 필터링 시 사용).
   * **적용 상황**: 필터링, 검색 조건, 선택적 파라미터를 받을 때.
   * **예시**: `/invoices?orderId=123&customerName=John`
2. **`@PathVariable`**:
   * **데이터 전달 방식**: URL 경로의 일부.
   * **주로 사용되는 HTTP 메서드**: `GET`, `DELETE`, `PUT` (리소스 식별 시 사용).
   * **적용 상황**: 리소스의 고유 ID로 단일 리소스를 조회, 수정, 삭제할 때.
   * **예시**: `/invoices/123`
3. **DTO 객체 (`@RequestBody`)**:
   * **데이터 전달 방식**: 요청 본문(body)에 JSON 형식 또는 폼 데이터로 전달.
   * **주로 사용되는 HTTP 메서드**: `POST`, `PUT`, `PATCH` (복잡한 데이터 입력 시 사용).
   * **적용 상황**: 객체 형태로 여러 필드를 받거나, 대량의 데이터를 처리할 때.
   * **예시**: 복잡한 주문 정보를 포함한 JSON을 `POST`로 전달할 때.

CC. Chat GPT



***

### AWS

#### VPC 와 Subnet

<figure><img src="../.gitbook/assets/스크린샷 2024-08-31 오후 1.33.38.png" alt=""><figcaption></figcaption></figure>

* VPC 란
  * AWS 서비스는 외부에서 퍼블릭으로 접근이 가능하지만 VPC는 접근이 불가하다.
  * VPC가 AWS 서비스에 접근하려면 인터넷 게이트웨이를 통해 접근해야한다. AWS 클라우드 내부이지만 직접 접근하지 못한다.
  * VPC 는 외부와 격리된 네트워크를 만드는게 목적. Virtual Private Cloud 의 약자. AWS 계정 전용 가상 네트워크이다.  EC2 , RDS , Lambda 등의 AWS 컴퓨팅 서비스 실행시 사용.
* VPC의 구성요소&#x20;
  * 서브넷, 인터넷 게이트웨이, NACL/보안그룹, 라우트테이블, NAT instance/NAT gateway, Bastion Host, VPC endpoint 등
* 서브넷
  * VPC의 하위단위로 VPC에 할당된 IP를 더 작은 단위로 분할한 개념
  * 하나의 서브넷은 하나의 가용영역에 위치
  * <mark style="background-color:orange;">CIDR block range</mark> 로 IP 주소 지정

<figure><img src="../.gitbook/assets/스크린샷 2024-08-31 오후 1.39.37.png" alt=""><figcaption></figcaption></figure>



CC. [쉽게 설명하는 AWS 기초 강좌 16:VPC 와 Subnet](https://www.youtube.com/watch?v=WY2xoIClOFA\&t=660s)

***
