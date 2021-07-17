# 📚 03장. 함수

## 작게 만들어라

- [ ] if, while 문 등에 들어가는 블록은 한 줄이어야 한다
- [ ] 함수에서 들여쓰기 수준은 1단이나 2단을 넘어서면 안 된다

<br />

## 한 가지만 해라

> **함수는 한 가지를 해야 한다**

- 함수 내 모든 문장의 ```*```추상화 수준이 동일해야 한다
- 의미 있는 이름으로 다른 함수를 추출할 수 없게 하라

_```*``` 다음 목차 참고_

<br />

## 함수 당 추상화 수준은 하나로

**추상화 수준**

- 매우 높음
```java
getHtml()
```
- 중간
```java
String pagePathName = PathParser.render(pagepath);
```
- 매우 낮음
```java
.append("\n")
```

<br />

## Switch문

- 작게 만들기 어렵다
- 길이가 길다
- N가지를 처리한다

피할 수 없다면 저차원 클래스에 숨기고 절대로 반복하지 않는다

<br />

## 서술적인 이름을 사용하라

> 함수가 하는 일을 더 잘 표현하도록 좋은 이름을 지어라

**일관성**

모듈 내에서 함수 이름은 같은 문구, 명사, 동사를 사용한다
```
includeSetupAndTeardownPages
includeSetupPages
includeSuiteSetupPage
includeSetupPage
```

<br />

## 함수 인수

### 0개 (무항)

이상적인 인수 개수로, 인수가 없다면 간단하다

### 1개 (단항)

- 인수에 질문을 던지는 경우
```java
boolean fileExists("MyFile")
```
- 인수를 변환해 결과를 반환하는 경우
```java
InputStream fileOpen("MyFile")  // String 파일 이름 -> InputStream
```
- 이벤트
```java
passwordAttemptFailedNtimes(int attempts)
```

**플래그 인수**

플래그 인수는 함수가 한꺼번에 여러 가지를 처리한다고 대놓고 공표하는 셈이다   
~~끔찍하다~~

### 2개 (이항)

인수가 1개인 함수보다 이해하기 어렵다

```java
// 적절한 경우
Point p = new Point(0, 0);
```

자연적인 순서가 없는 이항 함수는 위험이 따른다

### 3개 (삼항)

인수가 2개인 함수보다 훨씬 더 이해하기 어렵다   
가능한 피하는 것이 좋다

### 인수 객체

```java
Circle makeCircle(double x, double y, double radius);
Circle makeCircle(Point center, double radius);   // 개념도 표현
```

### 가변 인수

사실상 이항 함수이다

```java
public String format(String format, Object... args)
// String.format("%s worked %.2f hours.", name, hours);
```

### 동사와 키워드

동사 + 키워드로 함수 이름을 정한다   
```assertEquals``` → ```assertExpectedEqualsActual(expected, actual)```

<br />

## 부수 효과를 일으키지 마라

- 클래스 변수 수정
- 인수, 시스템 전역 변수 수정 등

⇒ **시간적인 결합**이나 **순서 종속성**을 초래함

### 출력 인수

출력 인수는 피해야 한다

```java
appendFooter(report);   // Bad
report.appendFooter();  // Good
```

<br />

## 명령과 조회를 분리하라

아래 중 하나만 해야 한다

1. 객체 상태 변경
2. 객체 정보 반환

<br />

## 오류 코드보다 예외를 사용하라

- 오류 처리 코드가 원래 코드에서 분리되므로 코드가 깔끔해진다
- 재컴파일/재배치 없이도 새 예외 클래스를 추가할 수 있다   
오류 코드를 정의하는 의존성 자석이 변경되면 그것을 사용하는 클래스 전부를 재컴파일/재배치 해야 한다

**오류 처리**

- try/catch 블록을 별도 함수로 뽑아내는 편이 좋다
- 오류를 처리하는 함수는 오류만 처리해야 마땅하다

<br />

## 반복하지 마라

- 코드의 길이가 늘어난다
- 알고리즘이 변하면 반복한 부분 모두 수정해야 한다   
⇒ 오류가 발생할 확률이 높다

<br />

## 구조적 프로그래밍

> 모든 함수와 함수 내 모든 블록에 입구와 출구는 하나만 존재해야 한다   
> Edsger Dijkstra

⇒ 함수는 ```return``` 문이 하나여야 한다는 말이다

함수가 아주 클 때만 상당한 이익을 제공한다

<br />

## 결론

```java
// 40 목록 3-1 HtmlUtil.java (FitNesse 20070619)
public class HtmlUtil {
    public static String testableHtml(
            PageData pageData,
            boolean includeSuiteSetup
    ) throws Exception {
        WikiPage wikiPage = pageData.getWikiPage();
        StringBuffer buffer = new StringBuffer();
        if (pageData.hasAttribute("Test")) {
            if (includeSuiteSetup) {
                WikiPage suiteSetup =
                        pageCrawlerImpl.getInheritedPage(
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
            if (teardown != null) {
                WikiPagePath tearDownPath =
                        wikiPage.getPageCrawler().getFullPath(teardown);
                String tearDownPathName = PathParser.render(tearDownPath);
                buffer.append("!include -teardown .")
                        .append(tearDownPathName)
                        .append("\n");
            }
            if (includeSuiteSetup) {
                WikiPage suiteTearDown =
                        pageCrawlerImpl.getInheritedPage(
                                SuiteResponder.SUITE_TEARDOWN_NAME,
                                wikiPage
                        );
                if (suiteTearDown != null) {
                    WikiPagePath pagePath =
                            suiteTearDown.getPageCrawler().getFullPath(suiteTearDown);
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
}
```

🔽

```java
// 62 목록 3-7 SetupTeardownIncluder.java
package fitnesse.html;

import fitnesse.responders.run.SuiteResponder;
import fitnesse.wiki.*;

public class SetupTeardownIncluder {
    private PageData pageData;
    private boolean isSuite;
    private WikiPage testPage;
    private StringBuffer newPageContent;
    private PageCrawler pageCrawler;

    public static String render(PageData pageData) throws Exception {
        return render(pageData, false);
    }

    public static String render(PageData pageData, boolean isSuite)
        throws Exception {
        return new SetupTeardownIncluder(pageData).render(isSuite);
    }

    private SetupTeardownIncluder(PageData pageData) {
        this.pageData = pageData;
        testPage = pageData.getWikiPage();
        pageCrawler = testPage.getPageCrawler();
        newPageContent = new StringBuffer();
    }

    private String render(boolean isSuite) throws Exception {
        this.isSuite = isSuite;
        if (isTestPage())
            includeSetupAndTeardownPages();
        return pageData.getHtml();
    }

    private boolean isTestPage() throws Exception {
        return pageData.hasAttribute("Test");
    }

    private void includeSetupAndTeardownPages() throws Exception {
        includeSetupPages();
        includePageContent();
        includeTeardownPages();
        updatePageContent();
    }

    private void includeSetupPages() throws Exception {
        if (isSuite)
            includeSuiteSetupPage();
        includeSetupPage();
    }

    private void includeSuiteSetupPage() throws Exception {
        include(SuiteResponder.SUITE_SETUP_NAME, "-setup");
    }

    private void includeSetupPage() throws Exception {
        include("SetUp", "-setup");
    }

    private void includePageContent() throws Exception {
        newPageContent.append(pageData.getContent());
    }

    private void includeTeardownPages() throws Exception {
        includeTeardownPage();
        if (isSuite)
            includeSuiteTeardownPage();
    }

    private void includeTeardownPage() throws Exception {
        include("TearDown", "-teardown");
    }

    private void includeSuiteTeardownPage() throws Exception {
        include(SuiteResponder.SUITE_TEARDOWN_NAME, "-teardown");
    }

    private void updatePageContent() throws Exception {
        pageData.setContent(newPageContent.toString());
    }

    private void include(String pageName, String arg) throws Exception {
        WikiPage inheritedPage = findInheritedPage(pageName);
        if (inheritedPage != null) {
            String pagePathName = getPathNameForPage(inheritedPage);
            buildIncludeDirective(pagePathName, arg);
        }
    }

    private WikiPage findInheritedPage(String pageName) throws Exception {
        return PageCrawlerImpl.getInheritedPage(pageName, testPage);
    }

    private String getPathNameForPage(WikiPage page) throws Exception {
        WikiPagePath pagePath = pageCrawler.getFullPath(page);
        return PathParser.render(pagePath);
    }

    private void buildIncludeDirective(String pagePathName, String arg) {
        newPageContent
                .append("\n!include ")
                .append(arg)
                .append(" .")
                .append(pagePathName)
                .append(pagePathName)
                .append("\n");
    }
}
```

<br />

### 몰랐던 것

**SRP** 단일 책임 원칙   
: 어떤 클래스를 변경해야 하는 이유는 오직 하나뿐이어야 한다

**OCP** 개방 폐쇄 원칙   
: 확장엔 개방되어 있어야 하고, 변경엔 닫혀 있어야 한다

**attempt** 시도

<br />

## 기억에 남은 부분

> _처음에는 길고 복잡하다. 처음부터 탁 짜내지 않는다. 그게 가능한 사람은 없으리라._

<br />

## To Do

- [ ] 47 Switch문 이해
