# 12. 기본 인증

<br>

## 12.0 Intro

- 허가된 사람만이 데이터에 접근하고 업무를 처리할 수 있어야 한다.
- 예를 들면, 개인의 프로필이나 개인이 작성한 문서는 해당 소유자의 동의 없이는 볼 수 없어야 한다.
- 그러기 위해서는 서버가 사용자가 누구인지 식별할 수 있어야 한다.
- 이 장에서는 HTTP 인증과 그것이 기본이 되는 기본 인증을 알아볼 것이다.

<br>

## 12.1 인증

- 인증은 당신이 누구인지 증명하는 것이다.
- 완벽한 인증은 없다. 하지만 당신에 대한 여러 데이터는 당신이 누구인지 판단하는데 도움이 된다.

<br>

### 12.1.1 HTTP의 인증요구/응답 프레임워크

- HTTP는 사용자 인증을 하는 데 사용하는 자체 인증요구/응답 프레임워크를 제공한다.
- 웹 애플리케이션이 HTTP 요청 메시지를 받으면, 서버는 요청을 처리하는 대신에 현재 사용자가 누구인지를 알 수 있게 비밀번호 같이 개인 정보를 요구하는 '인증 요구'로 응답할 수 있다.

<br>

> <img src="https://blog.kakaocdn.net/dn/cdzmks/btqDRLcUdeU/DAhSn3NaHvOOYFj3R3crEk/img.jpg">
>
> 출처: https://chaengstory.tistory.com/15

<br>

### 12.1.2 인증 프로토콜과 헤더

- HTTP는 필요에 다라 고쳐 쓸 수 있는 제어 헤더를 통해, 다른 인증 프로토콜에 맞추어 확장할 수 있는 프레임워크를 제공한다.
- HTTP에는 기본 인증과 다이제스트 인증이라는 두 가지 공식적인 인증 프로토콜이 있다.

1. 서버가 사용자에게 인증요구를 보낼 때, 서버는 401 Unauthorized 응답과 함께 WWW-Authenticate 헤더를 기술해서 어디서 어떻게 인증할지 설명한다.
2. 클라이언트가 서버로 인증하려면, 인코딩된 비밀번호와 그 외 인증 파라미터들을 Authorization 헤더에 담아서 요청을 다시 보낸다.

<br>

### 12.1.3 보안 영역

- 웹 서버는 기밀문서를 보안 영역(realm) 그룹으로 나누다.
- 보안 영역은 저마다 다른 사용자 권한을 요구한다.

<br>

## 12.2 기본 인증

- 기본 인증은 가장 잘 알려진 HTTP 인증 규약이다.
- 거의 모든 주요 클라이언트와 서버에 기본 인증이 구현되어 있다.
- 기본 인증에서, 웹 서버는 클라이언트의 요청을 거부하고 유효한 사용자 이름과 비밀번호를 요구할 수 있다.

<br>

|인증요구/응답|헤더 문법|
|:------:|:---:|
|인증요구 (서버에서 클라이언트로)|WWW-Authenticate: Basic realm=따음표로 감싼 문서집합|
|응답 (클라이언트에서 서버로)|	Authorization: Basic base-64로 인코딩한 이름과 비밀번호|

<br>

### 12.2.2 Base-64 사용자 이름/비밀번호 인코딩

- HTTP 기본 인증은 사용자 이름과 비밀번호를 콜론으로 이어서 합치고, base-64 인코딩 메서드를 사용해 인코딩 한다.
- base-64 인코딩은 8비트 바이트로 이루어져 있는 시퀀스를 6비트 덩어리의 시퀀스로 변환한다.
- 각 6비트 조각은 대부분 문자와 숫자로 이루어진 특별한 64개의 문자 중에서 선택된다.

<br>

### 12.2.3 프락시 인증

- 중개 프락시 서버를 통해 인증할 수도 있다.
- 프락시 서버에서 접근 정책을 중앙 관리 할 수 있기 때문에, 회사 리소스 전체에 대해 통합적인 접근 제어를 하기 위해서 사용하기도 한다.

<br>

### 12.3 기본 인증의 보안 결함

- 기본 인증은 일반적인 환경에서 개인화나 접근을 제어하는데 편리하다.
- 다른 사람들이 보지 않기를 원하기는 하지만, 보더라도 치명적이지 않은 경우에는 여전히 유용하다.
- 이렇게 기본 인증은 호기심 많은 사용자가 우연이나 사고로 정보를 접근해서 보는 것을 예방하는 데 사용한다.