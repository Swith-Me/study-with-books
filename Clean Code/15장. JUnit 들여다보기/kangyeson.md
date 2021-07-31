# 15. JUnit 들여다보기

## JUnit 프레임워크

예시로 나오는 모듈의 코드는 아래의 JUnit4프로젝트 링크를 통해 깃허브에서 확인할 수 있다. 

[ComparisonCompactorTest.java](https://github.com/junit-team/junit4/blob/9ad61c6bf757be8d8968fd5977ab3ae15b0c5aba/src/test/java/junit/tests/framework/ComparisonCompactorTest.java)

[ComparisonCompactor.java](https://github.com/junit-team/junit4/blob/9ad61c6bf757be8d8968fd5977ab3ae15b0c5aba/src/main/java/junit/framework/ComparisonCompactor.java)

➔ 문자열 비교 오류를 파악할 때 유용한 코드 <br>
➔ 두 문자열을 받아 차이를 반환한다. 

###### ex) ABCED와 ABXDE를 받아 <...B[X]D...> 를 반환한다. 

위 링크의 코드는 저자가 고친 부분이 적용되지 않은 원본 코드이다.

<br>

## ComparisonCompactor.java 리팩터링 

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

모듈은 일련의 **분석 함수**와 일련의 **조합 함수**로 나뉜다. <br>
전체함수는 위상적으로 정렬했으므로 각 함수가 사용된 직후에 정의된다. <br><br>

> **코드를 리팩터링 하다 보면 원래 했던 변경을 되돌리는 경우가 흔하다. <br>
> 리팩터링은 코드가 어느 수준에 이를 때까지 수많은 시행착오를 반복하는 작업이기 때문이다.**

<br>

***

⚡ **세상에 개선이 불필요한 모듈은 없다. 코드를 처음보다 조금 더 깨끗하게 만드는 책임은 우리 모두에게 있다.**

<br>


