# π 15μ₯. JUnit λ€μ¬λ€λ³΄κΈ°

<br />

## JUnit νλ μμν¬

```ComparisonCompactor```λ λ λ¬Έμμ΄μ λ°μ μ°¨μ΄λ₯Ό λ°ννλ€   
μλ₯Ό λ€μ΄, ```ABCDE```μ ```ABXDE```λ₯Ό λ°μ ```<...B[X]D...>```λ₯Ό λ°ννλ€

<br />

## ComparisonCompactor λͺ¨λ

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

μ λ°μ μΌλ‘ μλΉν νλ₯­ν λͺ¨λμ΄λ€

νμ§λ§ **λ³΄μ΄μ€μΉ΄μ°νΈ κ·μΉ**μ λ°λ₯΄λ©΄ μ°λ¦¬λ μ²μ μμ λλ³΄λ€ λ κΉ¨λνκ² ν΄λκ³  λ λμΌ νλ€

<br />

## κ±°μ¬λ¦¬λ λΆλΆ

### λ©€λ² λ³μ μμ λΆμΈ μ λμ΄ f   
```java
private int fContextLength;
private String fExpected;
private String fActual;
private int fPrefix;
private int fSuffix;
```
π½
```java
private int contextLength;
private String expected;
private String actual;
private int prefix;
private int suffix;
```

<br />

### compact ν¨μ μμλΆμ μΊ‘μνλμ§ μμ μ‘°κ±΄λ¬Έ   
```java
if (expected == null || actual == null || areStringsEqual())
```
π½
```java
if (shouldNotCompact())

private boolean shouldNotCompact() {
  return expected == null || actual == null || areStringsEqual()
}
```

<br />

### compact ν¨μμμ μ¬μ©νλ this.expectedμ this.actual   
```java
// μ΄λ¦μ λͺννκ² λΆμΈλ€
String compactExpected = compactString(expected);
String compactActual = compactString(actual);
```

<br />

### λΆμ λ¬Έμ κΈμ λ¬Έλ³΄λ€ μ΄ν΄νκΈ° μ½κ° λ μ΄λ ΅λ€   
```java
if (canBeCompacted())

private boolean canBeCompacted() {
  return expected != null && actual != null && !areStringsEqual()
}
```

<br />

### compact ν¨μ μ΄λ¦μ΄ μ΄μνλ€   
μ€μ λ‘ ```canBeCompacted```κ° ```false```μ΄λ©΄ μμΆνμ§ μλλ€   
```java
// μ΄λ¦ λ³κ²½
public String formatCompactedComparison(String message) {
```

<br />

### ν¨μ μ¬μ©λ°©μμ΄ μΌκ΄μ μ΄μ§ λͺ»νλ€   
λ°νκ°μ΄ μκΈ°λ μκΈ°λ νλ€   
```java
private void compactExpectedAndActual() {
  findCommonPrefix();
  findCommonSuffix();
  compactExpected = compactString(expected);
  compactActual = compactString(actual);
}
```
π½
```java
private void compactExpectedAndActual() {
  // λ©€λ² λ³μ μ΄λ¦ λ³κ²½λ ν¨κ» ν¨ (λ°λ‘ μλ λ΄μ©)
  prefixIndex = findCommonPrefix();
  suffixIndex = findCommonSuffix();
  compactExpected = compactString(expected);
  compactActual = compactString(actual);
}
```

<br />

### λ©€λ² λ³μ μ΄λ¦λ λͺ¨νΈνλ€   
```java
private int prefix;
private int suffix;
```
π½
```java
private int prefixIndex;
private int suffixIndex;
```

<br />

### μ¨κ²¨μ§ μκ°μ μΈ κ²°ν©```hidden temporal coupling```μ΄ μ‘΄μ¬νλ€   
```findCommonSuffix```λ ```findCommonPrefix```κ° ```prefixIndex```λ₯Ό κ³μ°νλ€λ μ¬μ€μ μμ‘΄νλ€   
```java
prefixIndex = findCommonPrefix();
suffixIndex = findCommonSuffix();
```
π½
```java
findCommonPrefixAndSuffix();

private void findCommonPrefixAndSuffix() {
  findCommonPrefix();
  // ... findCommonSuffix λ΄μ©
}
```

<br />

### κ³ μΉκ³  λλ, ```suffixIndex```μ μ΄λ¦μ΄ μ μ νμ§ λͺ»νλ€  
```prefixIndex```λ λ§μ°¬κ°μ§λ‘, μ΄ κ²½μ° ```index```μ ```length```κ° λμμ΄λ€    
```java
private int prefixIndex;
private int suffixIndex;
```
π½
```java
private int prefixLength;
private int suffixLength;
```

<br />

### ```compactString```μ μλ if λ¬Έ λ λ€ νμμμ΄ λ³΄μΈλ€   
```java
if (fPrefix > 0)
  result = computeCommonPrefix() + result;
if (fSuffix > 0)
  result = result + computeCommonSuffix();
```
π½
```java
return
  computeCommonPrefix() +
  DELTA_START +
  source.substring(prefixLength, source.length() - suffixLength()) +
  DELTA_END +
  computeCommonSuffix();
```

<br />

## μ΅μ’ μ½λ

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

μ½λλ₯Ό λ¦¬ν©ν°λ§ νλ€ λ³΄λ©΄ μλ νλ λ³κ²½μ λλλ¦¬λ κ²½μ°κ° ννλ€   
λ¦¬ν©ν°λ§μ μ½λκ° μ΄λ μμ€μ μ΄λ₯Ό λκΉμ§ μλ§μ μνμ°©μ€λ₯Ό λ°λ³΅νλ μμμ΄κΈ° λλ¬Έμ΄λ€

<br />

## κΈ°μ΅μ λ¨μ λΆλΆ

> _μ½λλ₯Ό μ²μλ³΄λ€ μ‘°κΈ λ κΉ¨λνκ² λ§λλ μ±μμ μ°λ¦¬ λͺ¨λμκ² μλ€._

<br />

## λ©λͺ¨

```java
// λ€λ₯Έ λ¬Έμμ΄ μ λ€λ‘ λͺ μλ₯Ό λ³΄μΌ κ²μΈμ§
private int contextLength;
// λΉκ΅ν  λ¬Έμμ΄ 1
private String expected;
// λΉκ΅ν  λ¬Έμμ΄ 2
private String actual;
// κ°μ μλΆλΆ κΈΈμ΄
private int prefixLength;
// κ°μ λ·λΆλΆ κΈΈμ΄
private int suffixLength;
```
