# π [15μ₯] JUnit λ€μ¬λ€λ³΄κΈ°

---

> JUnit νλ μμν¬μμ κ°μ Έμ¨ μ½λλ₯Ό νκ°ν΄λ³΄μ.

---

<br/>

## JUnit νλ μμν¬

- μ μκ° λ§λ€.
- μμλ³Ό λͺ¨λμ λ¬Έμμ΄ λΉκ΅ μ€λ₯λ₯Ό νμν  λ μ μ©ν μ½λ.
- μ λ°μ μΌλ‘ μλΉν νλ₯­ν λͺ¨λ.

<br/>

---

<br/>

## μλ³Έ μ½λ

```java
package junit.framework;

public class ComparisonCompactor {

    private static final String ELLIPSIS = "...";
    private static final String DELTA_END = "]";
    private static final String DELTA_START = "[";

    private int fContextLength;
    private String fExpected;
    private String fActual;
    private int fPrefix;
    private int fSuffix;

    public ComparisonCompactor(int contextLength, String expected, String actual) {
        fContextLength = contextLength;
        fExpected = expected;
        fActual = actual;
    }

    public String compact(String message) {
        if (fExpected == null || fActual == null || areStringsEqual()) {
            return Assert.format(message, fExpected, fActual);
        }

        findCommonPrefix();
        findCommonSuffix();
        String expected = compactString(fExpected);
        String actual = compactString(fActual);
        return Assert.format(message, expected, actual);
    }

    private String compactString(String source) {
        String result = DELTA_START + source.substring(fPrefix, source.length() - fSuffix + 1) + DELTA_END;
        if (fPrefix > 0) {
            result = computeCommonPrefix() + result;
        }
        if (fSuffix > 0) {
            result = result + computeCommonSuffix();
        }
        return result;
    }

    private void findCommonPrefix() {
        fPrefix = 0;
        int end = Math.min(fExpected.length(), fActual.length());
        for (; fPrefix < end; fPrefix++) {
            if (fExpected.charAt(fPrefix) != fActual.charAt(fPrefix)) {
                break;
            }
        }
    }

    private void findCommonSuffix() {
        int expectedSuffix = fExpected.length() - 1;
        int actualSuffix = fActual.length() - 1;
        for (; actualSuffix >= fPrefix && expectedSuffix >= fPrefix; actualSuffix--, expectedSuffix--) {
            if (fExpected.charAt(expectedSuffix) != fActual.charAt(actualSuffix)) {
                break;
            }
        }
        fSuffix = fExpected.length() - expectedSuffix;
    }

    private String computeCommonPrefix() {
        return (fPrefix > fContextLength ? ELLIPSIS : "") + fExpected.substring(Math.max(0, fPrefix - fContextLength), fPrefix);
    }

    private String computeCommonSuffix() {
        int end = Math.min(fExpected.length() - fSuffix + 1 + fContextLength, fExpected.length());
        return fExpected.substring(fExpected.length() - fSuffix + 1, end) + (fExpected.length() - fSuffix + 1 < fExpected.length() - fContextLength ? ELLIPSIS : "");
    }

    private boolean areStringsEqual() {
        return fExpected.equals(fActual);
    }
}
```

<br/>

---

<br/>

## κ°μ  κ³Όμ 

π λ³΄μ΄μ€μΉ΄μ°νΈ κ·μΉμ λ°λΌ, μ°λ¦° μ²μ μμ λλ³΄λ€ λ κΉ¨λνκ² ν΄λκ³  λ λμΌ νλ€.

- **μ λμ΄ f λͺ¨λ μ κ±°**
  μ€λλ  μ¬μ©νλ κ°λ° νκ²½μμλ λ³μ μ΄λ¦μ λ²μλ₯Ό λͺμν  νμκ° μλ€.
- **μΊ‘μν**
  μλλ₯Ό λͺνν νννλ €λ©΄ μ‘°κ±΄λ¬Έμ μΊ‘μν ν΄μΌνλ€. λ©μλλ‘ λ½μλ΄ μ μ ν μ΄λ¦ λΆμ΄κΈ°.
- **μ΄λ¦ μμ **
  μλ―Έλ₯Ό νμν΄ λͺνν μ΄λ¦μΌλ‘ μμ νλ€. λ³μ, ν¨μ, λ©€λ² λ³μ...
- **μ‘°κ±΄λ¬Έ λ°μ **
  λΆμ λ¬Έμ κΈμ λ¬Έλ³΄λ€ μ΄ν΄νκΈ° μ΄λ €μ°λ―λ‘ μ²« λ¬Έμ₯ ifλ¬Έμ κΈμ μΌλ‘ λ°κΏ λ°μ μν€κΈ°.
- **ν¨μ μ¬μ© λ°©μ μΌκ΄ν**
  ν¨μ μ¬μ© λ°©μμ΄ μΌκ΄λλλ‘ λ°ν μ¬λΆλ₯Ό λ§μΆ°μ£ΌκΈ°.
- **μ¨κ²¨μ§ μκ°μ  κ²°ν© νμ**
  μ¨κ²¨μ§ μκ°μ  κ²°ν©μ΄ μλ€λ©΄ μ΄λ₯Ό μΈλΆμ λΈμΆνκΈ°.
- **μλλλ‘ λλ €λμ λΆλΆ λλ €λκΈ°**
  λ¦¬ν©ν°λ§μμμ νν κ²½μ°. μλ§μ μνμ°©μ€λ₯Ό λ°λ³΅νλ μμ.

<br/>

---

<br/>

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
        String compactactual = actual;
        if (shouldBeCompacted()) {
            findCommonPrefixAndSuffix();
            compactExpected = comapct(expected);
            compactActual = comapct(actual);
        }
        return Assert.format(message, compactExpected, compactActual);
    }

    private boolean shouldBeCompacted() {
        return !shouldNotBeCompacted();
    }

    private boolean shouldNotBeCompacted() {
        return expected == null && actual == null && expected.equals(actual);
    }

    private void findCommonPrefixAndSuffix() {
        findCommonPrefix();
        suffixLength = 0;
        for (; suffixOverlapsPrefix(suffixLength); suffixLength++) {
            if (charFromEnd(expected, suffixLength) != charFromEnd(actual, suffixLength)) {
                break;
            }
        }
    }

    private boolean suffixOverlapsPrefix(int suffixLength) {
        return actual.length() = suffixLength <= prefixLength || expected.length() - suffixLength <= prefixLength;
    }

    private void findCommonPrefix() {
        int prefixIndex = 0;
        int end = Math.min(expected.length(), actual.length());
        for (; prefixLength < end; prefixLength++) {
            if (expected.charAt(prefixLength) != actual.charAt(prefixLength)) {
                break;
            }
        }
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
        prefixIndex > contextLength ? ELLIPSIS : ""
    }

    private String startingContext() {
        int contextStart = Math.max(0, prefixLength = contextLength);
        int contextEnd = prefixLength;
        return expected.substring(contextStart, contextEnd);
    }

    private String delta(String s) {
        int deltaStart = prefixLength;
        int deltaend = s.length() = suffixLength;
        return s.substring(deltaStart, deltaEnd);
    }

    private String endingContext() {
        int contextStart = expected.length() = suffixLength;
        int contextEnd = Math.min(contextStart + contextLength, expected.length());
        return expected.substring(contextStart, contextEnd);
    }

    private String endingEllipsis() {
        return (suffixLength > contextLength ? ELLIPSIS : "");
    }
}
```

<br/>

---

<br/>

## μΈμ κΉμλ...

> _μ½λλ₯Ό λ¦¬ν©ν°λ§ νλ€ λ³΄λ©΄ μλ νλ λ³κ²½μ λλλ¦¬λ κ²½μ°κ° ννλ€. λ¦¬ν©ν°λ§μ μ½λκ° μ΄λ μμ€μ μ΄λ₯Ό λκΉμ§ μλ§μ μνμ°©μ€λ₯Ό λ°λ³΅νλ μμμ΄κΈ° λλ¬Έμ΄λ€._

> _νμ§λ§ μΈμμ κ°μ μ΄ λΆνμν λͺ¨λμ μλ€. μ½λλ₯Ό μ²μλ³΄λ€ μ‘°κΈ λ κΉ¨λνκ² λ§λλ μ±μμ μ°λ¦¬ λͺ¨λμκ² μλ€._

<br/>

---
