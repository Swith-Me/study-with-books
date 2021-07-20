# 07. 오류 처리

- 뭔가 잘못될 가능성은 늘 존재하고, 잘못을 바로 잡을 책임은 프로그래머에게 있다.
- 흩어진 오류 처리 코드 ➔ 실제 코드 파악 불가능
- 깨끗한 코드와 오류 처리는 연관되어 있다.

> **목표**✔ <br>
> 깨끗하고 튼튼한 코드, 우아하고 고상하게 오류를 처리하는 기법과 고려사항을 익히는 것
<br>

## I. 오류 코드보다 예외를 사용하라
- 오류 태그와 오류 코드 ❌
  - 복잡해지는 호출자 코드

- 예외 사용 ✔
  - 기능을 구현하는 알고리즘과 오류를 처리하는 알고리즘 분리
  - 독립적인 각 개념
  - 코드 품질 개선

<br><br>

## II. Try-Catch-Finally 문부터 작성하라
> 예외가 발생할 코드를 짤 때 Try-Catch-Finally 문으로 시작하는 편이 낫다.

- 예외에서 프로그램 안에다 **범위를 정의** <br>
- try 블록에서 무슨 일이 생기든 catch 블록은 프로그램 상태를 일관성 있게 유지 <br>
- 호출자가 기대하는 상태 정의가 쉬워짐 <br><br>

```java
public List<RecordGrip> retrieveSection(String sectionName) {
  try {
    FileInputStream stream = new FileInputStream(sectionName);
    stream.close();
  } catch (FileNotFoundException e) {
    throw new StorageException("retrieval error", e);
  }
  return new ArrayList<RecordeGrip>();
}
```
1. 코드가 예외를 던지므로 단위 테스트 성공 (**리펙토링** 가능) <br><br>
2. catch 블록에서 **예외 유형 축소** <br>
➔ FileInputStream 생성자가 던진 FileNotFoundException을 잡아낸다. <br><br>
3. try-catch로 범위 정의 완료 <br><br>
4. **TDD**를 사용하여 필요한 나머지 논리 추가 <br>
➔ stream.close()문 삽입 <br>
➔ 강제로 예외를 일으키는 테스트 케이스 작성 후, 테스트 통과 코드 작성 <br><br>

###### ❓ 리펙토링 | 외부 동작을 바꾸지 않으면서 내부 구조를 개선하는 방법
###### ❓ TDD | Test Driven Development(테스트 주도 개발), 테스트를 먼저 만들고 테스트를 통과하기 위한 것을 짜는 것

<br><br>

## III. 미확인 예외를 사용하라
```
확인된 예외란❓
메서드 선언 단계에서 메서드가 반환할 예외를 모두 열거하는 것
```
확인된 예외는 안정적인 소프트웨어를 제작하는 요소로 필요하지 않다
   - **OCP**를 위반하기 때문
     - 즉 하위 단계의 오류를 해결하기 위해 최상위 단계 선언부까지 수정해야 한다
     - 캡슐화가 깨진다

  - 애플리케이션은 일반적으로 **의존성** > 확인된 예외
    - 아주 중요한 라이브러리일 경우엔, 확인된 예외도 유용  <br><br> 

###### ❓ OCP | Open Close Principle, 기존의 코드를 변경하지 않으면서 기능을 추가할 수 있도록 설계되는 것

<br><br>

## IV. 예외에 의미를 제공하라
> 예외를 던질 때는 **정보, 전후 상황**을 충분히 덧붙인다.

➔ 오류가 발생한 원인과 위치를 찾기가 쉬워진다.

<br><br>

## V. 호출자를 고려해 예외 클래스를 정의하라
오류를 정의, 분류할 때 가장 중요한 것은 **오류를 잡아내는 방법**이다
예외에 대응하는 방식은 <br>
1. 오류를 기록한다
2. 프로그램을 계속 수행해도 좋은지 확인한다

가 대부분인데, 이때 **외부 API 감싸기 기법**을 사용하라

#### 외부 API 감싸기 기법의 장점

- 외부 라이브러리와 프로그램 사이의 의존성이 줄어든다
- 라이브러리 변경이 간편하다
- 프로그램 테스트에 용이하다
- 설계 방식에 상관없이 편리하게 API를 사용할 수 있다

<br>

## VI. 정상 흐름을 정의하라
```java
// 비용 청구 시스템의, 총계를 계산하는 코드
// 👎 예외가 논리를 따라가기 어렵게 만든다
try {
  MealExpenses expenses = expenseReportDAO.getMeals(employee.getID());
  m_total += expenses.getTotal();
} catch(MealExpensesNotFound e) {
  m_total += getMealPerDiem();
}
```

➔ **특수 상황을 처리할 필요가 없도록 코드를 간결해지게 하라**

```java
MealExpenses expenses = expenseReportDAO.getMeals(employee.getID());
m_total += expenses.getTotal();
```

### 특수 사례 패턴
> 클래스를 만들거나 객체를 조작해 특수 사례를 처리하는 방식

➔ 예외적인 상황을 캡슐화해서 처리 <br>
➔ 클라이언트 코드가 예외적인 상황을 처리할 필요가 없어진다 
```java
public class PerDiemMealExpenses implements MealExpenses {
  public int getTotal() {
    // 기본값으로 일일 기본 식비를 반환한다
  }
}
```

<br><br>

## VII. null을 반환하지 마라
```java
// 나쁜 코드👎 
public void registerItem(Item item) {
  if (item != null) {
    ItemRegistry registry = persistentStore.getItemRegistry();
    if (registry != null) {
      ...
    }
  }
}
```
위와 같은 null 반환은
- 일거리를 늘린다
- 호출자에게 문제를 떠넘긴다
- 한 번이라도 null 확인을 빼먹으면 오류(NullPointerException)가 발생한다 <br><br>

🛠 **개선 방안** <br>
 - 외부 API로 null을 반환하는 감싸기 메서드 사용 <br>
 -  특수 사례 객체 반환 <br>
 - 꼭 필요하지 않다면 null 반환은 생략
 
<br>

## VIII. null을 전달하지 마라
> 대다수 프로그래밍 언어는 호출자가 실수로 넘기는 null을 적절히 처리하는 방법이 없다.

정상적인 인수로 null을 기대하는 API가 아니라면 메서드로 null을 전달하는 코드는 최대한 피한다 <br>

- 새로운 예외 유형 생성 후 던지기
- assert 문 사용 (InvalidArgumentException 예외 처리) <br><br><br>

## IX. 결론
> **오류 처리를 프로그램 논리와 독자적인 사안으로 고려하자.** <br>
> 이 둘을 분리하면 독립적인 추론이 가능해지면서 유지보수성이 높은, 튼튼하고 깨끗한 코드를 작성할 수 있다.
   
<br>

***

⚡ **깨끗한 코드는 읽기도 좋아야 하지만 안전성도 높아야 한다.**

<br>




