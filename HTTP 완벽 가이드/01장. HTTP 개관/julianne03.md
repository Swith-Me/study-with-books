# 01. HTTP 개관

## 1.0 Intro

- 전 세계 웹브라우저, 서버, 웹 애플리케이션은 모두 **HTTP**를 통해 서로 대화한다.
- HTTP는 현대 인터넷의 공용어이다.

<br>

## 1.1 HTTP: 인터넷의 멀티미디어 배달부

- HTTP는 신뢰성 있는 데이터 전송 프로토콜을 사용한다.
- 사용자는 인터넷에서 얻은 정보가 손상된 게 아닌지 염려하지 않아도 된다.

<br>

## 1.2 웹 클라이언트 서버

- 웹 콘텐츠는 **웹 서버**에 존재
- 웹 서버는 HTTP 프로토콜로 의사소통하기 때문에 보통 **HTTP 서버**라고 불린다.
- **클라이언트**: 서버에게 HTTP 요청을 보낸다.
- **웹 서버**: 인터넷의 데이터 저장 및 클라이언트가 요청한 데이터 제공
- 위에서 설명한 클라이언트와 웹 서버가 WWW(World Wide Web)의 기본 요소

<br>

> **만약 `https://www.naver.com/index.html` 로 접속한다면**
>
> 1. 웹브라우저가 HTTP 요청을 `www.naver.com` 서버로 보낸다.
> 2. 서버는 `index.html`을 찾아 응답 정보를 HTTP 응답에 실어서 클라이언트로 보낸다.

<br>

## 1.3 리소스

- **웹 리소스**란? 웹에 콘텐츠를 제공하는 모든 것
- 웹 서버는 웹 리소스를 관리하고 제공한다.
- 리소스는 요청에 따라 다른 콘텐츠를 생성한다. (ex. 동적 파일)

<br>

### 1.3.1 미디어 타입

- HTTP는 웹에서 전송되는 모든 객체 각각에 신중하게 **MIME 타입**을 붙인다.
- **MIME 타입**: 데이터 포맷 라벨
- 웹브라우저는 서버로부터 객체를 돌려받을 때, 다룰 수 있는 객체인지 MIME 타입을 통해 확인한다.
- MIME 타입 문법: 주 타입/부 타입
  - HTML로 작성된 텍스트 문서: `text/html`
  - JPEG 이미지: `image/jpeg`

<br>

### 1.3.2 URI

- 웹 서버의 각 리소스 이름을 **URI**라고 부른다.
- 정보 리소스를 고유하게 식별하고 위치를 지정할 수 있다. (ex. `http://www.google.com/imgs/seungmin.jpeg`)
- URI의 두 가지 종류: `URL`, `URN`

#### URL

: 프로토콜, 서버, 리소스를 명시한다.  
: 리소스의 정확한 위치와 접근방법을 알려준다.

> <img width="640" alt="url description" src="https://user-images.githubusercontent.com/59498977/129055084-8c48a9db-2e1d-4b4c-89b6-6584a7a50ae1.png">

<br>

#### URN

: 특정 서버의 한 리소스에 대한 구체적인 위치를 서술한다.  
: 콘텐츠를 이루는 한 리소스에 대해, 그 리소스의 위치에 영향 받지 않는 유일무이한 이름 역할

<br>

## 1.4 트랜잭션

- HTTP 트랜잭션은 요청 명령과 응답 결과로 구성되어 있다.
- 이 상호작용은 **HTTP 메시지**라고 불리는 데이터 덩어리를 이용해 이루어진다.

> <img width="500" alt="transaction description" src="https://user-images.githubusercontent.com/59498977/129126141-93358150-2489-4bb8-b988-a1817a32dc8f.png"><br>
> 출처: https://velog.io/@codemcd/HTTP-%EB%A9%94%EC%8B%9C%EC%A7%80-lqk14ernft

<br>

### 1.4.1 메서드

- HTTP는 **HTTP 메서드**라고 불리는 여러 가지 종류의 요청 명령을 지원한다.
- 모든 HTTP 요청 메시지는 한 개의 메서드를 갖는다.
- 메서드의 역할: 서버에게 어떤 동작이 취해져야 하는지 말해준다.

> **HTTP 메서드 종류**
>
> <img width="400" alt="http method" src="https://user-images.githubusercontent.com/59498977/129127481-a1a3941e-9716-4537-a098-2fb7332d5be9.png">

<br>

### 1.4.2 상태 코드

- 모든 HTTP 응답 메시지는 상태 코드와 함께 반환된다.
- 상태 코드의 역할: 클라이언트에게 요청의 성공 여부 및 추가 조치가 필요한지 알려준다.

> **자주 쓰이는 상태 코드 종류**
>
> <img width="400" alt="status code" src="https://user-images.githubusercontent.com/59498977/129127905-623f7040-7a55-48cc-8cc8-b846677be223.png">

<br>

```
// HTTP는 각 숫자 상태 코드에 텍스트로 된 사유 구절도 함께 보낸다.

200 OK
200 Document attached
200 Success
```

<br>

### 1.4.3 웹페이지는 여러 객체로 이루어질 수 있다

- 애플리케이션은 보통 하나의 작업을 수행하기 위해 여러 HTTP 트랜잭션을 수행한다.
- 웹페이지는 첨부된 리소스들에 대해 각각 별개의 HTTP 트랜잭션을 필요로 한다.
- 웹페이지는 **리소스의 모음**

<br>

## 1.5 메시지

- 요청 메시지: 웹 클라이언트 -> 웹 서버로 HTTP 메시지
- 응답 메시지: 웹 서버 -> 웹 클라이언트 HTTP 메시지

> <img width="300" src="https://user-images.githubusercontent.com/59498977/129129287-ba2b30c7-275d-4b31-900a-94897974d73d.png"><br>
> 요청 메시지 구조
>
> <img width="300" src="https://user-images.githubusercontent.com/59498977/129129297-56e71753-121d-4431-a718-a37b32fe3a3e.png"><br>
> 응답 메시지 구조
>
> 출처: https://heegyukim.medium.com/computer-network-3-http-d85014120b05

<br>

**1. 시작줄**

- 요청이라면 무엇을 해야 하는지
- 응답이라면 무슨 일이 일어났는지

**2. 헤더**

- 시작줄 다음에는 0개 이상의 헤더 필드가 이어진다.
- 이름: 값 (ex. `Accept: text/*`)
- 헤더는 빈 줄로 끝난다.

**3. 본문**

- 요청의 본문이라면 웹 서버로 데이터를 실어 보내고
- 응답의 본문이라면 클라이언트로 데이터를 반환
- 임의의 텍스트, 이진 데이터를 포함할 수 있다.

<br>

## 1.6 TCP 커넥션

- **TCP**란? 전송 제어 프로토콜
- 메시지는 TCP 커넥션을 통해 한 곳에서 다른 곳으로 옮겨진다.

<br>

### 1.6.1 TCP/IP

- HTTP는 네트워크 통신의 핵심적인 세부사항을 대중적이고 신뢰성 있는 `TCP/IP` 프로토콜에 맡긴다.
- TCP/IP는 패킷 교환 네트워크 프로토콜의 집합이다.
- 각 네트워크와 하드웨어의 특성을 숨긴다.
- 어떤 종류의 컴퓨터나 네트워크든 서로 신뢰성 있는 의사소통을 하게 해 준다.

> <img width="350" alt="스크린샷 2021-08-12 오전 11 54 57" src="https://user-images.githubusercontent.com/59498977/129131338-bea9ff2b-21e5-443b-b262-066563c87652.png"><br>
> HTTP 네트워크 프로토콜 스택

<br>

**TCP가 제공하는 것들**

1. 오류 없는 데이터 전송
2. 순서에 맞는 전달 (데이터는 언제나 보낸 순서대로 도착한다.)
3. 조각나지 않는 데이터 스트림 (언제든 어떤 크기로든 보낼 수 있다.)

<br>

### 1.6.2 접속, IP 주소 그리고 포트번호

- 클라이언트가 서버에 메시지를 전송하기 전, **IP 주소**와 **포트번호**를 사용해 클라이언트와 서버 사이에 **TCP/IP 커넥션**을 맺어야 한다.
- IP 주소와 포트 번호 알아내는 방법

  ```
  http://207.200.83.29:80/index.html

  IP 주소: 207.200.83.29
  포트 번호: 80

  http://www.netscape.com:80/index.html

  IP 주소: www.netscape.com의 별칭을 가지고 있는 IP 주소
  포트 번호: 80

  http://www.netscape.com/index.html

  IP 주소: www.netscape.com의 별칭을 가지고 있는 IP 주소
  포트 번호: 80 (생략됨 - 기본값이 80)
  ```

  <br>

**웹브라우저가 사용자에게 HTML 리소스를 보여주는 과정**

1. 웹브라우저는 서버의 URL에서 호스트 명을 추출한다.
2. 웹브라우저는 서버의 호스트 명을 IP로 반환한다.
3. 웹브라우저는 URL에서 포트번호를 추출한다.
4. 웹브라우저는 웹 서버와 TCP 커넥션을 맺는다.
5. 웹브라우저는 서버에 HTTP 요청을 보낸다.
6. 서버는 웹브라우저에 HTTP 응답을 돌려준다.
7. 커넥션이 닫히면, 웹브라우저는 문서를 보여준다.

<br>

### 1.7 프로토콜 버전의 역사

#### HTTP/0.9

- 심각한 디자인 결함이 다수 존재
- 구식 클라이언트하고만 같이 사용 가능
- 오직 GET 메서드만 지원
- MIME 타입, HTTP 헤더, 버전 번호 지원 X

#### HTTP/1.0

- 처음으로 널리 쓰이기 시작한 HTTP 버전
- 버전 번호, HTTP 헤더, 추가 메서드, 멀티미디어 객체 처리 추가됨

#### HTTP/1.0+

- `keep-alive` 커넥션, 가상 호스팅 지원, 프락시 연결 지원 등을 포함한 많은 기능이 추가됨

#### HTTP/1.1

- 더 복잡해진 웹 애플리케이션과 배포 지원
- 현재 대부분 사용 중인 HTTP 버전

#### HTTP/2.0

- 이전 버전의 성능 문제를 개선하기 위해 설계가 진행중임

<br>

## 1.8 웹의 구성 요소

### 1.8.1 프락시

- 클라이언트와 서버 사이에서 트래픽을 전달한다.
- 주로 보안을 위해 사용된다.
- 요청과 응답을 필터링한다. (ex. 바이러스 차단)

<br>

### 1.8.2 캐시

- 많이 찾는 웹페이지를 클라이언트 가까이에 보관하는 HTTP 창고

<br>

### 1.8.3 게이트웨이

- 다른 서버들의 중개자로 특별한 서버
- 주로 HTTP 트래픽을 다른 프로토콜로 변환하기 위해 사용된다.
- 언제나 진짜 서버인 것처럼 요청을 다룬다.

> <img width="300" alt="스크린샷 2021-08-12 오후 12 31 40" src="https://user-images.githubusercontent.com/59498977/129134351-b1f92aae-2cdb-4f55-abda-ba87e88ff60f.png"><br>
> HTTP/FTP 게이트웨이<br>
> 출처: https://chaengstory.tistory.com/11

<br>

### 1.8.4 터널

- 두 커넥션 사이에서 날(raw) 데이터를 그대로 전달해주는 HTTP 애플리케이션
- 주로 비 HTTP 데이터를 하나 이상의 HTTP 연결을 통해 그대로 전송해주기 위해 사용된다.

<br>

### 1.8.5 에이전트

- 사용자를 위해 HTTP 요청을 만들어주는 클라이언트 프로그램
- 웹 요청을 만드는 애플리케이션은 모두 HTTP 에이전트이다.
