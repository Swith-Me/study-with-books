# 08. 통합점: 게이트웨이, 터널, 릴레이

## 8.0 Intro

- 브라우저 같은 HTTP 애플리케이션은 인터넷상의 콘텐츠에 접근하는 통일된 방법을 제공한다.

<br>

## 8.1 게이트웨이

- 인터프리터 같이 리소스를 받기 위한 경로를 안내하는 역할을 한다.
- 리소스와 애플리케이션을 연결하는 역할을 한다.

<br>

### 8.1.1 클라이언트 측 게이트웨이와 서버 측 게이트웨이

```
<클라이언트 프로토콜>/<서버 프로토콜>
```

- 서버 측 게이트웨이: 클라이언트와 HTTP로 통신하고, 서버와는 외래 프로토콜로 통신한다.
- 클라이언트 측 게이트웨이: 클라이언트와 외래 프로토콜로 통신하고, 서버와는 HTTP로 통신한다.

<br>

## 8.2 프로토콜 게이트웨이

- 브라우저는 일반 HTTP 트래픽은 원 서버로 바로 보낸다.
- FTP URL을 포함한 요청은 클라이언트에게 그 결과를 HTTP로 전송한다.

<br>

### 8.2.1 HTTP/\*: 서버 측 웹 게이트웨이

- USER와 PASS 명령을 보내서 서버에 로그인한다.
- 서버에서 적절한 디렉토리로 변경하기 위해 CWD 명령을 내린다.
- 다운로드 형식을 ASCII로 설정한다.
- MDTM으로 문서의 최근 수정 시간을 가져온다.
- PASV로 서버에게 수동형 데이터 검색을 하겠다고 말한다.
- RETR로 객체를 검색한다.
- 제어 채널에서 반환된 포트로 FTP 서버에 데이터 커넥션을 맺는다.
- 데이터 채널이 열리는 대로, 객체가 게이트웨이로 전송된다.

<br>

### 8.2.2 HTTP/HTTPS: 서버 측 보안 게이트웨이

- 기업 내부의 모든 웹 요청을 암호화함으로써 개인 정보 보호와 보안을 제공하는 데 게이트웨이를 사용할 수 있다.

<br>

### 8.2.3 HTTPS/HTTP: 클라이언트 측 보안 가속 게이트웨이

- HTTPS/HTTP 게이트웨이는 웹 서버의 앞단에 위치하고 보이지 않는 인터셉트 게이트웨이나 리버스 프락시 역할을 한다.

<br>

## 8.3 리소스 게이트웨이

- 게이트웨이의 가장 일반적인 형태인 애플리케이션 서버는 목적지 서버와 게이트웨이를 한 개의 서버로 결합한다.
- 애플리케이션 게이트웨이에서 유명했던 최초의 API는 공용 게이트웨이 인터페이스였다. (CGI)
- 웹 개발자가 자신의 모듈을 HTTP와 직접 연결할 수 있는 강력한 인터페이스인 서버 확장 API를 제공하였다.

<br>

## 8.4 애플리케이션 인터페이스와 웹 서비스

- 웹 서비스는 SOAP을 통해 XML을 사용하여 정보를 교환한다.
- XML은 데이터 객체를 담는 데이터를 생성하고 해석하는 방식을 제공한다.
- SOAP은 HTTP 메시지에 XML 데이터를 담는 방식에 관한 표준이다.

<br>

## 8.5 터널

- HTTP의 또 다른 사용 방식, 웹 터널
- HTTP 프로토콜을 지원하지 않는 애플리케이션에 HTTP 애플리케이션을 사용해 접근하는 방법을 제공한다.

<br>

## 8.6 릴레이

- HTTP 릴레이는 HTTP 명세를 완전히 준수하지는 않는 간단한 HTTP 프락시다.
- 릴레이는 커넥션을 맺기 위한 HTTP 통신을 한 다음, 바이트를 맹목적으로 전달한다.
- 단순 맹목적 릴레이를 구현하는데 관련된 더 일반적인 문제 중 하나
  - 맹목적 릴레이가 Connection 헤더를 제대로 처리하지 못해서 `keep-alive` 커넥션이 행에 걸리는 것이다.
