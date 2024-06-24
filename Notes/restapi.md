---
created:
  2024-06-19
updated:
  2024-06-20
tags:
  - API
---

<link rel="stylesheet" href="../Assets/style.css" />

# REST API

## REST 등장 배경

- `REST`는 <span class="_pink _600">Representational State Transfer</span>의 약자
- 2000년 로이 필딩<span class="_small">Roy Feilding</span>의 박사학위 논문에 최초 소개됨
- 웹의 장점을 최대한 활용할 수 있는 아키텍처 목적

## REST 구성

- `RESOURCE` 자원, URI
- `Verb` 행위, HTTP method
- `Fepresentations` 표현

## REST 특징

1. Uniform
    - 유니폼 인터페이스
    - URI로 지정한 리소스에 대한 조작을 통일되고 한정적인 인터페이스로 수행하는 아키텍처 스타일
2. Stateless
    - 무상태성
    - 작업을 위한 상태 정보(세션 정보나 쿠키 정보)를 따로 저장하거나 관리하지 않음
    - API 서버는 요청만 단순히 처리할 수 있음
        - 서비스 자유도가 높아지고 서버에서 불필요한 정보를 관리하지 않아 구현이 단순해짐
3. Cacheable
    - 캐시 가능
    - HTTP 웹표준을 그대로 사용하기 때문에, 웹의 기존 인프라를 그대로 활용할 수 있음
        - HTTP 캐싱 기능 적용 가능
        - Last-Modified Tag 또는 E-Tag 사용(HTTP 프로토콜 표준)
4. Self-descriptiveness
    - 자체 표현 구조
    - REST API message 만으로 쉽게 이해할 수 있음
5. Client-Server
    - REST Server
        - API 제공
    - REST Client
        - 사용자 인증이나 컨텍스트(세션, 로그인 정보) 등을 직접 관리
    - 각 구조의 역할이 명확히 구분되어 클라이언트와 서버에서 개발할 내용이 분명하고 서로 의존성이 줄어듦
6. 계층형 구조
    - REST Server는 다중 계층으로 구성 가능
    - 보안, 로드 밸런싱, 암호화 계층을 추가해 구조 상 유연성 확보
    - PROXY, 게이트웨이 같은 네트워크 기반의 중간매체 사용 가능

## REST API 디자인 가이드

    URI는 정보 자원을 표현해야 함
    자원에 대한 행위는 HTTP Method(GET, POST, PUT, DELETE 등)로 표현함

### REST API 중심 규칙

1. URI는 정보의 자원을 표현해야 한다.
    - 리소스 명은 동사보다는 명사를 사용
    - <span class="_small _italic">cf.</span> REST를 적용하지 않은 URI: ~delete~(행위에 대한 표현)
        - `GET /members/delete/1`
2. 자원에 대한 행위는 HTTP Method(GET, POST, PUT, DELETE 등)로 표현해야 한다.
    - <span class="_small _italic">ex.</span> REST를 적용하지 않은 URI의 수정
        - `DELETE /members/1`
    - 참고로 회원정보를 가져올 때는 GET, 회원 등록 행위를 표현할 때는 POST Method로 표현함

- 회원정보를 가져오는 URI

```bash
GET /members/show/1  # (x)
GET /members/1  # (o)
```

- 회원을 추가하는 URI

```bash
GET /members/insert/2  # (x) - GET 메서드는 리소스 생성에 부적합
POST /members/2  # (o)
```

<details>
    <summary class="_pink _600">HTTP Method의 적절한 역할</summary>

- POST, GET, PUT, DELETE 4가지 Method로 CRUD가 가능

|메소드|역할|
|:-:|:-|
|POST|해당 URI를 요청하면 리소스를 생성|
|GET|해당 리소스를 조회하고 해당 도큐먼트에 대한 자세한 정보를 가져옴|
|PUT|해당 리소스를 수정|
|DELETE|리소스를 삭제|
</details>

### URI 설계 시 주의사항

1. `/` 구분자는 계층 구분을 위해 사용

```bash
http://restapi.example.com/houses/apartments
http://restapi.example.com/animals/mammals/whales
```

2. URI 마지막 문자로 `/` 구분자를 사용하지 않아야 함
    - URI가 다르면 리소스가 다르고 리소스가 다르면 URI가 달라져야 함
    - REST API는 분명한 URI로 통신할 수 있도록 혼동을 방지하기 위해 URI 경로 마지막에 `/`를 사용하지 않음

```bash
http://restapi.example.com/houses/apartments/  # (X)
http://restapi.example.com/houses/apartments   # (0)
```

3. URI 가독성을 높이기 위해 `-`을 사용
    - 불가피하게 긴 URI를 사용해야 하는 경우 `-`을 포함해 URI 읽고 해석하기 쉽게 만들어야 함
4. URI에는 `_`을 사용하지 않아야 함
    - 폰트 스타일에 따라 문자를 가리거나 식별이 불분명한 문제가 발생할 수 있음
5. URI 경로에는 소문자를 사용해야 함
    - URI 문법 형식 RFC 3986에 따르면 URI 스키마 및 호스트를 제외한 URI는 대소문자를 구분하도록 규정
    - 대소문자 혼용 문제로 다른 리소스로 오인식하지 않도록 대문자 사용을 지양해야 함

```bash
RFC 3986 is the URI (Unified Resource Identifier) Syntax document
```

6. URI 경로에 파일 확장자를 포함하지 않아야 함
    - 메시지 바디 내용 포맷을 나타낼 때는 Accept header를 사용

```bash
http://restapi.example.com/members/soccer/345/photo.jpg (X)

GET / members/soccer/345/photo HTTP/1.1 Host: restapi.example.com Accept: image/jpg
```

### 리소스 관계를 표현하는 방법

- REST 리소스 간 연관 관계가 있는 경우 `/리소스명/리소스 ID/관계가 있는 다른 리소스명` 형식으로 작성

```bash
GET : /users/{userid}/devices  # 일반적으로 소유 ‘has’의 관계를 표현할 때
```

- 만약에 관계명이 복잡하다면 이를 서브 리소스에 명시적으로 표현
    - 사용자가 ‘좋아하는’ 디바이스 목록을 표현해야 할 경우,

```bash
GET : /users/{userid}/likes/devices  # 관계명이 애매하거나 구체적 표현이 필요할 때
```

### 자원을 표현하는 Collection과 Document

- Document
    - 단순히 문서 또는 한 객체
- Collection
    - 문서들의 집합, 객체들의 집합
- collection과 document는 모두 리소스라고 할 수 있으며 URI에 표현됨

```bash
http:// restapi.example.com/sports/soccer  # sports라는 컬렉션과 soccer라는 도큐먼트
```

- collection과 document는 복수로 사용할 수 있음

```bash
http:// restapi.example.com/sports/soccer/players/13  # sports, players 컬렉션과 soccer, 13(13번 선수) 도큐먼트
```

## HTTP 응답 상태 코드

- 잘 설계된 REST API는 URI 설계 뿐만 아니라 리소스 응답도 고려해야 함
- 응답의 상태코드 값을 명확히 회신하여 추가적인 정보를 전달할 수도 있음

|상태코드||
|:-:|:-|
|200|클라이언트의 요청을 정상적으로 수행함|
|201|클라이언트가 어떠한 리소스 생성을 요청, 해당 리소스가 성공적으로 생성됨(POST를 통한 리소스 생성 작업 시)|

|상태코드||
|:-:|:-|
|400|클라이언트의 요청이 부적절할 경우 사용하는 응답 코드|
|401|클라이언트가 인증되지 않은 상태에서 보호된 리소스를 요청했을 때 사용하는 응답 코드|
||로그인하지 않은 유저가 로그인 했을 때, 요청 가능한 리소스를 요청했을 때|
|403|유저 인증 상태와 관계없이 응답하고 싶지 않은 리소스를 클라이언트가 요청했을 때 사용하는 응답 코드|
||403 보다는 400이나 404를 사용할 것을 권고, 403 자체가 리소스가 존재한다는 뜻이기 때문|
|405|클라이언트가 요청한 리소스에는 사용 불가능한 Method를 이용했을 경우 사용하는 응답 코드|

|상태코드||
|:-:|:-|
|301|클라이언트가 요청한 리소스에 대한 URI가 변경되었을 때 사용하는 응답 코드|
||응답 시 Location header에 변경된 URI를 적어줘야 함|
|500|서버에 문제가 있을 경우 사용하는 응답 코드|

# Reference

[참조](https://meetup.nhncloud.com/posts/92)