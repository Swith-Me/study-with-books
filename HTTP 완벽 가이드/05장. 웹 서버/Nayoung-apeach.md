# 05. 웹 서버

**5.1 다채로운 웹 서버**

'웹 서버' 라는 용어는 웹 서버 소프트웨어와 웹페이지 제공에 특화된 장비 양쪽 모두를 가리킨다.

**5.1.1 웹 서버 구현**

- 다목적 소프트웨어 웹 서버를 표준 컴퓨터 시스템에 설치하고 실행할 수 있다.
- 마이크로프로세서의 기적으로, 어떤 회사들은 사용자에게 판매할 전자기기 안에 몇 개의 컴퓨터 칩만으로 구현된 웹 서버를 내장시켜서 완전한 관리 콘솔로 제공 한다.

**5.1.2 다목적 소프트웨어 웹 서버**

- 모든 인터넷 웹 사이트의 37%가 마이크로소프트 웹 서버를 통해 서비스되고 있다.
- 아파치 웹 서버가 점유율 35%로, 최근 근소한 차이로 점유율 2위로 밀려났다.
- nginx 서버를 사용하는 사이트가 최근 수년간 꾸준히 증가하여 약 14%를 점유하고 있다.

**5.1.3 임베디드 웹 서버**

- 아이픽 성냥 머리 크기 웹 서버
- 넷미디어 사이트플레이어 sp1 이더넷 웹 서버

### 5.2 간단한 펄 웹 서버

- 먼저, 관리자는 특정 포트로 수신하는 type-o-serve를 이용해 어떻게 http통신을 테스트하는지 보여준다.
- type-o-serve가 동작하기 시작하면, 이 웹 서버에 브라우저로 접근할 수 있다.
- type-o-serve 프로그램은 브라우저로부터 http 요청 메시지를 받아 그 내용을 화면에 출력한 뒤, 관리자가 마침표 하나뿐인 줄로 끝나는 간단한 응답 메시지를 입력할 때까지 기다린다.
- type-o-serve는 http 응답 메시지를 브라우저에게 돌려주고, 브라우저는 응답 메시지의 본문을 출력한다.

### 5.3 진짜 웹 서버가 하는 일

1. 커넥션을 맺는다. → 클라이언트의 접속을 받아들이거나, 원치 않는 클라이언트라면 닫는다.
2. 요청을 받는다 → HTTP 요청 메시지를 네트워크로부터 읽어 들인다.
3. 요청을 처리한다. → 요청 메시지를 해석하고 행동을 취한다.
4. 리소스에 접근한다. → 메시지에서 지정한 리소스에 접근한다.
5. 응답을 만든다. → 올바른 헤더를 포함한 HTTP 응답 메시지를 생성한다.
6. 응답을 보낸다. → 응답을 클라이언트에게 돌려준다.
7. 트랜잭션을 로그로 남긴다. → 로그파일에 트랜잭션 완료에 대한 기록을 남긴다. 

**단계 1 : 클라이언트 커넥션 수락**

- 새 커넥션 다루기
- 클라이언트 호스트 명 식별
- ident를 통해 클라이언트 사용자 알아내기

**단계 2 : 요청 메시지 수신**

- 메시지의 내부 표현
- 커넥션 입력/출력 처리 아키텍처

**단계 3 : 요청 처리**

**단계 4 : 리소스의 매핑과 접근**

- Docroot
- 디렉토리 목록
- 동적 콘텐츠 리소스 매핑
- 접근 제어

**단계 5 : 응답 만들기**

- 응답 엔터티
- mime 타입 결정하기
- 리다이렉션

**단계 6 : 응답 보내기** 

**단계 7 : 로깅**
