🤔 **프로그래밍 초창기** 때는...

→ 루틴 / 하위 루틴

🤔 **포트란**과 **PL/1** 때는... 

→ 프로그램 / 하위 프로그램 / **함수**

❗ 이 중 함수만 살아 남았고, 어떤 프로그램이든 가장 기본적인 단위가 되었다.

FitNesse에서 찾은 긴 함수 코드를 살펴보자.

```java
public static String testableHtml(
	PageData pagaData,
	boolean includeSuiteSetup
) throws Exception {
	WikiPage wikiPage = pageData.getWikiPage();
	StringBuffer buffer = new StringBuffer();
	if (pageData.hasAttribute("Test")) {
		if (includeSuiteSetup) {
			WikiPage suiteSetup = 
				PageCrawlerImpl.getInheritedPage(
					SuiteResponder.SUITE_SETUP_NAME, wikiPage
				);
			if (suiteSetup != null) {
				WikiPagePath pagePath = 
					suiteSetup.getPageCrawler().getFullPath(suiteSetup);
				String pagePathName = PathParser.render(pagePath);
				buffer.append("!include -setup .")
							.append(pagePathName)
							.append("\n");
			}
		}
		WikiPage setup =
			PageCrawlerImpl.getInheritedPage("SetUp", wikiPage);
		if (setup != null) {
			WikiPagePath setupPath = 
				wikiPage.getPageCrawler().getFullPath(setup);
			String setupPathName = PathParser.render(setupPath);
			buffer.append("!include -setup .")
						.append(setupPathName)
						.append("\n");
		}
	}
	buffer.append(pageData.getContent());
	if (pageData.hasAttribute("Test")) {
		WikiPage teardown =
			PageCrawlerImpl.getInheritedPage("TearDown", wikiPage);
		if (tearDown != null) {
			WikiPagePath tearDownPath =
				wikiPage.getPageCrawler().getFullPath(teardown);
			String tearDownPathName = PathParser.render(tearDownPath);
			buffer.append("\n")
						.append("!include -teardown .")
						.append(tearDownPathName)
						.append("\n");
		}
		if (includeSuiteSetup) {
			WikiPage suiteTeardown = 
				PageCrawlerImpl.getInheritedPage(
					SuiteResponder.SUITE_TEARDOWN_NAME,
					wikiPage
				);
			if (suiteTeardown != null) {
				WikiPagePath pagePath = 
					suiteTeardown.getPageCrawler().getFullPath(suiteTeardown);
				String pagePathName = PathParser.render(pagePath);
				buffer.append("!include -teardown .")
							.append(pagePathName)
							.append("\n");
			}
		}
	}
	pageData.setContent(buffer.toString());
	return pageData.getHtml();
}
```
다양한 추상화 수준과 긴 코드로 아마 이해하기 힘들 것이다.  

이 코드를 변경해보면...

```java
public static String renderPageWithSetupAndTeardowns(
	PageData pageData, boolean isSuite
) throws Exception {
	boolean isTestPage = pageData.hasAttribute("Test");
	if (isTestPage){
		WikiPage testPage = pageData.getWikiPage();
		StringBuffer newPageContent = new StringBuffer();
		includeSetupPages(testPage, newPageContent, isSuite);
		newPageContent.append(pageData.getContent());
		includeTeardownPages(testPage, newPageContent, isSuite);
		pageData.setContent(newPageContent.toString());
	}
	return pageData.getHtml();
}
```
이렇게 메서드 몇 개를 추출하고, 이름 몇 개를 변경하고, 구조를 조금 변경했더니 9줄의 함수 의도가 표현됐다.


## 작게 만들어라!

- 함수를 만드는 규칙
    1. '작게!'
    2. '더 작게!'

앞서 작성한 코드보다 함수는 더 짧아야한다.

```java
public static String renderPageWithSetupAndTeardowns(
	PageData pageData, boolean isSuite) throws Exception {
	if (isTestPage(pageData)){
		includeSetupAndTeardownPages(pageData, isSuite);
	return pageData.getHtml();
}
```

### 블록과 들여쓰기

- `if / else` 문, `white` 문 등에 들어가는 블록은 **한 줄**이어야 한다.

    ⇒ 바깥을 감싸는 함수(enclosing function)이 작아지고, 블록 안에서 호출하는 함수 이름을 적절히 지으면 코드 이해도 쉽다.

- 중첩 구조가 생길만큼 함수가 커져서는 안 된다.

    ⇒ 함수에서 들여쓰기 수준은 1단이나 2단을 넘어서지 말자.

## 한 가지만 해라!

> **함수는 한 가지를 해야 한다. 그 한 가지를 잘 해야 한다. 그 한가지만을 해야 한다.**

앞서 축약한 코드가 어떤 것을 처리하는 지 보자.

1. 페이지가 테스트 페이지인지 판단한다.
2. 그렇다면 설정 페이지와 해제 페이지를 넣는다.
3. 페이지를 HTML로 렌더링한다.

위에서 언급하는 세 단계는 지정된 함수 이름 아래에서 추상화 수준이 하나다.

### 함수가 '한 가지'만 하는지 판단하는 방법

- 간단한 **TO** 문단으로 구분

    TO RenderPageWithSetupsAndTeardowns, 페이지가 테스트 페이지인지 확인한 후, 테스트 페이지라면 설정 페이지와 해제 페이지를 넣는다. 테스트 페이지든 아니든 페이지를 HTML로 렌더링한다.

    지정된 함수 이름 아래에서 추상화 수준이 하나인 단계만 수행한다면 그 함수는 한 가지 작업만 한다. 

    우리가 함수를 만드는 이유는 다음 추상화 수준에서 여러 단계로 나눠 수행하기 위해서이다.

- 단순히 다른 표현이 아닌, 의미 있는 이름으로 다른 함수를 추출할 수 없을 때
- 함수 내 섹션

    → 한 가지 작업만 하는 함수는 자연스럽게 섹션으로 나누기 어렵다.

## 함수 당 추상화 수준은 하나로!

함수가 확실히 '한 가지' 작업만 하려면 **함수 내 모든 문장의 추상화 수준**이 **동일**해야한다.

→ 앞서 작성한 예제 코드에서 예를 들자면,

- `getHtml()`은 추상화 수준이 **아주 높다**.
- `String pagePathName = PathParser.render(pagepath);`는 추상화 수준이 **중간**이다.
- `.append("\n")`와 같은 코드는 추상화 수준이 **아주 낮다**.

- 한 함수 내에 추상화 수준을 섞으면 특정 표현이 근본 개념인지 아니면 세부사항인지 구분하기 어려워 읽는 사람이 헷갈린다.

    → 더 나아가면, 깨어진 창문처럼 사람들이 함수에 세부사항을 점점 더 추가한다.

### 위에서 아래로 코드 읽기 : 내려가기 규칙

- 코드는 위에서 아래로 이야기처럼 읽혀야 좋다.

    ⇒ 함수 추상화 수준이 한 단계 낮은 함수가 오기 때문.

    ➕ `TO 문단`

## Switch 문

- `switch` 문은 작게 만들기 어렵다.

    → 단일 블록이나 함수를 선호하기 때문

- '한 가지' 작업만 하는 `switch` 문도 만들기 어렵다.

    → 본질적으로 `switch` 문은 N가지를 처리하기 때문

- 이런 각 `switch` 문을 저차원 클래스에 숨기고 절대로 반복하지 않기 위해 **다형성**을 이용한다.

```java
public Money calculatePay(Employee e)
throws InvalidEmployeeType {
	switch (e.type) {
		case COMMISSIONED:
			return calculateCommissionedPay(e);
		case HOURLY:
			return calculateHourlyPay(e);
		case SALARIED:
			return calculateSalariedPay(e);
		default:
			throw new InvalidEmployeeType(e.type);
	}
}
```
직원 유형에 따라 다른 값을 계산해 반환 해주는 함수 코드.

하지만 위 함수에는 몇 가지 문제가 있다.

1. 함수가 길다.
2. '한 가지' 작업만 수행하지 않는다.
3. `SRP(Single Responsibility Principle)`를 위반한다.

    → 코드를 변경할 이유가 여럿이기 때문

4. `OCP(Open Closed Principle)`를 위반한다.

    → 새 직원 유형을 추가할 때마다 코드를 변경하기 때문

하지만.. **가장 심각한 문제**는 위 함수와 구조가 동일한 함수가 **무한정 존재**한다는 사실..!

```java
isPayday(Employee e, Data date);

혹은

deliverPay(Employee e, Money pay);
```
가능성은 무한하다. 그리고 모두가 똑같이 유해한 구조다.

이 코드의 문제를 해결한다면...

```java
public abstract class Employee {
	public abstract boolean isPayday();
	public abstract Money calculatePay();
	public abstract void deliverPay(Money pay);
}
--------------
public interface EmployeeFactory {
	public Employee makeEmployee(EmployeeRecord r) throws InvalidEmployeeType;
}
--------------
public class EmployeeFactoryImpl implements EmployeeFactory {
	public Employee makeEmployee(EmployeeRecord r) throws InvalidEmployeeType {
		switch(r.type){
			case COMMISSIONED:
				return new CommissionedEmployee(r);
			case HOURLY:
				return new HourlyEmployee(r);
			case SALARIED:
				return new SalaridEmployee(r);
			default:
				throw new InvalidEmployeeType(r.type);
		}
	}
}
```
이렇게 구현하면, 다형성으로 인해 실제 파생 클래스의 함수가 실행된다.


## 서술적인 이름을 사용해라!

- 좋은 이름이 주는 가치는 아무리 강조해도 지나치지 않다.
- 워드의 원칙

    > "코드를 읽으면서 짐작했던 기능을 각 루틴이 그대로 수행한다면 깨끗한 코드라 불러도 되겠다."

- 함수가 **작고 단순**할 수록 서술적인 이름을 고르기도 쉽다.
- 짧고 어려운 이름보단 **길고 서술적인 이름**이 좋다.
- 여러 단어가 쉽게 읽히는 **명명법**을 사용하자.

    → 그 다음, 여러 단어를 사용해 함수 기능을 잘 표현하는 이름을 선택한다.

- 이름을 붙일 때는 **일관성**이 있어야 한다.

✔  서술적인 이름을 사용하면 개발자 머릿속에서도 설계가 뚜렷해져 코드를 개선하기 쉬워진다.

## 함수 인수

- 함수에서 이상적인 인수 개수는 0개(무항)다.

    → 그 다음은 1개(단항), 다음은 2개(이항)다.

    → 3개(삼항)는 가능한 피하는 편이 좋다.

    → 4개 이상(다항)은 특별한 이유가 필요하다. 특별한 이유가 있어도 사용하면 안 된다.

### **많이 쓰는 단항 형식**

- 인수에 질문을 던지는 경우`boolean fileExists(“MyFile”);`
- 인수를 뭔가로 변환해 결과를 변환하는 경우`InputStream fileOpen(“MyFile”);`
- 이벤트 함수일 경우 (이 경우에는 이벤트라는 사실이 코드에 명확하게 드러나야 한다.)

위의 3가지가 아니라면 단항 함수는 가급적 피하는 것이 좋다.

### **플래그 인수**

플래그 인수는 추하다. 쓰지마라. bool 값을 넘기는 것 자체가 그 함수는 한꺼번에 여러가지 일을 처리한다고 공표하는 것과 마찬가지다.

### **이항 함수**

단항 함수보다 이해하기가 어렵다. Point 클래스의 경우에는 이항 함수가 적절하다. 2개의 인수간의 자연적인 순서가 있어야함 `Point p = new Point(x,y);` 무조건 나쁜 것은 아니지만, 인수가 2개이니 만큼 이해가 어렵고 위험이 따르므로 가능하면 단항으로 바꾸도록

### **삼항 함수**

이항 함수보다 이해하기가 훨씬 어려우므로, 위험도 2배 이상 늘어난다. 삼항 함수를 만들 때는 신중히 고려하라.

### **인수 객체**

인수가 많이 필요할 경우, 일부 인수를 독자적인 클래스 변수로 선언할 가능성을 살펴보자 x,y를 인자로 넘기는 것보다 Point를 넘기는 것이 더 낫다.


## 인수 목록

때로는 인수 개수가 가변적인 함수도 필요하다.

 ex- `String.format()`

```java
String.format("%s worked %.2f hours.", name, hours);
```

```java
public String format(String format, Object... args);
```
이처럼 가변 인수 전부를 동등하게 취급하면 List 형 인수 하나로 취급할 수 있다.

가변 인수를 취하는 모든 함수에 같은 원리가 적용된다.

- 가변 인수를 취하는 함수는 단항, 이항, 삼항 함수로 취급할 수 있다.

    → 하지만 이를 넘어서는 인수를 사용할 경우에는 문제가 있다.

    ```java
    void monad(Integer... args);
    void dyad(String name, Integer... args);
    void triad(String name, int count, Integer... args);
    ```
    사실 따져보면 `String.format`은 이항 함수이다.

### 동사와 키워드

함수의 의도나 인수의 순서와 의도를 제대로 표현하려면 좋은 함수 이름이 필수다.

- 단항 함수는 함수와 인수가 동사/명사 쌍을 이뤄야 한다.

    ex- `write(name)` → `writeField(name)`

- 함수 이름에 인수 이름(키워드)을 추가하면, 인수 순서를 기억할 필요가 없다.

    ex- `assertEquals` → `assertExpectedEqualsActual(expected, actual)`

## 부수 효과를 일으키지 마라!

- 부수 효과는 **거짓말**이다.

    → 함수에서 한 가지를 하겠다고 약속하고선 남몰래 다른 짓도 한다..

    → 때로는 예상치 못하게 클래스 변수를 수정한다.

    → 때로는 함수로 넘어온 인수나 시스템 전역 변수를 수정한다.

    ⇒ 많은 경우 시간적인 결합(temporal coupling)이나 순서 종속성(order dependency)을 초래한다.

아래 코드에서 `Session.initialize();`는 함수명과는 맞지 않는 부수효과이다.

```java
public class UserValidator {
	private Cryptographer cryptographer;
	public boolean checkPassword(String userName, String password) { 
		User user = UserGateway.findByName(userName);
		if (user != User.NULL) {
			String codedPhrase = user.getPhraseEncodedByPassword(); 
			String phrase = cryptographer.decrypt(codedPhrase, password); 
			if ("Valid Password".equals(phrase)) {
				Session.initialize();
				return true; 
			}
		}
		return false; 
	}
}
```

### **출력인수**

일반적으로 출력 인수는 피해야 한다.함수에서 상태를 변경해야 한다면 함수가 속한 객체 상태를 변경하는 방식을 택하라.

## 명령과 조회를 분리하라!

함수는 `1. 뭔가를 수행하거나`, `2. 뭔가에 답하거나` 둘 중 하나만 해야 한다. 둘 다 하면 안 된다.

ex- 객체 상태를 변경하거나 아니면 객체 정보를 반환하거나

```java
public boolean set(String attribute, String value);

// 이름이 attribute인 속성을 찾아 값을 value로 설정 후 성공하면 true / 실패하면 false 반환
// 이렇게 되면...

if (set("username", "unclebob"))... 과 같은 괴상한 코드가 나온다!
```

이 코드를 해결하면..?

```java
if (attributeExists("username")){
	setAttribute("username", "unclebob");
	...
}
```
명령과 조회를 분리해 혼란을 아예 뿌리 뽑았다.

## 오류 코드보다 예외를 사용하라!

명령 함수에서 오류 코드를 반환하는 방식은 명령/조회 분리 규칙을 미묘하게 위반한다

→ if 문에서 명령을 표현식으로 사용하기 쉽다.

```java
if (deletePage(page) == E_OK);
```
이 코드는 동사/형용사 혼란을 일으키지 않지만, 여러 단계로 중첩되는 코드를 야기한다. 

```java
if (deletePage(page) == E_OK) {
	if (registry.deleteReference(page.name) == E_OK) {
		if (configKeys.deleteKey(page.name.makeKey()) == E_OK){
			logger.log("page deleted");
		} else {
			logger.log("configKey not deleted");
		}
	} else {
		logger.log("deleteReference from registry failed");
	}
} else {
	logger.log("delete failed");
	return E_ERROR;
}
```
위 코드에서 오류 코드 반환 시, 호출자는 오류 코드를 곧바로 처리해야한다는 문제가 발생하여 처리한 코드이다.

반면 오류 코드 대신 예외를 사용하면 오류 처리 코드가 원래 코드에서 분리되므로 코드가 깔끔해진다.

```java
try {
	deletePage(page);
	registry.deleteReference(page.name);
	configKeys.deleteKey(page.name.makeKey());
}
catch (Exception e) {
	logger.log(e.getMessage());
}
```

### Try/Catch 블록 뽑아내기

- try/catch 블록은 코드 구조에 혼란을 일으키고, 정상 동작가 오류 처리 동작을 뒤섞는다.

    ⇒ 그러므로, try/catch 블록을 별도 함수로 뽑아내는 편이 좋다.

### 오류 처리도 한 가지 작업이다.

- 함수는 '한 가지' 작업만 해야 한다.

    ⇒ 오류 처리도 '한 가지' 작업에 속해 오류를 처리하는 함수를 오류만 처리해야 한다.

    → 함수에 키워드 try가 있다면 try 문으로 시작해 catch/finally 문으로 끝나야 한다.

### Error.java 의존성 자석

오류 코드를 반환한다는 이야기는, 클래스든 열거형 변수든, 어디선가 오류 코드를 정의한다는 뜻이다.

→ 의존성 자석 클래스 예제 코드

```java
public enum Error {
	OK,
	INVALID,
	NO_SUCH,
	LOCKED,
	OUT_OF_RESOURCES,
	WAITING_FOR_EVENT;
}
```

## 반복하지 마라!

- 코드 길이가 늘어난다.
- 알고리즘이 변하면 여러 곳을 손 봐야 한다.
- 어느 한 곳을 빠뜨린다면 ... 오류가 발생할 확률이 높다.
- 중복을 없애면 가독성이 높아진다.
- 구조적 프로그래밍(AOP, COP)의 중복 제거 전략은 코드를 부모 클래스로 몰아넣는다.

## 구조적 프로그래밍

**데이크스트라**의 **구조적 프로그래밍 원칙**

: 모든 함수와 함수 내 모든 블록에 입구와 출구가 하나만 존재해야 한다.

⇒ 즉, 함수는 `return` 문이 하나여야 한다.

→ 루프 안에서 `break`나 `continue` 사용 X / `goto`는 절대로 사용 X

하지만, 구조적 프로그래밍의 목표와 규율은 공감하나 함수가 작다면 위 규칙은 별 이익을 제공하지 못한다. 함수가 아주 클 때만 상당한 이익을 제공하기 때문이다.

→ 그러므로 함수를 작게 만든다면 `return`, `break`, `continue`를 여러 차례 사용해도 괜찮다.

→ 오히려 때로는 단일 입/출구 규칙(single entry-exit rule)보다 의도를 표현하기 쉬워진다.

반면, `goto` 문은 큰 함수에서만 의미가 있으므로, 작은 함수에서는 피해야한다.

## 함수를 어떻게 짜죠?

- 소프트웨어를 짜는 행위는 여느 글짓기와 비슷하다.

    → 처음 함수를 짤 때에는, 길고 복잡하고, 들여쓰기 단계도 많고, 중복된 루프도 많고, 인수 목록도 아주 길고, 이름은 즉흥적이고, 코드가 중복되지만 이 서투른 코드를 빠짐없이 테스트하는 단위 테스트 케이스를 만든다.

    → 그 후 코드를 다듬고, 함수를 만들고, 이름을 바꾸고, 중복을 제거하고, 메서드를 줄이고, 순서를 바꾸고, 전체 클래스를 쪼개는데 항상 단위 테스트를 통과하곤 한다.

    ⇒ 이 말은 이 장에서 설명한 규칙을 따르는 함수가 얻어진다는 말이다.

## 결론

- 모든 시스템은 특정 응용 분야 시스템을 기술할 목적으로 프로그래머가 설계한 도메인 특화 언어(`DSL; Domain Specific Language`)로 만들어진다.
    - 함수는 그 언어에서 동사며, 클래스는 명사다.
- 대가(master) 프로그래머는 시스템을 **[구현할]** 프로그램이 아닌, **[풀어갈]** 이야기로 여긴다.
- 작성하는 함수가 분명하고 정확한 언어로 깔끔하게 같이 맞아떨어져야 이야기를 풀어가기가 쉬워진다는 사실을 기억하자.
