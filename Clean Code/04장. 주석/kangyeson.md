# 04. 주석

> 나쁜 코드에 주석을 달지 마라. 새로 짜라
- **주석이란❓** 코드로 의도를 표현하는 것에 실패했을 때 사용하는 것
- 주석은 **유지보수**가 힘듬 ➔ 변화하고 진화하는 코드와 달리, 주석은 오래 될 수록 근거 없고 거짓되어 진다. 
- 부정확한 주석은 아예 없는 주석보다 훨씬 더 나쁘다. 

> **목표**✔ <br>
> 코드를 깔끔하게 정리하고 표현력을 강화하여, 주석을 가능한 줄이도록 하는 것
<br>

### ● 주석은 나쁜 코드를 보완하지 못한다
> 자신이 저지른 난장판을 주석으로 설명하려 애쓰는 대신에 그 난장판을 깨끗이 치우는 데 시간을 보내라

### ● 코드로 의도를 표현하라
```javascript
// 직원에게 복지 혜택을 받을 자격이 있는지 검사한다.
if ((employee.flags & HOURLY_FLAG) &&
    (employee.age > 65)
```
코드만으로 의도를 설명하기 어려운 경우<br>
➔ **주석으로 달려는 설명을 함수로 만들어 표현**
```javascript
if (employee.isEligibleForFullBenefits())
```

<br><br>

## ☝ 좋은 주석
> 정말로 좋은 주석은, 주석을 달지 않을 방법을 찾아낸 주석


#### 1. 법적인 주석
회사의 구현 표준에 맞춰 법적인 이유로 넣는 특정 주석
- 저작권 정보, 소유권 정보
- 표준 라이선스, 외부 문서 참조
```javascript
// Copyright (c) 2003, 2004, 2005 by Object Mentor, Inc. All rights reserved.
// GNU General Public License 버전 2 이상을 따르는 조건으로 배포한다.
```

<br>

#### 2. 정보를 제공하는 주석
기본적인 정보를 주석으로 제공하면 편리
```javascript
// 테스트 중인 Responder 인스턴스를 반환한다.
Protected abstract Responder responderInstance();
```
➔ 함수 이름에 정보를 담아, responderBeingTested로 바꾸면 주석 생략 가능

<br>

#### 3. 의도를 설명하는 주석
결정에 깔린 의도 설명
```javascript
// 스레드를 대량 생성하는 방법으로 어떻게든 경쟁 조건을 만들려 시도한다.
for (int i=0; i<25000; i++) {
  WidgetBuilderThread widgetBuilderThread = 
    new WidgetBuilderThread(widgetBuilder, text, parent, failFlag);
    ...
  }
  assertEquals(false, failFlag.get());
}
```

<br>

#### 4. 의도를 명료하게 밝히는 주석
- 표준 라이브러리＆변경 불가한 코드에 속하는 인수, 반환값일 경우
- 주석이 올바른지 검증하기 쉽지 않다. 

```javascript
assertTrue(a.compareTo(a) == 0); // a==a
assertTrue(a.compareTo(b) != 0); // a!=b
```

<br>

#### 5. 결과를 경고하는 주석
- 다른 프로그래머에게 결과를 경고할 목적
- @Ignore 속성 이용, 테스트 케이스 OFF
```javascript
public static SimpleDateFormat makeStandardHttpDateFormat()
{
  // SimpleDateFormat은 스레드에 안전하지 못하다.
  // 따라서 각 인스턴스를 독립적으로 생성해야 한다.
  SimpleDateFormat df = newSimpleDateFormat("EEE, dd MMM yyyy HH:mm:ss z");
  ...
}
```

<br>

#### 6. TODO 주석
- 앞으로 할 일 기술
- 주기적으로 TODO주석 점검 후 제거하기

```javascript
// TODO-MdM 현재 필요하지 않다.
// 체크아웃 모델을 도입하면 함수가 필요 없다.
protected VersionInfo makeVersion() throws Exception
{
  return null;
}
```

<br>

#### 7. 중요성을 강조하는 주석
- 공개 API는 (javadocs를 사용하여) 상세히 설명할 것. 


<br><br>

## ✌ 나쁜 주석
> 대다수 주석은 허술한 코드를 지탱하거나, 엉성한 코드를 변명하거나, 미숙한 결정을 합리화하는 등 프로그래머가 주절거리는 독백에서 크게 벗어나지 못한다.

#### 1. 주절거리는 주석
- 특별한 이유 없이 의무감으로 혹은 프로세스에서 하라고 하니까 마지못해 단 주석
- 독자아 제대로 소통하지 못하는 주석

```javascript
catch(IOException e)
{
  // 속성 파일이 없다면 기본값을 모두 메모리로 읽어 들였다는 의미다.
}
```

<br>

#### 2. 같은 이야기를 중복하는 주석
코드보다 더 많은 정보를 제공하지 못하는 주석

```javascript
/**
 * 이 컴포넌트의 프로세서 지연값
 */
protected int backgroundProcessorDelay = -1;
```

<br>
 
#### 3. 오해할 여지가 있는 주석
딱 맞을 정도로 엄밀하게 달지 못하여 오해가 생기는 주석 <br><br>

#### 4. 의무적으로 다는 주석
의무적인 주석은 코드를 복잡하게 하고 잘못된 정보를 제공할 여지만 만든다. <br><br>

#### 5. 이력을 기록하는 주석
모듈 첫머리에 모든 변경 이력을 기록❌ <br>
소스 코드 관리 시스템을 이용하자 ✔

<br>

#### 6. 있으나 마나 한 주석
- 너무 당연한 사실을 언급, 새로운 정보를 제공하지 못하는 주석
- 때로는 javadocs도 잡음
- 함수나 변수로 표현할 수 있다면 주석을 달지 마라
- 공로를 돌리거나 저자를 표시하는 주석

```javascript
/* 릭이 추가함 */
```

<br>

#### 7. 위치를 표시하는 주석
배너를 남용하면 가독성만 떨어진다.
```javascript
// Actions ////////////////////////////
```

<br>

#### 8. 닫는 괄호에 다는 주석
- 작고 캡슐화된 함수에선 잡음일뿐
- 닫는 괄호 주석 대신, 함수를 줄이려 시도하자

```javascript
try{
  while ((line = in.readLine()) != null) {
    ...
  } //while
  ...
} //try
```

<br>

#### 9. 주석으로 처리한 코드
- 주석으로 처리한 코드는 중요하다고 인식되어 지우기가 주저되며, 점차 쌓여만 간다.
- 코드를 삭제하라. 소스 코드 관리 시스템을 사용하자.

```javascript
response.setBody(formatter.getResultStream(), formatter.getByteCount());
// InputStream resultsStream = formatter.getResultStream();
```

<br>

#### 10. HTML 주석
```javascript
/**
 * <p/>
 * <pre>
 * 용법:
 */
```

<br>

#### 11. 전역 정보
- 코드 일부에 주석을 달면서 시스템의 전반적인 정보를 기술하지 마라
- 근처에 있는 코드만 기술하라

###### 아래 주석은 아래 함수가 아니라, 다른 어딘가의 함수를 설명하고 있다. 
```javascript
/**
 * 적합성 테스트가 동작하는 포트: 기본값은 <b>8082</b>.
 *
 * @param fitnessePort
 */
public void setFitnessePort(int fitnessePort)
{
  this.fitnessePort = fitnessePort;
}
```

<br>

#### 12. 너무 많은 정보
독자에게 불필요하며 아무 관련 없는 정보를 장황하게 늘어놓지 마라 <br><br>

#### 13. 모호한 관계
주석과 주석이 설명하는 코드는 둘 사이 관계가 명백해야 한다. <br>
주석 자체가 다시 설명을 요구해선 안된다. <br><br>

#### 14. 함수 헤더
짧고 한 가지만 수행하며 이름을 잘 붙인 함수 ✔<br>
주석으로 헤더를 추가한 함수 ❌ <br><br>

#### 15. 비공개 코드에서 Javadocs
공개하지 않을 코드라면 Javadocs는 쓸모가 없다.

 



