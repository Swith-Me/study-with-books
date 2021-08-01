## 16. SerialDate 리팩터링

### 들어가면서

- 이 장에서는 https://www.jfree.org/jcommon/api/index.html 링크에서 `org.jfree.date` 라는 패키지 안에 있는 `SerialDate` 클래스를 탐험한다.
- `SerialDate`는 날짜를 표현하는 자바 클래스다.

<br>

### 첫째, 돌려보자

- `SerialDateTests`라는 클래스는 단위 테스트 케이스 몇 개를 포함한다.
- 돌려보면 실패하는 테스트 케이스는 없지만 테스트 케이스를 훑어보면 모든 경우를 점검하지 않는다.

<br>

### 둘째, 고쳐보자

1. 변경 이력 없애기

   - 이제는 소스 코드 제어 도구를 사용하므로 변경 이력은 없애도 되겠다.

   ```java
   /*
   * Changes (from 11-Oct-2001)
    * --------------------------
    11-Oct-2001 : Re-organised the class and moved it to new package com.jrefinery.date (DG);
    05-Nov-2001 : Added a getDescription() method, and eliminated NotableDate
    */
   ```

2. import문 줄이기

   ex. `java.text.*`

3. Javadoc 주석은 HTML 태그 사용하기

   ```java
   /*
   // Before
   <p>
   Requirement 1 : match at least what Excel does for dates;
   Requirement 2 : the date represented by the class is immutable;
   </p>

   // After
   <pre>
   Requirement 1 : match at least what Excel does for dates;
   Requirement 2 : the date represented by the class is immutable;
   </pre>
   ```

4. 클래스 이름 변경하기

   - 첫째, '일련번호'(Serial) 라는 용어는 날짜보다 제품 식별 번호에 더 적합하다.

   - 둘째, `SerialDate`라는 이름은 구현을 암시하는데 실제로는 추상 클래스다.

   - 셋째, 하지만 `Date`, `Day`라는 이름들은 너무 많이 쓰이고 있다.

   - 결론: `DayDate`로 변경하자.

5. 상수 모음에서 `enum`으로 정의하기

   ```java
   // Before
   public static final int JANUARY = 1;
   public static final int FEBRUARY = 2;
   public static final int MARCH = 3;

   // After
   public static enum Month {
       JANUARY(1),
       FEBRUARY(2),
       MARCH(3)
       ...
   }
   ```

6. 불필요한 주석 제거하기

7. 변수명 변경하기

   ```java
   // DayDate 클래스가 표현할 수 있는 최초 날짜와 최후 날짜
   // Before
   /** The serial number for 1 January 1900. */
   public static final int SERIAL_LOWER_BOUND = 2;
   /** The serial number for 31 December 9999. */
   public static final int SERIAL_UPPER_BOUND = 2958465;

   // After
   public static final int EARLIEST_DATE_ORDINAL = 2; // 1/1/1900
   public static final int LATEST_DATE_ORDINAL = 2958465; // 12/31/9999
   ```

8. 사용하는 곳이 한 클래스인 변수 해당 클래스로 옮기기

9. 메서드 이름을 더 서술적으로 바꾸기

10. 변경하다가 불필요한 변수, 메서드 제거하기

11. 실질적인 가치는 없으면서 코드만 복잡하게 만드는 `final` 키워드 제거하기

12. 임시 변수를 사용하여 알고리즘을 명확히 고치기

<br>

### 결론

- 우리는 보이스카우트 규칙을 따랐다.
- 체크아웃한 코드보다 좀 더 깨끗한 코드를 체크인하게 되었다.
- 테스트 커버리지가 증가했고, 버그 몇 개를 고쳤고, 코드 크기도 줄였고, 코드가 명확해졌다.
- 다음 사람은 우리보다 코드를 좀 더 쉽게 이해하리라.
- 그래서 우리보다 코드를 더 쉽게 개선하리라.

<br>

### 느낀점

- 우선 코드를 이해하는데 시간이 오래 걸렸을 뿐더러, 아직 이 클래스의 모든 코드를 이해하진 못했다.
- 결론에 나온 것처럼 실제로 코드 리팩터링이 다음 사람이 내 코드를 좀 더 쉽게 개선할 수 있도록 한다는 것을 몸소 깨닫게 되었다.
- 나도 그 다음 사람을 위해 더 클린한 코드를 짜야겠다.
