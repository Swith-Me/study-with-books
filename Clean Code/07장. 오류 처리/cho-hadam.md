# 📙 07장. 오류 처리

오류 처리 코드로 인해 프로그램 논리를 이해하기 어려워진다면 깨끗한 코드라 부르기 어렵다

<br />

## 오류 코드보다 예외를 사용하라

오류 코드를 반환하는 방법은 함수를 호출하는 즉시 오류를 확인해야 하기 때문에 호출자 코드가 복잡해진다   
따라서 오류가 발생하면 **예외**를 던지는 편이 낫다

<br />

## Try-Catch-Finally 문부터 작성하라

예외가 발생한 코드를 짤 때는 try-catch-finally 문으로 시작하는 편이 낫다

먼저 강제로 예외를 일으키는 테스트 케이스를 작성한 후 테스트를 통과하게 코드를 작성하는 방법을 권장한다   
그러면 자연스럽게 try 블록의 트랜잭션 범위부터 구현하게 되므로 범위 내에서 트랜잭션 본직을 유지하기 쉬워진다

<br />

## 미확인 예외를 사용하라

확인된 예외는 새로운 오류를 던질 때 최하위 단계에서 최상위 단계까지 연쇄적인 수정이 일어난다

> 최하위 함수를 변경해 새로운 오류를 던진다고 가정하자   
> 확인된 오류를 던진다면 함수는 선언부에 throws 절을 추가해야 한다   
> 변경한 함수를 호출하는 모두가   
> 1) catch 블록에서 새로운 예외를 처리하거나   
> 2) 선언부에 throw 절을 추가해야 한다는 말이다

<br />

## 예외에 의미를 제공하라

예외를 던질 때는 전후 상황을 충분히 덧붙인다

<br />

## 호출자를 고려해 예외 클래스를 정의하라

**외부 API를 감싸면**
1. 외부 라이브러리와 프로그램 사이에서 의존성이 크게 줄어든다
2. 나중에 다른 라이브러리로 갈아타도 비용이 적다
3. 프로그램을 테스트하기도 쉬워진다
4. 특정 업체가 API를 설계한 방식에 발목 잡히지 않는다

<br />

## 정상 흐름을 정의하라

다음은 비용 청구 애플리케이션에서 총계를 계산하는 코드다

```java
try {
  MealExpenses expenses = expenseReportDAO.getMeals(employee.getID());
  // 비용을 청구했다면 청구한 식비를 총계에 더한다
  m_total += expenses.getTotal();
} catch(MealExpensesNotFound e) {
  // 비용을 청구하지 않았다면 일일 기본 식비를 총계에 더한다
  m_total += getMealPerDiem();
}
```

예외가 논리를 따라가기 어렵게 만든다

아래의 코드처럼 간결하게 만드는 것이 가능하다
```java
MealExpenses expenses = expenseReportDAO.getMeals(employee.getID());
m_total += expenses.getTotal();
```

### 특수 사례 패턴

클래스를 만들거나 객체를 조작해 특수 사례를 처리하는 방식이다

```java
public class PerDiemMealExpenses implements MealExpenses {
  public int getTotal() {
    // 기본값으로 일일 기본 식비를 반환한다
  }
}
```

클라이언트 코드가 예외적인 상황을 처리할 필요가 없어진다

<br />

## null을 반환하지 마라

null을 반환하는 코드는 일거리를 늘릴 뿐만 아니라 호출자에게 문제를 떠넘긴다   
null을 반환하는 대신 예외를 던지거나 특수 사례 객체를 반환한다

```java
List<Employee> employees = getEmployees();
if (employees != null) {
  for(Employee e : employees) {
    totalPay += e.getPay();
  }
}
```

위 코드에서 ```getEmployees```를 변경해 빈 리스트를 반환한다면 코드가 훨씬 깔끔해진다

```java
List<Employee> employees = getEmployees();
for(Employee e : employees) {
  totalPay += e.getPay();
}
```

```java
public List<Employee> getEmployees() {
  if (직원이 없다면)
    return Collections.emptyList();
}
```

<br />

## null을 전달하지 마라

메서드 인수로 null을 전달하지 마라   

대다수 프로그래밍 언어는 호출자가 실수로 넘기는 null을 적절히 처리하는 방법이 없다   
그렇다면 애초에 null을 넘기지 못하도록 금지하는 정책이 합리적이다

<br />

## 몰랐던 것

**retrieve** 회수하다, 검색하다

<br />

## 기억에 남은 부분

> _깨끗한 코드는 읽기도 좋아야 하지만 안정성도 높아야 한다. **이 둘은 상충하는 목표가 아니다.**_
