# ๐ 03์ฅ. ํจ์

## ์๊ฒ ๋ง๋ค์ด๋ผ

- [ ] if, while ๋ฌธ ๋ฑ์ ๋ค์ด๊ฐ๋ ๋ธ๋ก์ ํ ์ค์ด์ด์ผ ํ๋ค
- [ ] ํจ์์์ ๋ค์ฌ์ฐ๊ธฐ ์์ค์ 1๋จ์ด๋ 2๋จ์ ๋์ด์๋ฉด ์ ๋๋ค

<br />

## ํ ๊ฐ์ง๋ง ํด๋ผ

> **ํจ์๋ ํ ๊ฐ์ง๋ฅผ ํด์ผ ํ๋ค**

- ํจ์ ๋ด ๋ชจ๋  ๋ฌธ์ฅ์ ```*```์ถ์ํ ์์ค์ด ๋์ผํด์ผ ํ๋ค
- ์๋ฏธ ์๋ ์ด๋ฆ์ผ๋ก ๋ค๋ฅธ ํจ์๋ฅผ ์ถ์ถํ  ์ ์๊ฒ ํ๋ผ

_```*``` ๋ค์ ๋ชฉ์ฐจ ์ฐธ๊ณ _

<br />

## ํจ์ ๋น ์ถ์ํ ์์ค์ ํ๋๋ก

**์ถ์ํ ์์ค**

- ๋งค์ฐ ๋์
```java
getHtml()
```
- ์ค๊ฐ
```java
String pagePathName = PathParser.render(pagepath);
```
- ๋งค์ฐ ๋ฎ์
```java
.append("\n")
```

<br />

## Switch๋ฌธ

- ์๊ฒ ๋ง๋ค๊ธฐ ์ด๋ ต๋ค
- ๊ธธ์ด๊ฐ ๊ธธ๋ค
- N๊ฐ์ง๋ฅผ ์ฒ๋ฆฌํ๋ค

ํผํ  ์ ์๋ค๋ฉด ์ ์ฐจ์ ํด๋์ค์ ์จ๊ธฐ๊ณ  ์ ๋๋ก ๋ฐ๋ณตํ์ง ์๋๋ค

<br />

## ์์ ์ ์ธ ์ด๋ฆ์ ์ฌ์ฉํ๋ผ

> ํจ์๊ฐ ํ๋ ์ผ์ ๋ ์ ํํํ๋๋ก ์ข์ ์ด๋ฆ์ ์ง์ด๋ผ

**์ผ๊ด์ฑ**

๋ชจ๋ ๋ด์์ ํจ์ ์ด๋ฆ์ ๊ฐ์ ๋ฌธ๊ตฌ, ๋ช์ฌ, ๋์ฌ๋ฅผ ์ฌ์ฉํ๋ค
```
includeSetupAndTeardownPages
includeSetupPages
includeSuiteSetupPage
includeSetupPage
```

<br />

## ํจ์ ์ธ์

### 0๊ฐ (๋ฌดํญ)

์ด์์ ์ธ ์ธ์ ๊ฐ์๋ก, ์ธ์๊ฐ ์๋ค๋ฉด ๊ฐ๋จํ๋ค

### 1๊ฐ (๋จํญ)

- ์ธ์์ ์ง๋ฌธ์ ๋์ง๋ ๊ฒฝ์ฐ
```java
boolean fileExists("MyFile")
```
- ์ธ์๋ฅผ ๋ณํํด ๊ฒฐ๊ณผ๋ฅผ ๋ฐํํ๋ ๊ฒฝ์ฐ
```java
InputStream fileOpen("MyFile")  // String ํ์ผ ์ด๋ฆ -> InputStream
```
- ์ด๋ฒคํธ
```java
passwordAttemptFailedNtimes(int attempts)
```

**ํ๋๊ทธ ์ธ์**

ํ๋๊ทธ ์ธ์๋ ํจ์๊ฐ ํ๊บผ๋ฒ์ ์ฌ๋ฌ ๊ฐ์ง๋ฅผ ์ฒ๋ฆฌํ๋ค๊ณ  ๋๋๊ณ  ๊ณตํํ๋ ์์ด๋ค   
~~๋์ฐํ๋ค~~

### 2๊ฐ (์ดํญ)

์ธ์๊ฐ 1๊ฐ์ธ ํจ์๋ณด๋ค ์ดํดํ๊ธฐ ์ด๋ ต๋ค

```java
// ์ ์ ํ ๊ฒฝ์ฐ
Point p = new Point(0, 0);
```

์์ฐ์ ์ธ ์์๊ฐ ์๋ ์ดํญ ํจ์๋ ์ํ์ด ๋ฐ๋ฅธ๋ค

### 3๊ฐ (์ผํญ)

์ธ์๊ฐ 2๊ฐ์ธ ํจ์๋ณด๋ค ํจ์ฌ ๋ ์ดํดํ๊ธฐ ์ด๋ ต๋ค   
๊ฐ๋ฅํ ํผํ๋ ๊ฒ์ด ์ข๋ค

### ์ธ์ ๊ฐ์ฒด

```java
Circle makeCircle(double x, double y, double radius);
Circle makeCircle(Point center, double radius);   // ๊ฐ๋๋ ํํ
```

### ๊ฐ๋ณ ์ธ์

์ฌ์ค์ ์ดํญ ํจ์์ด๋ค

```java
public String format(String format, Object... args)
// String.format("%s worked %.2f hours.", name, hours);
```

### ๋์ฌ์ ํค์๋

๋์ฌ + ํค์๋๋ก ํจ์ ์ด๋ฆ์ ์ ํ๋ค   
```assertEquals``` โ ```assertExpectedEqualsActual(expected, actual)```

<br />

## ๋ถ์ ํจ๊ณผ๋ฅผ ์ผ์ผํค์ง ๋ง๋ผ

- ํด๋์ค ๋ณ์ ์์ 
- ์ธ์, ์์คํ ์ ์ญ ๋ณ์ ์์  ๋ฑ

โ **์๊ฐ์ ์ธ ๊ฒฐํฉ**์ด๋ **์์ ์ข์์ฑ**์ ์ด๋ํจ

### ์ถ๋ ฅ ์ธ์

์ถ๋ ฅ ์ธ์๋ ํผํด์ผ ํ๋ค

```java
appendFooter(report);   // Bad
report.appendFooter();  // Good
```

<br />

## ๋ช๋ น๊ณผ ์กฐํ๋ฅผ ๋ถ๋ฆฌํ๋ผ

์๋ ์ค ํ๋๋ง ํด์ผ ํ๋ค

1. ๊ฐ์ฒด ์ํ ๋ณ๊ฒฝ
2. ๊ฐ์ฒด ์ ๋ณด ๋ฐํ

<br />

## ์ค๋ฅ ์ฝ๋๋ณด๋ค ์์ธ๋ฅผ ์ฌ์ฉํ๋ผ

- ์ค๋ฅ ์ฒ๋ฆฌ ์ฝ๋๊ฐ ์๋ ์ฝ๋์์ ๋ถ๋ฆฌ๋๋ฏ๋ก ์ฝ๋๊ฐ ๊น๋ํด์ง๋ค
- ์ฌ์ปดํ์ผ/์ฌ๋ฐฐ์น ์์ด๋ ์ ์์ธ ํด๋์ค๋ฅผ ์ถ๊ฐํ  ์ ์๋ค   
์ค๋ฅ ์ฝ๋๋ฅผ ์ ์ํ๋ ์์กด์ฑ ์์์ด ๋ณ๊ฒฝ๋๋ฉด ๊ทธ๊ฒ์ ์ฌ์ฉํ๋ ํด๋์ค ์ ๋ถ๋ฅผ ์ฌ์ปดํ์ผ/์ฌ๋ฐฐ์น ํด์ผ ํ๋ค

**์ค๋ฅ ์ฒ๋ฆฌ**

- try/catch ๋ธ๋ก์ ๋ณ๋ ํจ์๋ก ๋ฝ์๋ด๋ ํธ์ด ์ข๋ค
- ์ค๋ฅ๋ฅผ ์ฒ๋ฆฌํ๋ ํจ์๋ ์ค๋ฅ๋ง ์ฒ๋ฆฌํด์ผ ๋ง๋ํ๋ค

<br />

## ๋ฐ๋ณตํ์ง ๋ง๋ผ

- ์ฝ๋์ ๊ธธ์ด๊ฐ ๋์ด๋๋ค
- ์๊ณ ๋ฆฌ์ฆ์ด ๋ณํ๋ฉด ๋ฐ๋ณตํ ๋ถ๋ถ ๋ชจ๋ ์์ ํด์ผ ํ๋ค   
โ ์ค๋ฅ๊ฐ ๋ฐ์ํ  ํ๋ฅ ์ด ๋๋ค

<br />

## ๊ตฌ์กฐ์  ํ๋ก๊ทธ๋๋ฐ

> ๋ชจ๋  ํจ์์ ํจ์ ๋ด ๋ชจ๋  ๋ธ๋ก์ ์๊ตฌ์ ์ถ๊ตฌ๋ ํ๋๋ง ์กด์ฌํด์ผ ํ๋ค   
> Edsger Dijkstra

โ ํจ์๋ ```return``` ๋ฌธ์ด ํ๋์ฌ์ผ ํ๋ค๋ ๋ง์ด๋ค

ํจ์๊ฐ ์์ฃผ ํด ๋๋ง ์๋นํ ์ด์ต์ ์ ๊ณตํ๋ค

<br />

## ๊ฒฐ๋ก 

```java
// 40 ๋ชฉ๋ก 3-1 HtmlUtil.java (FitNesse 20070619)
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

๐ฝ

```java
// 62 ๋ชฉ๋ก 3-7 SetupTeardownIncluder.java
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

### ๋ชฐ๋๋ ๊ฒ

**SRP** ๋จ์ผ ์ฑ์ ์์น   
: ์ด๋ค ํด๋์ค๋ฅผ ๋ณ๊ฒฝํด์ผ ํ๋ ์ด์ ๋ ์ค์ง ํ๋๋ฟ์ด์ด์ผ ํ๋ค

**OCP** ๊ฐ๋ฐฉ ํ์ ์์น   
: ํ์ฅ์ ๊ฐ๋ฐฉ๋์ด ์์ด์ผ ํ๊ณ , ๋ณ๊ฒฝ์ ๋ซํ ์์ด์ผ ํ๋ค

**attempt** ์๋

<br />

## ๊ธฐ์ต์ ๋จ์ ๋ถ๋ถ

> _์ฒ์์๋ ๊ธธ๊ณ  ๋ณต์กํ๋ค. ์ฒ์๋ถํฐ ํ ์ง๋ด์ง ์๋๋ค. ๊ทธ๊ฒ ๊ฐ๋ฅํ ์ฌ๋์ ์์ผ๋ฆฌ๋ผ._

<br />

## To Do

- [ ] 47 Switch๋ฌธ ์ดํด
