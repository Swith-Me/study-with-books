✨ 제 16장, SerialDate 리팩터링
----------------------

### 첫째, 돌려보자 
> * 돌려보면 실패하는 테스트 케이스는 X
>   - 테스트 케이스를 훑어보면 모든 경우를 점검하지 않는다는 사실이 드러남 

<br/>

### 둘째, 고쳐보자
> * 고친 작업 정리
>   - 처음에 나오는 주석이 너무 오래되어 간단하게 고치고 개선 
>   - enum을 모두 독자적인 소스파일로 이동
>   - 정적 변수(dayFormatSymbols)와 정적 메서드(getMonthNames, isLeapYear, lastDayOfMonth)를 새 클래스(DateUtil)로 이동 
>   - 일부 추상 메서드를 DayDate 클래스로 끌어올림
>   - Month.make를 Month.fromInt로 변경 
>     - enum도 똑같이 변경 (enum에 tInt() 접근자 생성, index 필드 private로 정의)
>   - plusYears와 plusMonths 사이의 중복을 새 메서드(correctLastDayOfMonth)를 생성해 제거
>     - 새 메서드가 좀 더 명확해지는 효과
>   - 팔방미인으로 사용하던 숫자 1을 없앰 

> * 수정 결과 ; DayDate 코드 커버리지가 84.9%로 감소 
