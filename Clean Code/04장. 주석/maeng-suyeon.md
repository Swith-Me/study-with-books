> 나쁜 코드에 주석을 달지 마라. 새로 짜라.     - 브라이언 W. 커니핸, P.J. 플라우거

- 주석은 필요악이다.
- 코드로 의도를 표현하지 못해, 실패를 만회하기 위해 주석을 사용한다.
- 주석은 언제나 실패를 의미한다.
- 주석 없이는 자신을 표현할 방법을 찾지 못해 할 수 없이 주석을 사용한다.
- 주석은 반겨 맞을 손님이 아니다.
- 주석은 오래될수록 코드에서 멀어져서 거짓말을 하게 될 가능성이 커진다.
- 코드는 유지보수를 해도 주석을 유지보수하기란 현실적으로 불가능하다.

## 주석은 나쁜 코드를 보완하지 못한다.

- 코드에 주석을 추가하는 일반적인 이유는 코드 품질이 나쁘기 때문이다.
- 표현력이 풍부하고 깔끔하며 주석이 거의 없는 코드가, 복잡하고 어수선하며 주석이 많이 달린 코드보다 훨씬 좋다.

## 코드로 의도를 표현하라!

확실히 코드만으로 의도를 설명하기 어려운 경우가 존재한다.

```java
// 직원에게 복지 혜택을 받을 자격이 있는지 검사한다. 
if ((emplotee.flags & HOURLY_FLAG) && (employee.age > 65))
```

다음 코드는 어떤가?

```java
if (employee.isEligibleForFullBenefits())
```

주석 필요없이 함수만으로 충분한 표현이 가능해졌다.

## 좋은 주석

어떤 주석은 필요하거나 유익하다.

하지만, 정말 좋은 주석은 주석을 달지 않을 방법을 찾아낸 주석이라는 사실을 명심하자.

### 법적인 주석

: 회사가 정립한 구현 표준에 맞춰 **법적인 이유**로 특정 주석을 넣으라고 명시한 주석

ex- 각 소스 파일 첫머리에 주석으로 들어가는 **저작권 정보**, **소유권 정보**

```java
// Copyright (C) 2003, 2004, 2005 by Object Montor, Inc. All right reserved.
// GNU General Public License
```

### 정보를 제공하는 주석

: **기본적인 정보**를 주석으로 제공하는 것.

ex- 추상 메서드가 반환할 값을 설명한 주석

```java
// 테스트 중인 Responder 인스턴스를 반환한다.
protected abstract Responder responderInstance();
```

가능하면, 함수 이름에 정보를 담는 편이 더 좋다.

그래서 위 코드는 함수 이름을 `responderBeingTested`로 바꾸면 주석이 필요 없어진다. 

더 나은 예 :

```java
// kk:mm:ss EEE, MMM dd, yyyy 형식이다.
Pattern timeMatcher = Pattern.compile("\\d*:\\d*\\d* \\w*, \\w*, \\d*, \\d*");
```

### 의도를 설명하는 주석

: **구현을 이해하게 도와주는 것**에서 **결정에 깔린 의도까지 설명**하는 주석

```java
// 스레드를 대량 생성하는 방법으로 어떻게든 경쟁 조건을 만들려 시도한다. 
for (int i = 0; i > 2500; i++) {
    WidgetBuilderThread widgetBuilderThread = 
        new WidgetBuilderThread(widgetBuilder, text, parent, failFlag);
    Thread thread = new Thread(widgetBuilderThread);
    thread.start();
}
```

### 의미를 명료하게 밝히는 주석

: **모호한 인수**나 **반환값**의 의미를 **읽기 좋게 표현**하여 이해를 돕는 주석

→ 특히, 인수나 반환값이 표준 라이브러리나 변경하지 못하는 코드에 속한다면 이 주석을 사용하는 것이 좋다.

```java
public void testCompareTo() throws Exception {
	WikiPagePath a = PathParser.parse("PageA");
	WikiPagePath b = PathParser.parse("PageB");

	assertTrue(a.compareTo(a) == 0);     // a == a
	assertTrue(a.compareTo(b) != 0);     // a != b
}
```

❗ 하지만, 주석이 올바른지 검증하기 쉽지 않아 위험한 주석이 될 수도 있다.

### 결과를 경고하는 주석

: 다른 프로그래머에게 결과를 **경고할 목적**으로 사용하는 주석

```java
// 여유 시간이 충분하지 않다면 실행하지 마십시오.
public void _testWithReallyBigFile() {
	writeLinesToFile(10000000);

	response.setBody(testFile);
	response.readyToSend(this);
	String responseString = output.toString();
	assertSubString("Content-Length: 1000000000", responseString);
	assertTrue(bytesSent > 1000000000);
}
```

### TODO 주석

: **앞으로 할 일**을 `//TODO` 주석으로 남기는 것

```java
// TODO-MdM 현재 필요하지 않다.
// 체크아웃 모델을 도입하면 함수가 필요 없다.
protected VersionInfo makeVersion() throws Exception {
    return null;
}
```

- 당장 구현하기 어려운 업무를 기술한다.
- 더 이상 필요 없는 기능을 삭제하라는 알림, 누군가에게 문제를 봐달라는 요청, 더 좋은 이름을 떠올려달라는 부탁, 앞으로 발생할 이벤트에 맞춰 코드를 고치라는 주의 등에 유용하다.
- 요즘은 IDE를 통해 남은 TODO를 쉽게 볼 수 있으므로 편리하게 이용할 수 있다.

### 중요성을 강조하는 주석

: 대수롭지 않다고 여겨질 **뭔가의 중요성을 강조**하기 위해 사용하는 것

```java
String listItemContent = match.group(3).trim();
// 여기서 trim은 정말 중요하다. trim 함수는 문자열에서 시작 공백을 제거한다.
// 문자열에 시작 공백이 있으면 다른 문자열로 인식되기 때문이다. 
new ListItemWidget(this, listItemContent, this.level + 1);
return buildList(text.substring(match.end()));
```

### 공개 API에서 Javadocs

설명이 잘 된 공개 API는 참으로 유용하고 만족스럽다. 

공개 API를 구현한다면 반드시 훌륭한 Javadocs 작성을 추천한다. 

하지만 여느 주석과 마찬가지로 Javadocs 역시 독자를 오도하거나, 잘못 위치하거나, 그릇된 정보를 전달할 가능성이 존재하는 것 역시 잊으면 안 된다.

## 나쁜 주석

대다수의 주석이 이 범주에 속한다. 

일반적으로 대다수 주석은 허술한 코드를 지탱하거나, 엉성한 코드를 변명하거나, 미숙한 결정을 합리화하는 등 프로그래머가 주절거리는 독백에서 크게 벗어나지 못한다.

### 주절거리는 주석

: **특별한 이유 없이 의무감**으로 혹은 프로세스에서 시켜서 **마지못해 다는** 주석

```java
public void loadProperties() {
    try {
        String propertiesPath = propertiesLocation + "/" + PROPERTIES_FILE;
        FileInputStream propertiesStream = new FileInputStream(propertiesPath);
        loadedProperties.load(propertiesStream);
    } catch (IOException e) {
        // 속성 파일이 없다면 기본값을 모두 메모리로 읽어 들였다는 의미다. 
    }
}
```

- 다른 모듈까지 뒤져야 하는 주석은 제대로 된 주석이 아니다.

### 같은 이야기를 중복하는 주석

: 헤더에 달린 주석이 같은 **코드 내용이 중복된** 주석

```java
// this.closed가 true일 때 반환되는 유틸리티 메서드다.
// 타임아웃에 도달하면 예외를 던진다. 
public synchronized void waitForClose(final long timeoutMillis) throws Exception {
    if (!closed) {
        wait(timeoutMillis);
        if (!closed) {
            throw new Exception("MockResponseSender could not be closed");
        }
    }
}
```

### 오해할 여지가 있는 주석

: 의도는 좋았으나 프로그래머가 딱 맞을 정도로 **엄밀하게는 주석을 달지 못해서 발생**하는 주석

위 코드를 다시 보면, 중복이 많으면서도 오해할 여지가 살짝 있다. 

`this.closed`가 `true`로 변하는 순간에 메서드는 반환되지 않는다. 

`this.closed`가 `true`여야 메서드는 반환된다. 아니면 무조건 타임아웃을 기다렸다 `this.closed`가 그래도 `true`가 아니면 예외를 던진다. 

🤔 주석에 담긴 '살짝 잘못된 정보'로 인해 어느 프로그래머가 경솔하게 함수를 호출해 자기 코드가 아주 느려진 이유를 못찾게 되는 것이다.

### 의무적으로 다는 주석

: 모든 함수에 **Javadocs**를 달거나 모든 변수에 **주석을 달아야하는 규칙**을 지킴으로 인한 주석

⇒ 코드를 복잡하게 만들며, 거짓말을 퍼뜨리고, 혼동과 무질서를 초래한다. 아래와 같은 주석은 아무 가치도 없다.

```java
/**
 *
 * @param title CD 제목
 * @param author CD 저자
 * @param tracks CD 트랙 숫자
 * @param durationInMinutes CD 길이(단위: 분)
 */
public void addCD(String title, String author, int tracks, int durationInMinutes) {
    CD cd = new CD();
    cd.title = title;
    cd.author = author;
    cd.tracks = tracks;
    cd.duration = durationInMinutes;
    cdList.add(cd);
}
```

### 이력을 기록하는 주석

: 모듈을 편집할 때마다 **모듈 첫머리**에 주석을 추가하는 것

하지만, 이제는 소스 코드 관리 시스템이 있으니 전혀 필요없다.

```java
* 변경 이력 (11-Oct-2001부터)
* ------------------------------------------------
* 11-Oct-2001 : 클래스를 다시 정리하고 새로운 패키징
* 05-Nov-2001: getDescription() 메소드 추가
* 이하 생략
```

### 있으나 마나 한 주석

: 너무 **당연한 사실**을 **언급**하며 **새로운 정보**를 **제공하지 못하는** 주석

```java
/*
 * 기본 생성자
 */
protected AnnualDateRule() {
}
```

✔ 있으나 마나 한 주석을 달려는 유혹에서 벗어나 코드를 정리하자. 더 낫고, 행복한 프로그래머가 되는 지름길이다.

### 무서운 잡음

: Javadocs가 때로는 잡음이 될 수도 있다.

### **함수나 변수로 표현할 수 있다면 주석을 달지 마라**

```java
// 전역 목록 <smodule>에 속하는 모듈이 우리가 속한 하위 시스템에 의존하는가?
if (module.getDependSubsystems().contains(subSysMod.getSubSystem()))
```

주석을 제거하고 다시 표현하면 다음과 같다.

```java
ArrayList moduleDependencies = smodule.getDependSubSystems();
String ourSubSystem = subSysMod.getSubSystem();
if (moduleDependees.contains(ourSubSystem))
```

### 위치를 표시하는 주석

: 소스 파일에서 **특정 위치**를 **표시**하려 주석을 사용하는 것

```java
// Actions /////////////////////////////////////////////
```

### 닫는 괄호에 다는 주석

: 닫는 괄호에 **특수한 주석**을 달아놓는 것

```java
public class wc {
public static void main(String[] args) {
	String line;
	...
	try {
		while (line > 5) {
			...
		} // while
	} // try
	catch (Exception e) {
		...
	} // catch
} // main
}
```

### 공로를 돌리거나 저자를 표시하는 주석

소스 코드 관리 시스템은 누가 언제 무엇을 추가했는지 귀신처럼 기억하기 때문에 저자 이름으로 코드를 오염시킬 필요가 없다.

```java
/* 릭이 추가함 */
```

### 주석으로 처리한 코드

: **중요**하거나, **이유**가 있어 **남겨놓은 코드**를 주석 처리한 것

```java
this.bytePos = writeBytes(pngIdBytes, 0);
//hdrPos = bytePos;
writeHeader();
writeResolution();
//dataPos = bytePos;
if (writeImageData()) {
    wirteEnd();
    this.pngBytes = resizeByteArray(this.pngBytes, this.maxPos);
} else {
    this.pngBytes = null;
}
return this.pngBytes;
```

### HTML 주석

: 말 그대로 **HTML 주석**으로 다는 것

```java
/**
	* 적합성 테스트를 수행하기 위한 과업
	* 이 과업은 적합성 테스트를 수행해 결과를 출력한다.
	* <p />
	* <pre>
	* ...
*/
```

### 전역 정보

주석을 달아야 한다면 시스템의 전반적인 정보를 기술하지 말고, **근처에 있는 코드만 기술**해라.

해당 시스템의 코드가 변해도 아래 주석이 변하리라는 보장은 전혀 없다. 그리고 심하게 중복된 주석도 확인하자.

```java
/**
	* 적합성 테스트가 동작하는 포트: 기본값은 <b>8082</b>.
	*
	*	@param fitnessePort
	*/

public void setFitnessePort(int fitnessePort) {
	this.fitnewssePort = fitnessePort;
}
```

### 너무 많은 정보

주석에다 **흥미로운 역사**나 **관련 없는 정보**를 **장황하게 늘어놓지 마라.**

```java
/*
	RFC 2045 - Multipurpose Internet Mail Extensions (MINE)
	1부 : 인터넷 메세지 본체 형식
	6.8절.  Base64 내용 전송 인코딩(Content-Transfer-Encoding)
	인코딩 과정은 입력 비트 중 24비트 그룹을 인코딩된 4글자로 구성된
	...
*/
```

### 모호한 관계

주석과 주석이 설명하는 코드는 **둘 사이 관계**가 **명백**해야 한다.

```java
/*
 * 모든 픽셀을 담을 만큼 충분한 배열로 시작한다(여기에 필터 바이트를 더한다).
 * 그리고 헤더 정보를 위해 200바이트를 더한다.
 */
this.pngBytes = new byte[((this.width + 1) * this.height * 3) + 200];
```

주석을 다는 목적은 **코드만으로 설명이 부족해서** 다는 것인데, 이렇게 작성하면 주석 자체가 다시 설명을 요구하니 안타깝기 그지없다..

### 함수 헤더

짧은 함수는 긴 설명이 필요 X

✔ 짧고 한 가지만 수행하며 이름을 잘 붙인 함수가 주석으로 헤더를 추가한 함수보다 훨씬 좋다.

### 비공개 코드에서 Javadocs

공개 API는 Javadocs가 유용하지만 공개하지 않을 코드라면 Javadocs는 쓸모가 없다. 코드만 보기싫고 산만해질 뿐이다.
