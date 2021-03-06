# 09. 웹 로봇

## 9.0 Intro

- **웹 로봇**은 사람과의 상호작용 없이 연속된 웹 트랜잭션들을 자동으로 수행하는 소프트웨어 프로그램이다.
- 종류로는 크롤러, 스파이더, 윔, 봇 등이 있다.

<br>

## 9.1 크롤러와 크롤링

**웹 크롤러가 하는 일**

- 먼저 웹페이지를 한 개 가져오고,
- 그 다음 그 페이지가 가리키는 모든 웹페이지를 가져오고,
- 다시 그 페이지들이 가리키는 모든 웹페이지들을 가져오는 일을 반복해서 웹을 순회하는 로봇이다.

웹 링크를 재귀적으로 따라가는 로봇을 **크롤러 혹은 스파이더**라고 부른다.<br>
이유는 HTML 하이퍼링크들로 만들어진 웹을 따라 **'기어다니기'** 때문이다.

<br>

### 9.1.1 어디에서 시작하는가: '루트 집합'

- 크롤러가 방문을 시작하는 URL들의 초기 집합은 **루트 집합**이라고 불린다.
- 일반적으로 좋은 루트 집합은 크고 인기 있는 웹 사이트, 새로 생성된 페이지들의 목록, 그리고 자주 링크되지 않는 잘 알려지지 않은 페이지들의 목록으로 구성되어 있다.

<br>

### 9.1.2 링크 추출과 상대 링크 정상화

- 크롤러는 검색한 각 페이지 안에 들어있는 URL 링크들을 파싱해서 크롤링할 페이지들의 목록에 추가 해야 한다.
- 크롤러들은 간단한 HTML 파싱을 해서 이들 링크들을 추출하고 상대 링크를 절대 링크로 변환할 필요가 있다.

<br>

### 9.1.3 순환 피하기

- 로봇들은 순환을 피하기 위해 반드시 그들이 어디를 방문했는지 알아야 한다.
- 순환은 로봇을 함정에 빠뜨려서 멈추게 하거나 진행을 느려지게 한다.

<br>

### 9.1.4 루프와 중복

순환은 최소한 다음의 세 이유로 인해 크롤러에게 헤롭다.

- 순환은 크롤러를 루프에 빠뜨려서 꼼짝 못하게 만들 수 있다.
- 크롤러가 같은 페이지를 반복해서 가져오면 고스란히 웹 서버의 부담이 된다.
- 비록 루프 자체가 문제가 되지 않더라도, 크롤러는 많은 수의 중복된 페이지들을 가져오게 된다.

<br>

### 9.1.5 빵 부스러기의 흔적

- 불행히도, 방문한 곳을 지속적으로 추적하는 것은 쉽지 않다.
- 대규모 웹 크롤러가 그들이 방문한 곳을 관리하기 위해 사용하는 유용한 기법은 다음과 같다.
  - 트리와 해시 테이블
  - 느슨한 존재 비트맵
  - 체크포인트
  - 파티셔닝

<br>

### 9.1.6 별칭과 로봇 순환

- 한 URL이 또 다른 URL에 대한 별칭이라면, 그 둘이 서로 달라 보이더라도 사실은 같은 리소스를 가리키고 있다.
- ex. `http://foo.com/bar.html`, `http://foo.com:80/bar.html`

<br>

### 9.1.7 URL 정규화하기

대부분의 웹 로봇은 URL들을 표준 형식으로 '정규화' 함으로써 다른 URL과 같은 리소스를 가리키고 있음이 확실한 것들을 미리 제거하려 시도한다.
로봇은 다음과 같은 방식으로 모든 URL을 정규화된 형식으로 변환할 수 있다.

1. 포트 번호가 명시되지 않았다면, 호스트 명에 ':80'을 추가한다.
2. 모든 %xx 이스케이핑된 문자들을 대응되는 문자로 변환한다.
3. `#` 태그들을 제거한다.

<br>

### 9.1.8 파일 시스템 링크 순환

- 파일 시스템의 심벌릭 링크는 사실상 아무것도 존재하지 않으면서도 끝없이 깊어지는 디렉터리 계층을 만들 수 있기 때문에, 매우 교묘한 종류의 순환을 유발할 수 있다.

<br>

### 9.1.9 동적 가상 웹 공간

- 웹 마스터들은 로봇들을 함정으로 빠뜨리기 위해 복잡한 크롤러 루프를 악의적으로 만들 수 있고, 악의가 없지만 자신도 모르게 심벌릭 링크나 동적 콘텐츠를 통한 크롤러 함정을 만들 수도 있다.

<br>

### 9.1.10 루프와 중복 피하기

모든 순환을 피하는 완벽한 방법은 없다.<br>
실제로 잘 설계된 로봇은 순환을 피하기 위해 휴리스틱의 집합을 필요로 한다.<br>
이러한 웹에서 로봇이 더 올바르게 동작하기 위해 사용하는 기법들은 다음과 같다.

- URL 정규화
- 너비 우선 크롤링
- 스로틀링
- URL 크기 제한
- URL/사이트 블랙리스트
- 패턴 발견
- 콘텐츠 지문
- 사람의 모니터링

<br>

## 9.2 로봇의 HTTP

- 많은 로봇이 그들이 찾는 콘텐츠를 요청하기 위해 필요한 HTTP를 최소한으로만 구현하려고 한다.
- 결과적으로 많은 로봇이 요구사항이 적은 HTTP/1.0 요청을 보낸다.

<br>

### 9.2.1 요청 헤더 식별하기

로봇 대부분은 약간의 신원 식별 헤더를 구현하고 전송한다.<br>
로봇 구현자들은 로봇의 능력, 신원, 출신을 알려주는 다음과 같은 기본적인 몇 가지 헤더를 사이트에서 보내주는 것이 좋다.

- User-Agent
- From
- Accept
- Referer

<br>

### 9.2.2 가상 호스팅

- 로봇 구현자들은 Host 헤더를 지원할 필요가 있다.
- 요청에 Host 헤더를 포함하지 않으면 로봇이 어떤 URL에 대해 잘못된 콘텐츠를 찾게 만든다.

<br>

### 9.2.3 조건부 요청

- 때때로 로봇들이 극악한 양의 요청을 시도한다는 것을 고려할 때, 로봇이 검색하는 콘텐츠의 양을 최소화하는 것은 상당히 의미 있는 일이다.

<br>

### 9.2.4 응답 다루기

- 대다수 로봇들은 주 관심사가 단순히 GET 메서드로 콘텐츠를 요청해서 가져오는 것이기 때문에, 응답 다루기라고 부를 만한 일은 거의 하지 않는다.

<br>

### 9.2.5 User-Agent 타기팅

- 웹 관리자들은 많은 로봇이 그들의 사이트를 방문하게 될 것임을 명심하고, 그 로봇들로부터의 요청을 예상해야 한다.
- 사이트 관리자들은 최소한 로봇이 그들의 사이트에 방문했다가 콘텐츠를 얻을 수 없어 당황하는 일이 없도록 대비해야 한다.

<br>

## 9.3 부적절하게 동작하는 로봇들

로봇들이 저지르는 실수 몇 가지와 그로 인해 초래되는 결과를 몇 가지 들어보면 다음과 같다.

**폭주하는 로봇**

- 만약 로봇이 논리적인 에러를 갖고 있거나 순환에 빠졌다면 웹 서버에 극심한 부하를 안겨줄 수 있다.

<br>

**오래된 URL**

- 만약 웹 사이트가 그들의 콘텐츠를 많이 바꾸었다면, 로봇들은 존재하지 않는 URL에 대한 요청을 많이 보낼 수 있다.

<br>

**길고 잘못된 URL**

- 순환이나 프로그래밍상의 오류로 인해 로봇은 웹 사이트에게 크고 의미 없는 URL을 요청할 수 있다.

<br>

**호기심이 지나친 로봇**

- 웹에서 많은 양의 데이터를 검색하는 로봇의 구현자들은 사이트 구현자들이 인터넷을 통해 접근 가능하리라고 결코 의도하지 않았을 몇몇 특정 지점의 민감한 데이터를 그들의 로봇이 검색할 수 있다는 것에 주의해야 한다.

<br>

**동적 게이트웨이 접근**

로봇은 게이트웨이 애플리케이션의 콘텐츠에 대한 URL로 요청을 할 수도 있다.

<br>

## 9.4 로봇 차단하기

- 1994년, 로봇이 그들에게 맞지 않는 장소에 들어오지 않도록 하고 웹 마스터에게 로봇의 동작을 더 잘 제어할 수 있는 메커니즘을 제공하는 단순하고 자발적인 기법이 고안되었다.
- 로봇의 접근을 제어하는 정보를 저장하는 파일의 이름을 따서 종종 그냥 `robots.txt`라고 이름 붙은 선택적인 파일을 제공할 수 있다.
- 로봇이 페이지를 요청하기 전에 먼저 이 페이지를 가져올 수 있는 권한이 있는지 확인하기 위해 `robots.txt` 파일을 검사한다.

<br>

### 9.4.1 로봇 차단 표준

- 오늘날 대부분의 로봇들은 v0.0이나 v1.0 표준을 채택했다.

<br>

### 9.4.2 웹 사이트와 robots.txt 파일들

- 웹 사이트의 어떤 URL을 방문하기 전에, 그 웹사이트에 `robots.txt` 파일이 존재한다면 로봇은 반드시 그 파일을 가져와서 처리해야 한다.
- 그 사이트 전체에 대한 `robots.txt` 파일은 단 하나만이 존재한다.

<br>

**robots.txt 가져오기**

- 로봇은 웹 서버의 여느 파일들과 마찬가지로 HTTP GET 메서드를 이용해 `robots.txt` 리소스를 가져온다.

<br>

### 9.4.3 robots.txt 파일 포맷

```
# this robots.txt file allows Slurp & Webcrawler to crawl
# the public parts of our site, but no other robots...

User-Agent: slurp
User-Agent: webcrawler
Disallow: /private

User-Agent: *
Disallow:
```

<br>

### 9.4.5 robots.txt의 캐싱과 만료

- 로봇은 주기적으로 `robots.txt`를 가져와서 그 결과를 캐시해야 한다.
- `robots.txt`의 캐시된 사본은 `robots.txt` 파일이 만료될 때까지 로봇에 의해 사용된다.

<br>

## 9.6 검색엔진

- 웹 로봇을 가장 광범위하게 사용하는 것은 인터넷 검색엔진이다.
- 웹 크롤러들은 마치 먹이를 주듯 검색엔진에게 웹이 존재하는 문서들을 가져다 주어서, 검색엔진이 어떤 문서에 어떤 단어들이 존재하는지에 대한 색인을 생성할 수 있게 한다.

<br>

### 9.6.1 넓게 생각하라

- 웹 전체를 크롤링하는 것은 여전히 쉽지 않은 도전이다.

<br>

### 9.6.2 현대적인 검색엔진의 아키텍처

- 검색엔진 크롤러들은 웹페이지들을 수집하여 집으로 가져와서, 이 풀 텍스트 색인에 추가한다.
- 동시에, 검색엔진 사용자들은 핫봇이나 구글과 같은 웹 검색 게이트웨이를 통해 통 텍스트 색인에 대한 질의를 보낸다.

<br>

### 9.6.3 풀 텍스트 색인

- 풀 텍스트 색인은 단어 하나를 입력받아 그 단어를 포함하고 있는 문서를 즉각 알려줄 수 있는 데이터베이스다.

<br>

### 9.6.4 질의 보내기

- 사용자가 질의를 웹 검색엔진 게이트웨이로 보내는 방법은, HTML 폼을 사용자가 채워 넣고 브라우저가 그 폼을 HTTP GET이나 POST 요청을 이용해서 게이트웨이로 보내는 식이다.
- 게이트웨이 프로그램은 검색 질의를 추출하고 웹 UI 질의를 풀 텍스트 색인을 검색할 때 사용되는 표현식으로 변환한다.

<br>

### 9.6.5 검색 결과를 정렬하고 보여주기

- 많은 웹페이지가 주어진 단어를 포함할 수 있기 때문에, 검색엔진은 결과에 순위를 매기기 위해 똑똑한 알고리즘을 사용한다.
- 검색엔진은 그 문서들이 주어진 단어와 가장 관련이 많은 순서대로 결과 문서에 나타날 수 있도록 문서들 간의 순서를 알 필요가 있다. (관련도 랭킹)

<br>

### 9.6.6 스푸핑

- 웹 사이트를 찾을 때 검색 결과의 순서는 중요하다.
- 많은 웹 마스터가 수많은 키워드들을 나열한 가짜 페이지를 만들거나, 더 나아가서는 검색 엔진의 관련도 알고리즘을 더 잘 속일 수 있는 게이트웨이 애플리케이션을 만들어 사용한다.
- 검색엔진과 로봇 구현자들은 이러한 속임수를 더 잘 잡아내기 위해 끊임없이 그들의 관련도 알고리즘을 수정해야만 한다.
