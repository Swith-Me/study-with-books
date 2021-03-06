## 16. SerialDate 리팩터링
> 오픈소스로 공개
> 누구나 사용하고 누구나 비판하라고 코드를 만천하에 공개
> 참으로 훌륭한 행동이다.

### 첫째, 돌려보자

- `SerialDateTests`라는 클래스는 단위 테스트 케이스 몇 개를 포함한다.
- 돌려보면 실패하는 테스트 케이스는 없지만 테스트 케이스를 훑어보면 모든 경우를 점검하지 않는다.

<br>

### 둘째, 고쳐보자

1. 변경 이력 없애기

2. import문 줄이기

   ex. `java.text.*`

3. Javadoc 주석은 HTML 태그 사용하기

4. 클래스 이름 변경하기

5. 상수 모음에서 `enum`으로 정의하기

6. 불필요한 주석 제거하기

7. 변수명 변경하기

8. 사용하는 곳이 한 클래스인 변수 해당 클래스로 옮기기

9. 메서드 이름을 더 서술적으로 바꾸기

10. 변경하다가 불필요한 변수, 메서드 제거하기

11. 실질적인 가치는 없으면서 코드만 복잡하게 만드는 `final` 키워드 제거하기


### 결론
- 우리는 보이스카우트 규칙을 따랐다.
- 체크아웃한 코드보다 좀 더 깨끗한 코드를 체크인하게 되었다.
- 테스트 커버리지가 증가했고, 버그 몇 개를 고쳤고, 코드 크기도 줄였고, 코드가 명확해졌다.
- 그래서 우리보다 코드를 더 쉽게 개선하리라.

### 느낀점
- 좀 더 현실적이고 경험담을 읽을 수 있어 이해하기 더욱 쉬웠다.
- 위와 같은 방법들을 평상시에 인지하고 코드를 작성한다면 코드를 작성할 때 더욱 수월할 것이다.
