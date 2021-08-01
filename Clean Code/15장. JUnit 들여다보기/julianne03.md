## 15. JUnit 들여다보기

### 들어가면서

- `JUnit`은 자바 프레임워크 중에서 가장 유명하다.
- 이 장에서는 `JUnit` 프레임워크에서 가져온 코드를 평가한다.

### JUnit 프레임워크

- 우리가 살펴볼 모듈은 문자열 비교 오류를 파악할 때 유용한 코드다.
- `ComparisonCompactor`는 두 문자열을 받아 차이를 반환한다.
- ex. `ABCDE`와 `ABXDE`를 받아 `B[X]D`를 반환한다.
- etc. 모듈의 코드가 너무 길어 생략한 관계로 본문의 코드를 참고하자.

### 예제 코드 리팩토링

1. 접두어 제거

```java
// Before
private int fContextLength;
private String fExpected;
private String fActual;
private int fPrefix;
private int fSuffix;

// After
private int contextLength;
private String expected;
private String actual;
private int prefix;
private int suffix;
```

---

2. 조건문 캡슐화

```java
// Before
public String compact(String message) {
    if (expected == null || actual == null || areStringsEqual())
        return Assert.format(message, expected, actual);

    findCommonPrefix();
    findCommonSuffix();
    String expected = compactString(this.expected);
    String actual = compactString(this.actual);
    return Assert.format(message, expected, actual);
}

// After
public String compact(String message) {
    if (shouldNotCompact())
        return Assert.format(message, expected, actual);

    findCommonPrefix();
    findCommonSuffix();
    String expected = compactString(this.expected);
    String actual = compactString(this.actual);
    return Assert.format(message, expected, actual);
}

private boolean shouldNotCompact() {
    return expected == null || actual == null || areStringsEqual();
}
```

---

3. 변수의 이름을 명확하게 변경

```java
// Before
String expected = compactString(this.expected);
String actual = compactString(this.actual);

// After
String compactExpected = compactString(this.expected);
String compactActual = compactString(this.actual);
```

---

4. `if` 조건문을 긍정문으로 변경

```java
// Before
public String compact(String message) {
    if (shouldNotCompact())
    ...
}

private boolean shouldNotCompact() {
    ...
}

// After
public String compact(String message) {
    if (canBeCompacted())
    ...
}

private boolean canBeCompacted() {
    ...
}
```

---

5. 함수의 이름을 명확하게 변경

```java
// Before - 문자열을 압축하는 함수인데 실제로 canBeCompacted가 false이면 압축하지 않는다.
public String compact(String message) {

// After
public String formatCompactedComparison(String message) {
```

---

6. 함수 분리

```java
...
String compactExpected;
String compactActual;

...
// 형식을 맞추는 작업 - formatCompactedComparison
public String formatCompactedComparison(String message) {
    if (canBeCompacted()) {
        compactExpectedAndActual();
        return Assert.format(message, compactExpected, compactActual);
    } else {
        return Assert.format(message, expected, actual);
    }
}

// 압축만 수행하는 작업 - compactExpectedAndActual
private void compactExpectedAndActual() {
    findCommonPrefix();
    findCommonSuffix();
    compactExpected = compactString(this.expected);
    compactActual = compactString(this.actual);
}
```

7. 함수 사용방식을 일관적이도록 변경

```java
private void compactExpectedAndActual() {
    findCommonPrefixAndSuffix();
    compactExpected = compactString(this.expected);
    compactActual = compactString(this.actual);
}

private void findCommonPrefixAndSuffix() {
    findCommonPrefix();
    int suffixLength = 1;

    for(; !suffixOverlapsPrefix(suffixLength); suffixLength++) {
        if (charFromEnd(expected, suffixLength) != charFromEnd(actual, suffixLength))
            break;
    }
    suffixIndex = suffixLength;
}

private char charFromEnd(String s, int i) {
    return s.charAt(s.length() - i);
}

private boolean suffixOverlapsPrefix(int suffixLength) {
    return actual.length() - suffixLength < prefixLength || expected.length() - suffixLength < prefixLength;
}
```

<br>

### 최종 코드

- 모듈은 일련의 분석 함수와 일련의 조합 함수로 나뉜다.
- 전체 함수는 위상적으로 정렬했으므로 각 함수가 사용된 직후에 정의된다.
- 분석 함수가 먼저 나오고 조합 함수가 그 뒤를 이어서 나온다.
- 코드를 리팩터링 하다 보면 원래 했던 변경을 되돌리는 경우가 흔하다.
- 리팩터링은 코드가 어느 수준에 이를 때까지 수많은 시행착오를 반복하는 작업이기 때문이다.

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

<br>

### 결론

- 세상에 개선이 불필요한 모듈은 없다.
- 코드를 처음보다 조금 더 깨끗하게 만드는 책임은 우리 모두에게 있다.

<br>

### 느낀점

- 결론에 나와있던 말이 너무 인상 깊었다.
- 내가 쓴 코드를 리팩토링 한 적은 아직까진 없는데 앞으로는 무조건 해야겠다고 다짐했다.
- p.s 조금씩 고쳐가는 코드를 보면서 희열을 느낄 수 있을 것 같다..ㅎ
