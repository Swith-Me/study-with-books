# 07. 캐시

## 7.0 Intro

- 웹 캐시는 자주 쓰이는 문서의 사본을 자동으로 보관하는 HTTP 장치

<br>

## 7.1 불필요한 데이터 전송

- 캐시를 사용하면 첫 번째 서버 응답은 캐시에 보관된다.
- 캐시된 사본이 뒤이은 요청들에 대한 응답으로 사용될 수 있다.

<br>

## 7.2 대역폭 병목

- 캐시는 또한 네트워크 병목을 줄여준다.
- 만약 클라이언트가 빠른 LAN에 있는 캐시로부터 사본을 가져온다면, 캐싱은 성능을 대폭 개선할 수 있다.

<br>

## 7.3 갑작스런 요청 쇄도

- 캐싱은 많은 사람이 거의 동시에 웹 문서에 접근할 때 이를 대처하기 위해 중요하게 사용된다.

<br>

## 7.4 거리로 인한 지연

- 캐시를 설치해서 문서가 전송되는 거리르 매우 줄일 수 있다.

<br>

## 7.5 적중과 부적중

- 캐시 적중: 캐시에 요청이 도착했을 때 만약 그에 대응하는 사본이 잇따면 그를 이용해 요청을 처리하는 것
- 캐시 부적중: 대응하는 사본이 없다면 원 서버로 전달되기만 하는데 이를 캐시 부적중이라고 한다.

<br>

### 7.5.1 재검사

- 원 서버 콘텐츠는 변경될 수 있기 때문에 캐시는 반드시 그들이 갖고 있는 사본이 여전히 최신인지 서버를 통해 때때로 점검해야 한다.
- 캐시는 스스로 원한다면 언제든지 사본을 재검사할 수 있다.
- 서버 객체가 변경되지 않았다면 `HTTP 304 Not Modified` 응답을 보낸다.
- 캐시된 사본과 다르다면, `HTTP 200 OK` 응답을 보낸다.
- 삭제되었다면, `404 Not Found` 응답을 돌려보내며 캐시는 사본을 삭제한다.

<br>

### 7.5.2 적중률

- 캐시가 요청을 처리하는 비율을 캐시 적중률 또는 문서 적중률이라고 부르기도 한다.

<br>

### 7.5.3 바이트 적중률

- 캐시를 통해 제공된 모든 바이트의 비율을 표현한다.

<br>

### 7.5.4 적중과 부적중의 구별

- Data 헤더 값을 현재 시각과 비교하여, 응답의 생성일이 더 오래되었다면 클라이언트는 응답이 캐시된 것임을 알아낼 수 잇따.
- Age 헤더 이용

<br>

## 7.6 캐시 토폴로지

**개인 전용 캐시**

- 한 명에게만 할당된 캐시, 개인만을 위한 캐시
- 한 명의 사용자가 자주 찾는 페이지를 담는다.

<br>

**공용 프락시 캐시**

- 여러 사용자들 간에 공유된 캐시
- 사용자 집단에게 자주 쓰이는 페이지를 담는다.

<br>

**프락시 캐시 계층들**

- 1단계 캐시에서 적중을 얻는다면 다행
- 그렇지 못했다면 더 큰 부모 캐시가 사용자의 요청을 처리할 수도 있다.
- 캐시 계층이 깊다면 요청은 캐시의 긴 연쇄를 따라가게 될 것이다.

<br>

**캐시망, 콘텐츠 라우팅, 피어링**

- 몇몇 네트워크 아키텍처는 단순한 캐시 계층 대신 복잡한 캐시망을 만든다.

<br>

## 7.7 캐시 처리 단계

> <img src="https://thisblogfor.me/static/26964d32468e2e565e43d36b722855b8/ad34c/cache-processing-flow-chart.png" width="400">
>
> 출처: https://thisblogfor.me/web/http/cache/

<br>

**1.요청 받기**

- 캐시는 네트워크로부터 도착한 요청 메시지를 읽는다.

**2.파싱**

- 캐시는 메시지를 파싱하여 URL과 해더들을 추출한다.

**3.검색**

- 캐시는 로컬 복사본이 있는지 검사하고, 사본이 없다면 사본을 받아온다. (그리고 로컬에 저장한다.)

**4.신선도 검사**

- 캐시는 캐시된 사본이 충분히 신선한지 검사하고, 신선하지 않다면 변경사항이 있는지 서버에게 물어본다.

**5.응답 생성**

- 캐시는 새로운 헤더와 캐시된 본문으로 응답 메시지를 만든다.

**6.발송**

- 캐시는 네트워크를 통해 응답을 클라이언트에게 돌려준다.

**7.로깅**

- 선택적으로, 캐시는 로그파일에 트랜잭션에 대해 서술한 로그 하나를 남긴다.

<br>

## 7.8 사본을 신선하게 유지하기

**문서 만료**

- `Cache-Control`과 `Expires` 라는 헤더를 통해 콘텐츠가 얼마나 오랫동안 신선한 상태로 보일 수 있는지 알 수 있다.

**유효기간과 나이**

- 서버는 응답 본문과 함께 하는, `HTTP/1.0+ Expires`나 `HTTP/1.1 Cache-Control: max-age` 응답 헤더를 이용해서 유효기간을 명시한다.

**서버 재검사**

- 캐시된 문서가 만료되었다는 것은 이제 검사할 시간이 되었음을 뜻한다.
- 서버 재검사는 캐시가 원 서버에게 문서가 변경되었는지의 여부를 물어볼 필요가 있음을 의미한다.

**조건부 메서드와의 재검사**

- HTTP의 조건부 메서드는 재검사를 효율적으로 만들어준다.
- `If-Modified-Since: <date>`: 날짜 재검사
- `If-None-Match: <tags>`: 엔터티 태그 재검사

**약한 검사기와 강한 검사기**

- 약한 검사기: 서버가 모든 캐시된 사본을 무효화시키지 않고 문서를 살짝 고칠 수 있도록 허용하고 싶을 때 사용
- 강한 검사기: 콘텐츠가 바뀔 때마다 바뀐다.

<br>

## 7.9 캐시 제어

- HTTP는 문서가 만료되기 전까지 얼마나 오랫동안 캐시될 수 있게 할 것인지 서버가 설정할 수 있는 여러 가지 방법을 정의한다.

**no-cache와 no-store 응답 헤더**

- 캐시가 검증되지 않은 캐시된 객체로 응답하는 것을 막는다.

**Max-Age 응답 헤더**

- 캐시가 매 접근마다 문서를 캐시하거나 리프래시하지 않도록 요청할 수 있다.

**Expires 응답 헤더**

- 더 이상 사용하지 않기를 권하는 `Expires` 헤더는 초 단위의 시간 대신 실제 만료 날짜를 명시한다.

**Must-Revalidate 응답 헤더**

- 캐시는 성능을 개선하기 위해 신선하지 않은 객체를 제공하도록 설정될 수 있다.

**휴리스틱 만료**

- 유명한 휴리스틱 만료 알고리즘의 하나인 LM 인자 알고리즘은 문서가 최근 변경 일시를 포함하고 있다면 사용할 수 있다.

**클라이언트 신선도 계약**

- 웹 브라우저는 브라우저나 프락시 캐시의 신선하지 않은 콘텐츠를 강제로 갱신시켜 주는 리프레시나 리로드 버튼을 갖고 있다.
