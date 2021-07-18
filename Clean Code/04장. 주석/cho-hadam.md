# 📒 04장. 주석

## 🔨 주석은 나쁜 코드를 보완하지 못한다

표현력이 충부하고 깔끔하며 주석이 거의 없는 코드가 복잡하고 어수선하며 주석이 많이 달린 코드보다 훨씬 좋다

<br />

## 💻 코드로 의도를 표현하라

```java
// 직원에게 복지 혜택을 받을 자격이 있는지 검사한다.
if ((employee.flags & HOURLY_FLAG) &&
    (employee.age > 65))
```

```java
if (employee.isEligibleForFullBenefits())
```

<br />

## 👍🏻 좋은 주석

### 법적인 주석

저작권 정보와 소유권 정보 등

```java
// Copyright (C) 2003,2004,2005 by Object Mentor, Inc. All rights reserved.
// GNU General Public License 버전 2 이상을 따르는 조건으로 배포한다.
```

### 정보를 제공하는 주석

때로는 기본적인 정보를 주석으로 제공하면 편리하다

### 의도를 설명하는 주석

```java
// 스레드를 대량 생산하는 방법으로 어떻게든 경쟁 조건을 만들려 시도한다.
for (int i = 0; i < 25000; i++) {
  WidgetBuilderThread widgetBuilderThread = 
    new WidgetBuilderThread(widgetBuilder, text, parent, failFlag);
  ...
}
```

### 의미를 명료하게 밝히는 주석

모호한 인수나 반환값이 표준 라이브러리나 변경하지 못하는 코드에 속한다면 의미를 명료하게 밝히는 주석이 유용하다

```java
assertTrue(a.compareTo(a) == 0);  // a == a
```

### 결과를 경고하는 주석

다른 프로그래머에게 결과를 경고할 목적으로 주석을 사용한다

```java
// SimpleDateFormat은 스레드에 안전하지 못하다.
// 따라서 각 인스턴스를 독립적으로 생성해야 한다.
SimpleDateFormat df = new SimpleDateFormat("EEE, dd MMM  yyyy HH:mm:ss z");
df.setTimeZone(TimeZone.getTimeZone("GMT"));
return df;
```

### TODO 주석

앞으로 할 일을 주석으로 남겨두면 편하다

```java
// TODO-MdM 현재 필요하지 않다.
// 체크아웃 모델을 도입하면 함수가 필요 없다.
protected VersionInfo makeVersion() throws Exception {
  return null;
}
```

하지만 주기적으로 TODO 주석을 점검해 없애도 괜찮은 주석은 없애라고 권한다

### 중요성을 강조하는 주석

```java
String listItemContent = match.group(3).trim();
// 여기서 trim은 정말 중요하다. trim 함수는 문자열에서 시작 공백을 제거한다.
// 문자열에 시작 공백이 있으면 다른 문자열로 인식되기 때문이다.
```

### 공개 API에서 Javadocs

공개 API를 구현한다면 반드시 훌륭한 Javadocs를 작성한다   
하지만 여느 주석과 마찬가지로 Javadocs 역시 독자를 오도하거나, 잘못 위치하거나, 그릇된 정보를 전달할 가능성이 존재한다.

<br />

## 👎🏻 나쁜 주석

### 주절거리는 주석

```java
catch(IOException e) {
  // 속성 파일이 없다면 기본값을 모두 메모리로 읽어 들였다는 의미다.
}
```

### 같은 이야기를 중복하는 주석

코드 내용을 그대로 중복하면 자칫 코드보다 주석을 읽는 시간이 더 오래 걸린다

```java
/**
 * 이 컴포넌트의 프로세서 지연값
 */
protected int backgroundProcessorDelay = -1;
```

### 오해할 여지가 있는 주석

주석에 담긴 '살짝 잘못된 정보'로 인해 어느 프로그래머가 경솔하게 함수를 호출할지도 모른다

### 의무적으로 다는 주석

모든 함수에 Javadocs을 달거나 모든 변수에 주석을 달아야 한다는 규칙은 어리석다

### 이력을 기록하는 주석

소스 코드 관리 시스템이 있는 지금은 혼란만 가중할 뿐이다

```java
/*
 * 11-Oct-2001 : 클래스를 다시 정리하고 새로운 패키지인
 *               com.jrefinery.date로 옮겼다 (DG);
 * 05-Nov-2001 : getDescription() 메서드를 추가했으며
 *               NotableDate class를 제거했다 (DG); 
 */
```

### 있으나 마나 한 주석

너무 당연한 사실을 언급하며 새로운 정보를 제공하지 못하는 주석은 나쁜 주석이다

```java
/** 
 * 기본 생성자 
 */
protected AnnualDateRule() {
}
```

### 무서운 잡음

```java
/** The version. */
private String version;

/** The version. */
private String info;
```

### 함수나 변수로 표현할 수 있다면 주석을 달지 마라

```java
// 전역 목록 <smodule>에 속하는 모듈이 우리가 속한 하위 시스템에 의존하는가?
if (smodule.getDependSubsystems().contains(subSysMod.getSubSystem()))
```

🔽

```java
ArrayList moduleDependees = smodule.getDependSubsystems();
String ourSubSystem = subSysMod.getSubSystem();
if (moduleDependees.contains(ourSubSystem))
```

### 위치를 표시하는 주석

```java
// Actions /////////////////////////
```

### 닫는 괄호에 다는 주석

대신에 함수를 줄이려 시도하자

```java
catch(IOException e) {
  System.err.println("Error: " + e.getMessage());
} // catch
```

### 공로를 돌리거나 저자를 표시하는 주석

소스 코드 관리 시스템에 저장하는 편이 좋다

```java
/* 릭이 추가함 */
```

### 주석으로 처리한 코드

```java
this.bytePos = writeBytes(pngIdBytes, 0);
// hdrPos = bytePos;
```

### HTML 주석

```java
/**
 * 이 과업은 적합성 테스트를 수행해 결과를 출력한다.
 * <p/>
 * <pre>
 */
```

### 전역 정보

주석을 달아야 한다면 근처에 있는 코드만 기술하라

```java
/**
 * 적합성 테스트가 동작하는 포트: 기본값은 <b>8082</b>
 * 
 * @param fitnessePort
 */
```

### 너무 많은 정보

```java
/*
 1부: 인터넷 메시지 본체 형식
 6.8절.  Base64 내용 전송 인코딩
 인코딩 과정은 입력 비트 중 ...
*/
```

### 모호한 관계

주석과 주석이 설명하는 코드는 둘 사이 관계가 명백해야 한다

```java
/*
 * 모든 픽셀을 담을 만큼 충분한 배열로 시작한다(여기에 필터 바이트를 더한다).
 * 그리고 헤더 정보를 위해 200바이트를 더한다.
 */
this.pngBytes = new byte[((this.width + 1) * this.height * 3) + 200];
```

_🔼 여기서 필터 바이트란? +1과 관련이 있을까? 아니면 *3과 관련이 있을까?_

### 함수 헤더

짧은 함수는 긴 설명이 필요 없다

### 비공개 코드에서 Javadocs

공개하지 않을 코드라면 Javadocs는 쓸모가 없다

<br />

## ❓ 몰랐던 것

**Javadoc**   
```/**```로 시작하는 주석에 설명과 @ 태그를 이용하는 등을 통해 API 문서를 생성할 수 있다

<br />

## 🧐 기억에 남은 부분

> _실패를 만회하기 위해 주석을 사용한다._

> _주석이 언제나 코드를 따라가지는 않는다._
