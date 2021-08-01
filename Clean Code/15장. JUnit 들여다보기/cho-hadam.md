# 📓 15장. JUnit 들여다보기

<br />

## JUnit 프레임워크

```ComparisonCompactor```는 두 문자열을 받아 차이를 반환한다   
예를 들어, ```ABCDE```와 ```ABXDE```를 받아 ```<...B[X]D...>```를 반환한다

<br />

## ComparisonCompactor 모듈

```java
package junit.framework;

public class OriginalComparisonCompactor {

  private static final String ELLIPSIS = "...";
  private static final String DELTA_END = "]";
  private static final String DELTA_START = "[";

  private int fContextLength;
  private String fExpected;
  private String fActual;
  private int fPrefix;
  private int fSuffix;

  public OriginalComparisonCompactor(int contextLength,
                                     String expected,
                                     String actual) {
    fContextLength = contextLength;
    fExpected = expected;
    fActual = actual;
  }

  public String compact(String message) {
    if (fExpected == null || fActual == null || areStringsEqual())
      return Assert.format(message, fExpected, fActual);

    findCommonPrefix();
    findCommonSuffix();
    String expected = compactString(fExpected);
    String actual = compactString(fActual);
    return Assert.format(message, expected, actual);
  }

  private String compactString(String source) {
    String result = DELTA_START +
                      source.substring(fPrefix, source.length() -
                                        fSuffix + 1) + DELTA_END;
    if (fPrefix > 0)
      result = computeCommonPrefix() + result;
    if (fSuffix > 0)
      result = result + computeCommonSuffix();
    return result;
  }

  public void findCommonPrefix() {
    fPrefix = 0;
    int end = Math.min(fExpected.length(), fActual.length());
    for (; fPrefix < end; fPrefix++) {
      if (fExpected.charAt(fPrefix) != fActual.charAt(fPrefix))
        break;
    }
  }

  public void findCommonSuffix() {
    int expectedSuffix = fExpected.length() - 1;
    int actualSuffix = fActual.length() - 1;
    for (;
         actualSuffix >= fPrefix && expectedSuffix >= fPrefix;
         actualSuffix--, expectedSuffix--) {
      if (fExpected.charAt(expectedSuffix) != fActual.charAt(actualSuffix))
        break;
    }
  }

  private String computeCommonPrefix() {
    return (fPrefix > fContextLength ? ELLIPSIS : "") +
              fExpected.substring(Math.max(0, fPrefix - fContextLength), fPrefix);
  }

  private String computeCommonSuffix() {
    int end = Math.min(fExpected.length() - fSuffix + 1 + fContextLength,
                          fExpected.length());
    return fExpected.substring(fExpected.length() - fSuffix + 1, end) +
            (fExpected.length() - fSuffix + 1 < fExpected.length() -
              fContextLength ? ELLIPSIS : "");
  }

  public boolean areStringsEqual() {
    return fExpected.equals(fActual);
  }
}
```

전반적으로 상당히 훌륭한 모듈이다

하지만 **보이스카우트 규칙**에 따르면 우리는 처음 왔을 때보다 더 깨끗하게 해놓고 떠나야 한다

<br />

## 거슬리는 부분

### 멤버 변수 앞에 붙인 접두어 f   
```java
private int fContextLength;
private String fExpected;
private String fActual;
private int fPrefix;
private int fSuffix;
```
🔽
```java
private int contextLength;
private String expected;
private String actual;
private int prefix;
private int suffix;
```

<br />

### compact 함수 시작부에 캡슐화되지 않은 조건문   
```java
if (expected == null || actual == null || areStringsEqual())
```
🔽
```java
if (shouldNotCompact())

private boolean shouldNotCompact() {
  return expected == null || actual == null || areStringsEqual()
}
```

<br />

### compact 함수에서 사용하는 this.expected와 this.actual   
```java
// 이름은 명확하게 붙인다
String compactExpected = compactString(expected);
String compactActual = compactString(actual);
```

<br />

### 부정문은 긍정문보다 이해하기 약간 더 어렵다   
```java
if (canBeCompacted())

private boolean canBeCompacted() {
  return expected != null && actual != null && !areStringsEqual()
}
```

<br />

### compact 함수 이름이 이상하다   
실제로 ```canBeCompacted```가 ```false```이면 압축하지 않는다   
```java
// 이름 변경
public String formatCompactedComparison(String message) {
```

<br />

### 함수 사용방식이 일관적이지 못하다   
반환값이 없기도 있기도 하다   
```java
private void compactExpectedAndActual() {
  findCommonPrefix();
  findCommonSuffix();
  compactExpected = compactString(expected);
  compactActual = compactString(actual);
}
```
🔽
```java
private void compactExpectedAndActual() {
  // 멤버 변수 이름 변경도 함께 함 (바로 아래 내용)
  prefixIndex = findCommonPrefix();
  suffixIndex = findCommonSuffix();
  compactExpected = compactString(expected);
  compactActual = compactString(actual);
}
```

<br />

### 멤버 변수 이름도 모호하다   
```java
private int prefix;
private int suffix;
```
🔽
```java
private int prefixIndex;
private int suffixIndex;
```

<br />

### 숨겨진 시간적인 결합```hidden temporal coupling```이 존재한다   
```findCommonSuffix```는 ```findCommonPrefix```가 ```prefixIndex```를 계산한다는 사실에 의존한다   
```java
prefixIndex = findCommonPrefix();
suffixIndex = findCommonSuffix();
```
🔽
```java
findCommonPrefixAndSuffix();

private void findCommonPrefixAndSuffix() {
  findCommonPrefix();
  // ... findCommonSuffix 내용
}
```

<br />

### 고치고 나니, ```suffixIndex```의 이름이 적절하지 못하다  
```prefixIndex```도 마찬가지로, 이 경우 ```index```와 ```length```가 동의어다    
```java
private int prefixIndex;
private int suffixIndex;
```
🔽
```java
private int prefixLength;
private int suffixLength;
```

<br />

### ```compactString```에 있는 if 문 둘 다 필요없어 보인다   
```java
if (fPrefix > 0)
  result = computeCommonPrefix() + result;
if (fSuffix > 0)
  result = result + computeCommonSuffix();
```
🔽
```java
return
  computeCommonPrefix() +
  DELTA_START +
  source.substring(prefixLength, source.length() - suffixLength()) +
  DELTA_END +
  computeCommonSuffix();
```

<br />

## 최종 코드

```java
package junit.framework;

public class ComparisonCompactor {

  private static final String ELLIPSIS = "...";
  private static final String DELTA_END = "]";
  private static final String DELTA_START = "[";

  private int contextLength;
  private String expected;
  private String actual;
  private int prefixLength;
  private int suffixLength;

  public ComparisonCompactor(int contextLength, String expected, String actual) {
    this.contextLength = contextLength;
    this.expected = expected;
    this.actual = actual;
  }

  public String formatCompactedComparison(String message) {
    String compactExpected = expected;
    String compactActual = actual;
    if (shouldBeCompacted()) {
      findCommonPrefixAndSuffix();
      compactExpected = compact(expected);
      compactActual = compact(actual);
    }
    return Assert.format(message, compactExpected, compactActual);
  }

  private boolean shouldBeCompacted() {
    return !shouldNotBeCompacted();
  }

  private boolean shouldNotBeCompacted() {
    return expected == null ||
            actual == null ||
            expected.equals(actual);
  }

  private void findCommonPrefixAndSuffix() {
    findCommonPrefix();
    suffixLength = 0;
    for (; !suffixOverlapsPrefix(); suffixLength++) {
      if (charFromEnd(expected, suffixLength) !=
          charFromEnd(actual, suffixLength)
      )
        break;
    }
  }

  private char charFromEnd(String s, int i) {
    return s.charAt(s.length() - i - 1);
  }

  private boolean suffixOverlapsPrefix() {
    return actual.length() - suffixLength <= prefixLength ||
            expected.length() - suffixLength <= prefixLength;
  }

  private void findCommonPrefix() {
    prefixLength = 0;
    int end = Math.min(expected.length(), actual.length());
    for (; prefixLength < end; prefixLength++)
      if (expected.charAt(prefixLength) != actual.charAt(prefixLength))
        break;
  }

  private String compact(String s) {
    return new StringBuilder()
            .append(startingEllipsis())
            .append(startingContext())
            .append(DELTA_START)
            .append(delta(s))
            .append(DELTA_END)
            .append(endingContext())
            .append(endingEllipsis())
            .toString();
  }

  private String startingEllipsis() {
    return prefixLength > contextLength ? ELLIPSIS : "";
  }

  private String startingContext() {
    int contextStart = Math.max(0, prefixLength - contextLength);
    int contextEnd = prefixLength;
    return expected.substring(contextStart, contextEnd);
  }

  private String delta(String s) {
    int deltaStart = prefixLength;
    int deltaEnd = s.length() - suffixLength;
    return s.substring(deltaStart, deltaEnd);
  }

  private String endingContext() {
    int contextStart = expected.length() - suffixLength;
    int contextEnd = Math.min(contextStart + contextLength, expected.length());
    return expected.substring(contextStart, contextEnd);
  }

  private String endingEllipsis() {
    return suffixLength > contextLength ? ELLIPSIS : "";
  }
}
```

코드를 리팩터링 하다 보면 원래 했던 변경을 되돌리는 경우가 흔하다   
리팩터링은 코드가 어느 수준에 이를 때까지 수많은 시행착오를 반복하는 작업이기 때문이다

<br />

## 기억에 남은 부분

> _코드를 처음보다 조금 더 깨끗하게 만드는 책임은 우리 모두에게 있다._

<br />

## 메모

```java
// 다른 문자열 앞 뒤로 몇 자를 보일 것인지
private int contextLength;
// 비교할 문자열 1
private String expected;
// 비교할 문자열 2
private String actual;
// 같은 앞부분 길이
private int prefixLength;
// 같은 뒷부분 길이
private int suffixLength;
```
