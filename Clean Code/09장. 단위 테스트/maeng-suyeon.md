1997년만 해도 **TDD; Test Driven Development**라는 개념을 아무도 몰랐고, 대다수에게 단위 테스트란 프로그램이 <b>'돌아간다'<b/>는 사실만 확인하는 일회성 코드에 불과했다.

지금은 애자일과 TDD 덕택에 단위 테스트를 자동화하는 프로그래머들이 이미 많아졌으며 점점 더 늘어나는 추세다. 하지만 우리 분야에 테스트를 추가하려고 급하게 서두르는 와중에 많은 프로그래머들이 제대로 된 테스트 케이스를 작성해야 한다는 좀 더 미묘한(그리고 더욱 중요한) 사실을 놓쳐버렸다.

## TDD 법칙 세 가지

이제는 TDD가 실제 코드를 짜기 전에 **단위 테스트**부터 짜라고 요구한다는 사실을 모르는 사람이 없을 것이다. 하지만 이 규칙은 빙산의 일각에 불과하다. 다음 세 가지 법칙을 살펴보자.

- **첫째 법칙:** 실패하는 단위 테스트를 작성할 때까지 실제 코드를 작성하지 않는다.
- **둘째 법칙:** 컴파일은 실패하지 않으면서 실행이 실패하는 정도로만 단위 테스트를 작성한다.
- **셋째 법칙:** 현재 실패하는 테스트를 통과할 정도로만 실제 코드를 작성한다.

위 세 가지 규칙을 따르면 개발과 테스트가 대략 30초 주기로 묶인다. 테스트 코드와 실제 코드와 함께 나올뿐더러 테스트 코드가 실제 코드보다 불과 몇 초 전에 나온다.

✔️ 하지만, 실제 코드와 맞먹을 정도로 방대한 테스트 코드는 심각한 관리 문제를 유발하기도 한다.

## 깨끗한 테스트 코드 유지하기

실제 코드가 진화하면 테스트 코드도 변해야 된다.

실제 코드를 변경해 기존 테스트 케이스가 실패하기 시작하면, 지저분한 코드로 인해,  실패하는 테스트 케이스를 점점 더 통과시키기 어려워진다. 그래서 테스트 코드는 계속해서 늘어나는 부담이 되버린다.

✔️ 그래서 **테스트 코드는 실제 코드 못지 않게 중요하며, 사고**와 **설계**와 **주의**가 필요하다.

### 테스트는 유연성, 유지보수성, 재사용성을 제공한다

테스트 코드를 깨끗하게 유지하지 않으면 결국은 잃어버린다. 그리고, 테스트 케이스가 없으면 실제 코드를 유연하게 만드는 버팀목도 사라진다.  

그래서 코드에 **유연성**, **유지보수성**, **재사용성**을 제공하는 버팀목인 **단위 테스트**를 사용한다.

테스트 케이스가 없다면, 모든 변경이 잠정적인 버그가 되고, 개발자는 변경을 주저한다.

하지만, 테스트 케이스가 있다면 공포는 사라지고, 테스트 커버리지가 높을수록 더욱 공포는 줄어들어 엉망인 코드라도 별다른 우려 없이 변경할 수 있다.  

그러므로 실제 코드를 점검하는 자동화된 단위 테스트 슈트는 **설계**와 **아키텍처**를 최대한 **깨끗하게 보존**하는 **열쇠**다. 테스트 케이스가 있으면 **변경**이 쉬워지기 때문이다.

✔️ 따라서, 테스트 코드가 지저분하면 코드를 변경하는 능력이 떨어지며 코드 구조를 개선하는 능력도 떨어진다.

## 깨끗한 테스트 코드

깨끗한 테스트 코드를 만들려면, 3가지가 필요한데 그것은 **가독성, 가독성, 가독성**이다.

이처럼 가독성은 테스트 코드에 더더욱 중요하며, 이를 높이려면 최소의 표현으로 많은 것을 나타내야 하기 때문에 **명료성, 단순성, 풍부한 표현력**이 필요하다.

목록 9-1은 FitNess에서 가져온 코드다. 아래 테스트 케이스 세 개는 이해하기 어렵기에 개선할 여지가 충분하다. 첫째, addPage와 assertSubString을 부르느라 중복되는 코드가 매우 많다. 좀 더 중요하게는 자질구레한 사항이 너무 많아 테스트 코드의 표현력이 떨어진다.

목록 9-1 SerializedPageResponderTest.java

```java
public void testGetPageHieratchyAsXml() throws Exception {
  crawler.addPage(root, PathParser.parse("PageOne"));
  crawler.addPage(root, PathParser.parse("PageOne.ChildOne"));
  crawler.addPage(root, PathParser.parse("PageTwo"));

  request.setResource("root");
  request.addInput("type", "pages");
  Responder responder = new SerializedPageResponder();
  SimpleResponse response =
    (SimpleResponse) responder.makeResponse(new FitNesseContext(root), request);
  String xml = response.getContent();

  assertEquals("text/xml", response.getContentType());
  assertSubString("<name>PageOne</name>", xml);
  assertSubString("<name>PageTwo</name>", xml);
  assertSubString("<name>ChildOne</name>", xml);
}

public void testGetPageHieratchyAsXmlDoesntContainSymbolicLinks() throws Exception {
  WikiPage pageOne = crawler.addPage(root, PathParser.parse("PageOne"));
  crawler.addPage(root, PathParser.parse("PageOne.ChildOne"));
  crawler.addPage(root, PathParser.parse("PageTwo"));

  PageData data = pageOne.getData();
  WikiPageProperties properties = data.getProperties();
  WikiPageProperty symLinks = properties.set(SymbolicPage.PROPERTY_NAME);
  symLinks.set("SymPage", "PageTwo");
  pageOne.commit(data);

  request.setResource("root");
  request.addInput("type", "pages");
  Responder responder = new SerializedPageResponder();
  SimpleResponse response =
    (SimpleResponse) responder.makeResponse(new FitNesseContext(root), request);
  String xml = response.getContent();

  assertEquals("text/xml", response.getContentType());
  assertSubString("<name>PageOne</name>", xml);
  assertSubString("<name>PageTwo</name>", xml);
  assertSubString("<name>ChildOne</name>", xml);
  assertNotSubString("SymPage", xml);
}

public void testGetDataAsHtml() throws Exception {
  crawler.addPage(root, PathParser.parse("TestPageOne"), "test page");

  request.setResource("TestPageOne"); request.addInput("type", "data");
  Responder responder = new SerializedPageResponder();
  SimpleResponse response =
    (SimpleResponse) responder.makeResponse(new FitNesseContext(root), request);
  String xml = response.getContent();

  assertEquals("text/xml", response.getContentType());
  assertSubString("test page", xml);
  assertSubString("<Test", xml);
}
```

예를 들어, PathParser 호출을 살펴보자. PathParser는 문자열을 pagePath 인스턴스로 변환한다. 이 코드는 테스트와 무관하며 테스트 코드의 의도만 흐린다. responder 객체를 생성하는 코드와 response를 수집해 변환하는 코드 역시 잡음에 불과하다. 게다가 resource와 인수에서 요청 URL을 만드는 어설픈 코드도 보인다.

**마지막으로 위 코드는 읽는 사람을 고려하지 않는다.** 불쌍한 독자들은 온갖 잡다하고 무관한 코드를 이해한 후라야 간신히 테스트 케이스를 이해한다.

이제 목록 9-2를 살펴보자. 목록 9-1을 개선한 코드로, 목록 9-1과 정확히 동일한 테스트를 수행한다. 하지만 목록 9-2는 좀 더 깨끗하고 좀 더 이해하기 쉽다.

목록 9-2 SerializedPageResponderTest.java (refactored)

```java
public void testGetPageHierarchyAsXml() throws Exception {
  makePages("PageOne", "PageOne.ChildOne", "PageTwo");

  submitRequest("root", "type:pages");

  assertResponseIsXML();
  assertResponseContains(
    "<name>PageOne</name>", "<name>PageTwo</name>", "<name>ChildOne</name>");
}

public void testSymbolicLinksAreNotInXmlPageHierarchy() throws Exception {
  WikiPage page = makePage("PageOne");
  makePages("PageOne.ChildOne", "PageTwo");

  addLinkTo(page, "PageTwo", "SymPage");

  submitRequest("root", "type:pages");

  assertResponseIsXML();
  assertResponseContains(
    "<name>PageOne</name>", "<name>PageTwo</name>", "<name>ChildOne</name>");
  assertResponseDoesNotContain("SymPage");
}

public void testGetDataAsXml() throws Exception {
  makePageWithContent("TestPageOne", "test page");

  submitRequest("TestPageOne", "type:data");

  assertResponseIsXML();
  assertResponseContains("test page", "<Test");
}
```

**BUILD-OPERATE-CHECK 패턴이 위와 같은 테스트 구조에 적합하다.** 각 테스트는 명확히 세 부분으로 나눠진다. 첫 부분은 테스트 자료를 만든다. 두 번째 부분은 테스트 자료를 조작하며, 세 번째 부분은 조작한 결과가 올바른지 확인한다.

잡다하고 세세한 코드를 거의 다 없앴다는 사실에 주목한다. 테스트 코드는 본론에 돌입해 진짜 필요한 자료 유형과 함수만 사용한다. 그러므로 코드를 읽는 사람은 온갖 잡다하고 세세한 코드에 주눅들고 헷갈릴 필요 없이 코드가 수행하는 기능을 재빨리 이해한다.

### 도메인에 특화된 테스트 언어

목록 9-2는 도메인에 특화된 언어DSL로 테스트 코드를 구현하는 기법을 보여준다. 흔히 쓰는 시스템 조작 API를 사용하는 대신 API 위에 함수와 유틸리티를 구현한 후 그 함수와 유틸리티를 사용하므로 테스트 코드를 짜기도 읽기도 쉬워진다. 이렇게 구현한 유틸리티는 테스트 코드에서 사용하는 특수 API가 된다. 즉, 테스트를 구현하는 당사자와 나중에 테스트를 읽어볼 독자를 도와주는 테스트 언어이다.  

이런 테스트 API는 처음부터 설계된 API가 아니다. 잡다하고 세세한 사항으로 범벅된 코드를 계속 리팩터링하다가 진화된 API다. 내가 목록 9-1에서 목록 9-2로 리팩터링했듯이, 숙련된 개발자라면 자기 코드를 좀 더 간결하고 표현력이 풍부한 코드로 리팩터링해야 마땅하다.  

### 이중 표준

**테스트 API코드에 적용하는 표준은 실제 코드에 적용하는 표준과 확실히 다르다.** 단순하고, 간결하고, 표현력이 풍부해야 하지만, 실제 코드만큼 효율적일 필요는 없다. 실제 환경이 아니라 테스트 환경에서 돌아가는 코드이기 때문인데, 실제 환경과 테스트 환경은 요구사항이 판이하게 다르다.  

목록 9-3을 살펴보자. 온도가 '급격하게 떨어지면' 경보, 온풍기, 송풍기가 모두 가동되는지 확인하는 코드이다.  

**목록 9-3 [EnvironmentControllerTest.java](http://environmentcontrollertest.java)**

```java
@Test
public void turnOnLoTempAlarmAtThreashold() throws Exception {
  hw.setTemp(WAY_TOO_COLD); 
  controller.tic(); 
  assertTrue(hw.heaterState());   
  assertTrue(hw.blowerState()); 
  assertFalse(hw.coolerState()); 
  assertFalse(hw.hiTempAlarm());       
  assertTrue(hw.loTempAlarm());
}
```

물론 위 코드는 세세한 사항이 아주 많다. 예를 들어, tic함수가 무엇인지 지금은 신경쓰지 말자. 단지 시스템 최종 상태의 온도가 "급강하"했는지 그것만 신경 써서 살펴보기 바란다.  

목록 9-3을 읽으면 점검하는 상태 이름과 상태 값을 확인하느라 눈길이 이리저리 흩어진다.   heaterState라는 상태 이름을 확인하고 왼쪽으로 눈길을 돌려 assertTrue를 읽는다. 

이런식으로 모든 state를 확인해야 하면 따분하고 미덥잖다. 읽기가 어렵다. 그래서 목록 9-4와 같이 변환해 코드 가독성을 크게 높였다.  

**목록 9-4 EnvironmentControllerTest.java(리팩터링)**

```java
@Test
public void turnOnLoTempAlarmAtThreshold() throws Exception {
  wayTooCold();
  assertEquals("HBchL", hw.getState()); 
}
```

당연히 tic 함수는 wayTooCold라는 함수를 만들어 숨겼다. 그런데 assertEquals에 들어있는 이상한 문자열에 주목한다. 대문자는 '켜짐'이고 소문자는 '꺼짐'을 뜻한다. 문자는 항상 '{heater, blower, cooler, hi-temp-alarm, lo-temp-alarm}' 순서다.  

비롯 위 방식이 그릇된 정보를 피하라는 규칙의 위반에 가깝지만 여기서는 적절해 보인다. 일단 의미만 안다면 눈길이 문자열을 따라 움직이며 결과를 재빨리 판단한다. 테스트 코드를 읽기가 사뭇 즐거워진다. 목록 9-5를 살펴보면 테스트 코드를 이해하기 너무도 쉽다는 사실이 분명히 드러난다.  

**목록 9-5 EnvironmentControllerTest.java (bigger selection)**

```java
@Test
public void turnOnCoolerAndBlowerIfTooHot() throws Exception {
  tooHot();
  assertEquals("hBChl", hw.getState()); 
}
  
@Test
public void turnOnHeaterAndBlowerIfTooCold() throws Exception {
  tooCold();
  assertEquals("HBchl", hw.getState()); 
}

@Test
public void turnOnHiTempAlarmAtThreshold() throws Exception {
  wayTooHot();
  assertEquals("hBCHl", hw.getState()); 
}

@Test
public void turnOnLoTempAlarmAtThreshold() throws Exception {
  wayTooCold();
  assertEquals("HBchL", hw.getState()); 
}
```

목록 9-6은 'getState' 함수를 보여준다. 코드가 그리 효율적이지 못하다는 사실에 주목한다. 효율을 높이려면 StringBuffer가 더 적합하다.  

**목록 9-6 MockControlHardware.java**

```java
public String getState() {
  String state = "";
  state += heater ? "H" : "h"; 
  state += blower ? "B" : "b"; 
  state += cooler ? "C" : "c"; 
  state += hiTempAlarm ? "H" : "h"; 
  state += loTempAlarm ? "L" : "l"; 
  return state;
}
```

하지만 StringBuffer는 보기에 흉하다. 나는 실제 코드에서도 크게 무리가 아니라면 이를 피한다. 목록 9-6은 StringBuffer를 안 써서 치르는 대가가 미미하다. **실제 환경에서는 문제가 될 수 있지만 테스트 환경은 자원이 제한적일 가능성이 낮기 때문이다.**  

이것이 이중 표준의 본질이다. 실제 환경에서는 절대로 안 되지만 테스트 환경에서는 전혀 문제없는 방식이 있다. 대개 메모리나 CPU 효율과 관련 있는 경우다. **코드의 깨끗함과는 철저히 무관하다.**  

## 테스트 당 assert 하나

JUnit으로 테스트 코드를 짤 때는 함수마다 `assert` 문을 **단 하나만 사용**해야 한다고 주장하는 학파가 있다. 이렇게 작성하면, 결론이 하나로 나와 코드를 이해하기 쉽고 빠르다.  

목록 9-2 코드는, "출력이 XML이다"라는 assert문과 "특정 문자열을 포함한다"는 assert문을 하나로 병합하는 방식이 불합리해 보인다. 하지만 이를 테스트를 쪼개 각자가 assert를 수행하면 된다.

목록 9-7 SerializedPageResponderTest.java (단일 Assert)

```java
public void testGetPageHierarchyAsXml() throws Exception { 
  givenPages("PageOne", "PageOne.ChildOne", "PageTwo");
  
  whenRequestIsIssued("root", "type:pages");
  
  thenResponseShouldBeXML(); 
}

public void testGetPageHierarchyHasRightTags() throws Exception { 
  givenPages("PageOne", "PageOne.ChildOne", "PageTwo");
  
  whenRequestIsIssued("root", "type:pages");
  
  thenResponseShouldContain(
    "<name>PageOne</name>", "<name>PageTwo</name>", "<name>ChildOne</name>"
  ); 
}
```

위에서 함수 이름을 바꿔 given-when-then 이라는 관례를 사용했다는 사실에 주목한다. 그러면 테스트 코드를 읽기가 쉬워진다. 불행하게도 위에서 보듯이 테스트를 분리하면 중복되는 코드가 많아진다.

### 테스트 당 개념 하나

어쩌면 "테스트 함수마다 한 개념만 테스트하라"는 규칙이 더 낫겠다.

이것저것 잡다한 개념을 연속으로 테스트하는 긴 함수는 피한다.

목록 9-8은 독자적인 개념 세 개를 테스트하므로 독자적인 테스트 세 개로 쪼개야 마땅해 바람직하지 못한 테스트 함수다.  

목록 9-8

```java
/**
 * addMonth() 메서드를 테스트하는 장황한 코드
 */
public void testAddMonths() {
  SerialDate d1 = SerialDate.createInstance(31, 5, 2004);

  SerialDate d2 = SerialDate.addMonths(1, d1); 
  assertEquals(30, d2.getDayOfMonth()); 
  assertEquals(6, d2.getMonth()); 
  assertEquals(2004, d2.getYYYY());
  
  SerialDate d3 = SerialDate.addMonths(2, d1); 
  assertEquals(31, d3.getDayOfMonth()); 
  assertEquals(7, d3.getMonth()); 
  assertEquals(2004, d3.getYYYY());
  
  SerialDate d4 = SerialDate.addMonths(1, SerialDate.addMonths(1, d1)); 
  assertEquals(30, d4.getDayOfMonth());
  assertEquals(7, d4.getMonth());
  assertEquals(2004, d4.getYYYY());
}
```

셋으로 분리한 테스트 함수는 각각 다음 기능을 수행한다.

- (5월처럼) 31일로 끝나는 달의 마지막 날짜가 주어지는 경우
    1. (6월처럼) 30일로 끝나는 한 달을 더하면 날짜는 30일이 되어야지 31일이 되어서는 안 된다.
    2. 두 달을 더하면 그리고 두 번째 달이 31일로 끝나면 날짜는 31일이 되어야 한다.
- (6월처럼) 30일로 끝나는 달의 마지막 날짜가 주어지는 경우
    1. 31일로 끝나는 한 달을 더하면 날짜는 30일이 되어야지 31일이 되면 안 된다.  

개념들을 이렇게 정리해 표현하면 장황한 코드 속에 여러 개념을 테스트하고 있음을 알 수 있다. 이 경우 assert 문이 여럿이라는 사실이 문제가 아니라, 한 테스트 함수에서 여러 개념을 테스트한다는 사실이 문제다. 

✔️ 그러므로 가장 좋은 규칙은 <b>"개념 당 assert 문 수를 최소로 줄여라"</b>와 <b>"테스트 함수 하나는 개념 하나만 테스트하라"</b>라 하겠다.

## F.I.R.S.T.

깨끗한 테스트는 다음 다섯 가지 규칙을 따른다.

### 빠르게; Fast

: 테스트는 **빨라야** 한다.

⇒ **빨리 돌아야 한다는 말**이다. 테스트가 느리면 자주 돌릴 엄두를 못 낸다.

자주 돌리지 않으면 초반에 문제를 찾아내 고치지 못하고, 코드를 마음껏 정리하지도 못해 결국 코드 품질이 망가지기 시작한다.

### 독립적으로; Independent

: 각 테스트는 **서로 의존하면 안 된다.**

한 테스트가 다음 테스트가 실행될 환경을 준비해서는 안 된다. 각 테스트는 독립적으로 그리고 어떤 순서로 실행해도 괜찮아야 한다.

테스트가 서로에게 의존하면 하나가 실패할 때 나머지도 잇달아 실패하므로 **원인을 진단하기 어려워지며**, 후반 테스트가 **찾아내야 할** **결함이 숨겨진다.**

### 반복가능하게; Repeatable

: 테스트는 **어떤 환경에서도** 반복 가능해야 한다.

실제 환경, QA 환경, 버스를 타고 집으로 가는 길에 사용하는 (네트워크에 연결되지 않은) 노트북 환경에서도 실행할 수 있어야 한다.

### 자가검증하는; Self-Validating

: 테스트는 `bool` 값으로 **성공** 아닌 **실패**의 결과를 내야 한다.

통과 여부를 알려고 로그 파일을 읽게 / 텍스트 파일 두 개를 수작업으로 비교하게 만들어서는 안 된다.

테스트가 스스로 성공과 실패를 가늠하지 않는다면, 판단은 주관적이 되며 지루한 수작업 평가가 필요하게 된다.

### 적시에; Timely

: 테스트는 적시에 작성해야 한다.

단위 테스트는 테스트하려는 실제 코드를 구현하기 직전에 구현한다. 실제 코드를 구현한 다음에 테스트 코드를 만들면 실제 코드가 테스트하기 어렵다는 사실을 발견할지도 모른다.

## 결론

- 테스트 코드는 실제 코드의 **유연성, 유지보수성, 재사용성**을 **보존**하고 **강화**하기 때문에, 실제 코드만큼이나 프로젝트 건강에 **중요하다.**
- 그러므로 **지속적**으로 **깨끗하게**, **표현력**을 높이고 **간결하게** 관리하는 것이 좋다.
- 테스트 API를 구현해 **도메인 특화 언어; Domain Specific Language, DSL**을 만들자. 그러면 그만큼 테스트 코드를 짜기가 쉬워진다.
- 테스트 코드가 방치되어 망가지면 실제 코드도 망가진다. 테스트 코드를 깨끗하게 유지하자.
