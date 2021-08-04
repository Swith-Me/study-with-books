✨ 제 15장, JUnit 들여다보기 
----------------------

### JUnit 프레임워크
> * 다음 모듈은 문자열 비교 오류 파악 시 유용한 코드 
> * 보이스카우트 규칙 
>   - 멤버 변수 앞에 붙인 접두어 제거 
>   - 의도의 명확한 표현을 위해 캡슐화 
>   - 변수 이름을 명확하게 변경 
>   - 이해가 쉬운 긍정문으로 조건문 반전
>   - 함수 사용 방식을 일관적으로 
>   - 숨겨진 시각적 결함을 파악하면 외부로 노출
>   - 리팩터링을 하며, 원래 했던 변경을 돌리기도 함 

<br/>

최종코드
~~~java
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
~~~

