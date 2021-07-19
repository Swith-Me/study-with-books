**오류처리**는 입력이 이상하거나 디바이스가 실패할지도 모르기 때문에 프로그램에 반드시 **필요한 요소 중 하나**이다.

깨끗한 코드와 오류 처리는 확실히 **연관성**이 있고, 상당수 코드 기반은 전적으로 오류 처리 코드에 *좌우된다.

*좌우된다 : 여기저기 흩어진 오류 처리 코드 때문에 실제 코드가 하는 일 파악하기 거의 불가능한 것

## 오류 코드보다 예외를 사용하라

- 예전에는 예외를 지원하지 않는 언어가 많았다.
- 오류 처리하고 보고하는 방법인 오류 플래그를 설정하거나 호출자에게 오류 코드를 반환하는 것으로 제한적이었다.
- 이렇게 되면 함수를 호출한 즉시 오류를 확인해야 되고, 잊어버리기 쉽다.
- 그래서 오류를 발생하면 예외를 던지면 호출자 코드가 논리와 오류 처리 코드가 뒤섞이지 않아 더 깔끔해진다.

목록 7-1 DeviceController.java

```java
// Bad
public class DeviceController {
  ...
  public void sendShutDown() {
    DeviceHandle handle = getHandle(DEV1);
    // 디바이스 상태를 점검한다.
    if (handle != DeviceHandle.INVALID) {
      // 레코드 필드에 디바이스 상태를 점검한다.
      retrieveDeviceRecord(handle);
      // 디바이스가 일시정지 상태가 아니라면 종료한다.
      if (record.getStatus() != DEVICE_SUSPENDED) {
        pauseDevice(handle);
        clearDeviceWorkQueue(handle);
        closeDevice(handle);
      } else {
        logger.log("Device suspended. Unable to shut down");
      }
    } else {
      logger.log("Invalid handle for: " + DEV1.toString());
    }
  }
  ...
}
```

목록 7-2 [DeviceController.java](http://devicecontroller.java) (예외 사용)

```java
// Good
public class DeviceController {
  ...
  public void sendShutDown() {
    try {
      tryToShutDown();
    } catch (DeviceShutDownError e) {
      logger.log(e);
    }
  }
    
  private void tryToShutDown() throws DeviceShutDownError {
    DeviceHandle handle = getHandle(DEV1);
    DeviceRecord record = retrieveDeviceRecord(handle);
    pauseDevice(handle); 
    clearDeviceWorkQueue(handle); 
    closeDevice(handle);
  }
  
  private DeviceHandle getHandle(DeviceID id) {
    ...
    throw new DeviceShutDownError("Invalid handle for: " + id.toString());
    ...
  }
  ...
}
```

디바이스를 종료하는 알고리즘과 오류를 처리하는 알고리즘을 분리해서 코드가 확실히 깨끗해졌고, 품질도 좋아졌다.

## Try-Catch-Finally 문 부터 작성하라

- `try` 블록은 트랜잭션과 비슷하다.
- `try` 블록에서 무슨 일이 생기든지 `catch` 블록은 프로그램 상태를 일관성 있게 유지해야 한다.
- 그러므로 예외가 발생할 코드를 짤 때는 `try-catch-finally` 문으로 시작하는 편이 낫다.

    ⇒ `try` 블록에서 무슨 일이 생기든지 호출자가 기대하는 상태를 정의하기 쉬워진다.

```java
// Step 1: StorageException을 던지지 않으므로 이 테스트는 실패한다.
  
  @Test(expected = StorageException.class)
  public void retrieveSectionShouldThrowOnInvalidFileName() {
    sectionStore.retrieveSection("invalid - file");
  }
  
  public List<RecordedGrip> retrieveSection(String sectionName) {
    // 실제로 구현할 때까지 비어 있는 더미를 반환한다.
    return new ArrayList<RecordedGrip>();
  }
```

```java
// Step 2: 이제 테스트는 통과한다.
  public List<RecordedGrip> retrieveSection(String sectionName) {
    try {
      FileInputStream stream = new FileInputStream(sectionName)
    } catch (Exception e) {
      throw new StorageException("retrieval error", e);
    }
  return new ArrayList<RecordedGrip>();
}
```

```java
// Step 3: 예외 범위를 FileNotFoundException으로 줄여 정확히 어떤 Exception이 발생했는지 잡아낼 수 있다.
  public List<RecordedGrip> retrieveSection(String sectionName) {
    try {
      FileInputStream stream = new FileInputStream(sectionName);
      stream.close();
    } catch (FileNotFoundException e) {
      throw new StorageException("retrieval error", e);
    }
    return new ArrayList<RecordedGrip>();
  }
```

✔ 강제로 예외를 일으키는 테스트 케이스를 작성한 후 테스트를 통과하게 코드를 작성하는 방법을 사용하면, 자연스럽게 `try` 블록의 트랜잭션 범위부터 구현하게 되므로 범위 내에서 트랜잭션 본질을 유지하기 쉬워진다.

## 미확인(unchecked) 예외를 사용하라

- 확인된 예외는 몇 가지 장점을 제공하지만, 지금은 안정적인 소프트웨어를 제작하는 요소로 필요하지 않다는 사실이 분명해졌다.
- `C#`, `C++`, `Python`, `Ruby`는 확인된 예외를 지원하지 않지만, 안정적인 소프트웨어를 구현하기에 무리가 없다.
- 확인된 예외는 **OCP(Open Closed Principle)**를 위반한다.
- 확인된 예외는 최하위 단계에서 최상위 단계까지 **연쇄적인 수정**이 일어나 캡슐화를 깨버린다.
- 때로는 확인된 예외도 아주 중요한 라이브러리에서 작성할 때 모든 예외를 잡아야 될 때 유용하다.

## 예외에 의미를 제공하라

- 예외를 던질 때는 전후 상황을 충분히 덧붙인다.
- 그러면 오류가 발생한 원인과 위치를 찾기가 쉬워진다.
- 자바는 모든 예외에 호출 스택을 제공한다.
- 하지만, 실패한 코드의 의도를 파악하려면 호출 스택만으로 부족하다.
- 오류 메세지에 실패한 연산 이름, 실패 유형과 예외와 함께 정보를 담아 던진다.
- 애플리케이션이 로깅 기능을 사용한다면 `catch` 블록에서 오류를 기록하도록 충분한 정보를 넘겨준다.

## 호출자를 고려해 예외 클래스를 정의하라

- 오류를 분류하는 방법은 수없이 많다.
- 발생한 위치/컴포넌트/유형으로 분류가 가능하다.
    - ex- 디바이스/네트워크 실패, 프로그래밍 오류

✔️ 하지만, 애플리케이션에서 오류를 정의할 때 프로그래머에게 가장 중요한 관심사는 **오류를 잡아내는 방법**이 되어야 한다.

```java
// Bad
  // catch문의 내용이 거의 같다.
  
  ACMEPort port = new ACMEPort(12);
  try {
    port.open();
  } catch (DeviceResponseException e) {
    reportPortError(e);
    logger.log("Device response exception", e);
  } catch (ATM1212UnlockedException e) {
    reportPortError(e);
    logger.log("Unlock exception", e);
  } catch (GMXError e) {
    reportPortError(e);
    logger.log("Device response exception");
  } finally {
    ...
  }
```

```java
// Good
  // ACME 클래스를 LocalPort 클래스로 래핑해 new ACMEPort().open() 메소드에서 던질 수 있는 exception들을 간략화
  
  LocalPort port = new LocalPort(12);
  try {
    port.open();
  } catch (PortDeviceFailure e) {
    reportError(e);
    logger.log(e.getMessage(), e);
  } finally {
    ...
  }
  
  public class LocalPort {
    private ACMEPort innerPort;
    public LocalPort(int portNumber) {
      innerPort = new ACMEPort(portNumber);
    }
    
    public void open() {
      try {
        innerPort.open();
      } catch (DeviceResponseException e) {
        throw new PortDeviceFailure(e);
      } catch (ATM1212UnlockedException e) {
        throw new PortDeviceFailure(e);
      } catch (GMXError e) {
        throw new PortDeviceFailure(e);
      }
    }
    ...
  }
```

## 정상 흐름을 정의하라

- 앞 절에서 충고한 지침을 따른다면 비즈니스 논리와 오류 처리가 잘 분리된 코드가 나온다.
- 그러면 코드 대부분이 깨끗하고 간결한 알고리즘으로 보이기 시작한다.
- 하지만, 그러다 보면 오류 감지가 프로그램 언저리로 밀려난다.
- 외부 API를 감싸 독자적인 예외를 던지고, 코드 위에 처리기를 정의해 중단된 계산을 처리한다.
- 대개는 멋진 처리 방식이지만, 때로는 중단이 적합하지 않은 때도 있다.

```java
// Bad

  try {
    MealExpenses expenses = expenseReportDAO.getMeals(employee.getID());
    m_total += expenses.getTotal();
  } catch(MealExpensesNotFound e) {
    m_total += getMealPerDiem();
  }
```

```java
// Good
  MealExpenses expenses = expenseReportDAO.getMeals(employee.getID());
  m_total += expenses.getTotal();
  
  public class PerDiemMealExpenses implements MealExpenses {
    public int getTotal() {
      // 기본값으로 일일 기본 식비를 반환한다.
    }
  }
```

위와 같은 코드를 ***특수 사례 패턴**이라 부른다.

***특수 사례 패턴(Special case pattern)** : 클래스를 만들거나 객체를 조작해 특수 사례를 처리하는 방식.

- 이를 사용하면 클래스나 객체가 예외적인 상황을 **캡슐화**해서 **처리**하기 때문에, 클라이언트 코드가 예외적인 상황을 처리할 필요가 없어진다.

## null을 반환하지 마라

- 오류를 유발하는 행위 중 첫째는 `null`**을 반환하는 습관**이다.

```java
// Bad

  public void registerItem(Item item) {
    if (item != null) {
      ItemRegistry registry = peristentStore.getItemRegistry();
      if (registry != null) {
        Item existing = registry.getItem(item.getID());
        if (existing.getBillingPeriod().hasRetailOwner()) {
          existing.register(item);
        }
      }
    }
  }
  
  
  // 위 peristentStore가 null인 경우에 대한 예외처리가 안 됐다.
  // 만약 여기서 NullPointerException이 발생했다면 수십단계 위의 메소드에서 처리해줘야 될까?
  // 이 메소드의 문제점은 null 체크가 부족한게 아니라 null 체크가 **너무 많다.**
```

```java
// Bad
  List<Employee> employees = getEmployees();
  if (employees != null) {
    for(Employee e : employees) {
      totalPay += e.getPay();
    }
  }
```

```java
// Good
public List<Employee> getEmployees() {
	if( .. there are no employees .. )
		return Collections.emptyList();
}
```

## null을 전달하지 마라

- 메서드에서 `null`을 반환하는 방식도 나쁘지만 메서드로 `null`을 전달하는 방식은 더 나쁘다.
- 정상적인 인수로 `null`을 기대하는 API가 아니라면 메서드로 `null`을 전달하는 코드는 최대한 피한다.

```java
// Bad
// calculator.xProjection(null, new Point(12, 13));
// 위와 같이 부를 경우 NullPointerException 발생
public class MetricsCalculator {
  public double xProjection(Point p1, Point p2) {
    return (p2.x – p1.x) * 1.5;
  }
  ...
}

// Bad
// NullPointerException은 안나지만 윗단계에서 InvalidArgumentException이 발생할 경우 처리해줘야 함.
public class MetricsCalculator {
  public double xProjection(Point p1, Point p2) {
    if(p1 == null || p2 == null){
      throw InvalidArgumentException("Invalid argument for MetricsCalculator.xProjection");
    }
    return (p2.x – p1.x) * 1.5;
  }
}

// Bad
// 좋은 명세이지만 첫번째 예시와 같이 NullPointerException 문제를 해결하지 못한다.
public class MetricsCalculator {
  public double xProjection(Point p1, Point p2) {
    assert p1 != null : "p1 should not be null";
    assert p2 != null : "p2 should not be null";
    
    return (p2.x – p1.x) * 1.5;
  }
}
```

## 결론

✔️  깨끗한 코드는 **읽기**도 좋아야 하지만, **안정성**도 높아야 한다.

- 오류 처리를 프로그램 논리와 분리해 **독자적인 사안으로 고려**하면 튼튼하고 깨끗한 코드를 작성할 수 있다.
- 오류 처리를 프로그램 논리와 분리하면 **독립적인 추론**이 가능해지며, **코드 유지보수성**도 크게 높아진다.
