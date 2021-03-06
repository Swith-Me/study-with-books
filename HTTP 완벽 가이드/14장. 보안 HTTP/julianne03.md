# 14. 보안 HTTP

## 14.1 HTTP를 안전하게 만들기

- 중요한 트랜잭션을 위해서는, HTTP와 디지털 암호화 기술을 결합해야 한다.
- 우리는 다음을 제공해 줄 수 있는 HTTP 보안 기술이 필요하다.
    - 서버 인증
    - 클라이언트 인증
    - 무결성
    - 암호화
    - 효율
    - 편재성
    - 관리자 확장성
    - 적응성
    - 사회적 생존성

<br>

### 14.1.1 HTTPS

- HTTPS는 HTTP를 안전하게 만드는 방식 중에서 가장 인기 있는 것이다.
- HTTPS를 사용할 때, 모든 HTTP 요청과 응답 데이터는 네트워크로 보내지기 전에 암호화된다.
- HTTPS는 HTTP의 하부에 전송 레벨 암호 보안 계층을 제공함으로써 동작한다.
    - 보안 계층의 종류: SSL, TLS (둘은 매우 비슷)
- 대부분의 경우, TCP 입력/출력 호출을 SSL 호출로 대체하고, 보안 정보를 설정하고 관리하기 위한 몇 가지 호출을 추가하기만 하면 된다.

<br>

## 14.2 디지털 암호학

- 디지털 암호의 기초
    - 암호
    - 키
    - 대칭키 암호 체계
    - 비대칭키 암호 체계
    - 공개키 암호법
    - 디지털 서명
    - 디지털 인증서

<br>

### 14.2.1 비밀 코드의 기술과 과학

- 암호법은 메시지 인코딩과 디코딩에 대한 과학이자 기술이다.
- 메시지를 암호화하는 것뿐 아니라, 메시지의 변조를 방지, 증명하는 데도 사용될 수 있다.

<br>

### 14.2.2 암호

- 암호법은 암호라 불리는 비밀 코드에 기반한다.
- 암호란 메시지를 인코딩하는 어떤 특정한 방법과 나중에 그 비밀 메시지를 디코딩하는 방법이다.
- 텍스트 or 평문: 인코딩되기 전의 원본 메시지 (ex. Meet me at midnight)
- 암호문: 암호가 적용되어 코딩된 메시지 (ex. Phhw ph dw plgqlikw)

<br>

### 14.2.3 암호 기계

- 기술이 진보하면서, 사람들은 보다 복잡한 암호로 메시지를 빠르고 정확하게 인코딩하고 디코딩하는 기계를 만들기 시작했다.

<br>

### 14.2.4 키가 있는 암호

- 대부분의 기계들에는 암호의 동작방식을 변경할 수 있는 큰 숫자로 된 다른 값을 설정할 수 있는 다이얼을 키라고 부른다.
- 서로 다른 키 값을 갖고 있으면 다르게 동작한다.

<br>

### 14.2.5 디지털 암호

- 기계 장치의 물리적인 금속 키나 다이얼 설정과는 달리, 디지털 키는 그냥 숫자에 불과하다.
- 코딩 알고리즘은 데이터 덩어리를 받아서 알고리즘과 키의 값에 근거하여 인코딩하거나 디코딩하는 함수이다.

<br>

## 14.3 대칭키 암호법

- 많은 디지털 암호 알고리즘은 대칭키 암호라 불리는데, 왜냐하면 그들이 인코딩할 때 사용하는 키가 디코딩을 할 때와 같기 때문이다.
- 대칭키 암호에서, 발송자와 수신자 모두 통신을 위해 비밀 키 k를 똑같이 공유할 필요가 있다.

<br>

### 14.3.1 키 길이와 열거 공격

- 대부분의 경우, 인코딩 및 디코딩 알고리즘은 공개적으로 알려져 있으므로, 키만이 유일한 비밀이다.
- 무차별로 모든 키 값을 대입해보는 공격을 열거 공격이라고 한다.
- 128비트 키를 사용한 대칭키 암호는 매우 강력한 것으로 간주된다.

<br>

### 14.3.2 공유키 발급하기

- 대칭키 암호의 단점 중 하나는 발송자와 수신자가 서로 대화하려면 둘 다 공유키를 가져야 한다는 것이다.

<br>

## 14.4 공개키 암호법

- 한 쌍의 호스트가 하나의 인코딩/디코딩 키를 사용하는 대신, 공개키 암호 방식은 두 개의 비대칭 키를 사용한다.
- 인코딩 키는 모두를 위해 공개되어 있다.
- 하지만 호스트만이 개인 디코딩 키를 알고 있다.
- 공개키 암호화 기술은 보안 프로토콜을 전 세계의 모든 컴퓨터 사용자에게 적용하는 것을 가능하게 했다.

<br>

### 14.4.1 RSA

- 공개키 비대칭 암호의 과제는 설혹 악당이 아래 내용을 알고 있다 해도 비밀인 개인 키를 계산할 수 없다는 것을 확신시켜 주는 것이다.
    - 공개키
    - 가로채서 얻은 암호문의 일부
    - 메시지와 그것을 암호화한 암호문
- 이 모든 요구를 만족하는 공개키 암호 체계 중 유명한 하나는 **RSA 알고리즘**이다.

<br>

## 14.5 디지털 서명

- 디지털 서명이라 불리는 이 기법은 다음 절에서 논의할 인터넷 보안 인증서에게 중요하다.
- 디지털 서명은 보통 비대칭 공개키에 의해 생성된다.

<br>

### 14.5.1 서명은 암호 체크섬이다

- 디지털 서명은 메시지에 붙어있는 특별한 암호 체크섬이다.
- 두 가지 이점
    - 서명은 메시지를 작성한 저자가 누군지 알려준다.
    - 서명은 메시지 위조를 방지한다.

<br>

## 14.6 디지털 인증서

- 디지털 인증서는 신뢰할 수 있는 기관으로부터 보증 받은 사용자나 회사에 대한 정보를 담고 있다.

<br>

### 14.6.1 인증서의 내부

- 기본적인 디지털 인증서는 보통 다음과 같이 인쇄된 ID에도 흔히 들어가게 되는 기본적인 것들을 담고 있다.
-  추가적으로, 디지털 인증서는 대상과 사용된 서명 알고리즘에 대한 서술적인 정보뿐 아니라 보통 대상의 공개키도 담고 있다.
    - 대상의 이름
    - 유효 기간
    - 인증서 발급자
    - 인증서 발급자의 디지털 서명

<br>

### 14.6.3 서버 인증을 위해 인증서 사용하기

- 사용자가 HTTPS를 통한 안전한 웹 트랜잭션을 시작할 때, 최신 브라우저는 자동으로 접속한 서버에서 디지털 인증서를 가져온다.
- 서버 인증서는 다음을 포함한 많은 필드를 갖고 있다.
    - 웹 사이트의 이름과 호스트 명
    - 웹 사이트의 공개키
    - 서명 기관의 이름
    - 서명 기관의 서명

<br>

## 14.7 HTTPS의 세부사항

- HTTPS는 HTTP 프로토콜에 대칭, 비대칭 인증서 기반 암호 기법의 강력한 집합을 결합한 것이다.
- HTTPS는 인터넷 애플리케이션의 성장을 가속한 동시에 웹 기반 전자상거래의 고속 성장을 이끄는 주력이다.

<br>

### 14.7.1 HTTPS 개요

- HTTPS는 그냥 보안 전송 계층을 통해 전송되는 HTTP이다.
- HTTPS는 HTTP 메시지를 TCP로 보내기 전에 먼저 그것들을 암호화하는 보안 계층으로 보낸다.
- HTTPS의 보안 계층은, SSL과 그것의 현대적인 대체품인 TLS로 구현되었다.

<br>

### 14.7.2 HTTPS 스킴

- HTTPS 프로토콜에서 URL의 스킴 접두사는 https다.
- HTTPS의 기본 포트는 443번이다.
- HTTPS는 서버와 바이너리 포맷으로 된 몇몇 SSL 보안 매개변수를 교환하면서 '핸드셰이크'를 하고, 암호화된 HTTP 명령이 뒤를 잇는다.

<br>

### 14.7.4 SSL 핸드셰이크

- 암호화된 HTTP 메시지를 보낼 수 있게 되기 전에, 클라이언트와 서버는 SSL 핸드셰이크를 할 필요가 있다.
- 핸드셰이크에서는 다음과 같은 일이 일어난다.
    - 프로토콜 버전 번호 교환
    - 양쪽이 알고 있는 암호 선택
    - 양쪽의 신원을 인증
    - 채널을 암호화하기 위한 임시 세션 키 생성

<br>

### 14.7.5 서버 인증서

- 오늘날, 클라이언트 인증서는 웹 브라우징에선 흔히 쓰이지 않는다.
- 한편, 보안 HTTPS 트랜잭션은 항상 서버 인증서를 요구한다.
- 서버 인증서는 조직의 이름, 주소, 서버 DNS 도메인 이름, 그리고 그 외의 정보를 보여주는 X.509 v3에서 파생된 인증서다.

<br>

## 14.8 진짜 HTTPS 클라이언트
- SSL은 복잡한 바이너리 프로토콜이다.
- 몇 가지 SSL 클라이언트와 서버 프로그래밍을 쉽게 만들어주는 오픈 소스 라이브러리들이 존재한다.

- OpenSSL (생략)
- 간단한 HTTPS 클라이언트 (생략)
- 단순한 OpenSSL 클라이언트 실행하기 (생략)

<br>

## 14.9 프락시를 통한 보안 트래픽 터널링

- HTTPS가 프락시와도 잘 동작할 수 있게 하기 위해, 클라이언트가 프락시에게 어디에 접속하려고 하는지 말해주는 방법을 약간 수정해야 한다.
- 인기 있는 기법 중 하나가 HTTPS SSL 터널링 프로토콜이다,
- HTTP는 CONNECT라 불리는 새로운 확장 메서드를 이용해서 평문으로 된 종단 정보를 전송하기 위해 사용된다.

```
CONNECT home.netscape.com:443 HTTP/1.0
User-agent: Mozilla/1.1N
```
