# 16. SerialDate 리팩터링

 
예시로 나오는 클래스의 코드는 아래의 링크를 통해 깃허브에서 확인할 수 있으며, 저자가 수정하기 이전 버전이다.

[SerialDate 원본 코드](https://github.com/jfree/jcommon/blob/master/src/main/java/org/jfree/date/SerialDate.java)

- **SerialDate란?** 날짜를 표현하는 자바 클래스
- JCommon 라이브러리 ➔ org.jfree.date 패키지 ➔ SerialDate 클래스

<br>

## I. 돌려보자
- SerialDate 클래스는 MonthCodeToQuarter 메서드를 호출하지 않는다.
  - 단위 테스트가 실행되지 않은 코드가 클래스 여기저기에 흩어져 있다.
- stringToWeekdayCode 메서드의 존재 의의가 불확실하다.
- 'trues'와 'thurs'라는 약어를 지원해야 할지가 분명치 않다.
- getFollowingDayOfWeek 메서드에 버그가 발생한다.

<br><br>

## II. 고쳐보자
- SerialDate라는 이름은 너무 추상적 ➔ **DayDate**로 클래스 이름 변경
  - getYYY ➔ getYear
  - Month.make ➔ Month.fromInt
- 상수 클래스 MonthConstants는 enum으로 정의
```java
public abstract class DayDate implements Comparable, Serializable {
 public static enum Month {
  JANUARY(l), 
  FEBRUARY(2), 
  MARCH(3), 
  APRIL(4),
  MAY(5), 
  JUNE(6), 
  JULY(7) , 
  AUGUST(8),
  SEPTEMBERS(9),
  OCTOBER(10),
  NOVEMBER(11),
  DECEMBER (12); 
  
  Month(int index) { 
    this.index = index; 
  }
  ...
}
```

- 주석 추가 및 개선
- 부모 클래스는 자식 클래스를 몰라야 바람직. **추상팩토리 패턴** 적용 
- 일부 추상 메서드를 DayDate 클래스로 끌어올렸다.
- plusYears와 plusMonths에 중복 발생 ➔ correctLastDayOfMonth 메서드 생성해서 개선

<br>

> **결과 <br>
> DayDate 코드 커버리지가 84.9%로 감소!**

<br>

## III. 결론

1. 테스트 커버리지 증가
2. 버그 해결
3. 코드 크기 감소
4. 명확한 코드
5. 이해하기 쉬운 코드
   
<br>


