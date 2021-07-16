남들이 변수에 의존하지 않게 만들고 싶고, 변수 타입이나 구현을 맘대로 바꾸고 싶어서 변수를 `비공개(private)`로 정의하기도 한다.

그렇다면 어째서 수많은 프로그래머가 `조회(get)`함수와 `설정(set)`함수를 당연하게 `공개(public)`해 비공개 변수를 외부에 노출할까?

## 자료 추상화

목록 6-1 구체적인 Point 클래스

```java
public class Point { 
  public double x; 
  public double y;
}
```

목록 6-2 추상적인 Point 클래스

```java
public interface Point {
  double getX();
  double getY();
  void setCartesian(double x, double y); 
  double getR();
  double getTheta();
  void setPolar(double r, double theta); 
}
```

목록 6-2는 구현을 완전히 숨긴다. 그에 반해 목록 6-1은 내부 구조를 노출하고, 개별적으로 좌표값을 읽고 설정하게 강제한다.

변수를 `private`으로 선언 하더라도 각 값마다 `조회(get)`함수와 `설정(set)`함수를 제공한다면 구현을 외부로 노출하는 셈이다.

변수 사이에 함수라는 계층을 넣는다고 구현이 저절로 감춰지지 않아 이를 감추려면 **추상화**가 필요하다. 그저 `get`과 `set`으로 변수를 다룬다고 클래스가 되지는 않는다.

그보다는 추상 인터페이스를 제공해 사용자가 구현을 모른 채 자료의 핵심을 조작할 수 있어야 진정한 클래스라고 할 수 있다.

목록 6-3 구체적인 Vehicle 클래스

```java
public interface Vahicle {
	double getFuelTankCapacityInGallons();
	double getGallonsOfGasoline();
}
```

목록 6-4 추상적인 Vehicle 클래스

```java
public interface Vehicle {
	double getPercentFuelRemaining();
}
```

목록 6-3은 자동차 연료 상태를 구체적인 숫자 값으로 알려주어 두 함수가 변수값을 읽어 반환할 사실이 확실하다. 그에 반해 목록 6-4는 자동차 연료 상태를 백분율이라는 추상적인 개념으로 알려주어 정보가 어디서 오는지 전혀 드러나지 않는다.

✔ 자료를 세세하게 공개하기보다는 **추상적인 개념**으로 **표현**하는 편이 좋다.

인터페이스나 조회/설정 함수만으로는 추상화가 이뤄지지 않으니 객체가 포함하는 자료를 표현할 가장 좋은 방법을 고민해보되, 아무 생각 없이 조회/설정 함수를 추가하지는 말자.

## 자료/객체 비대칭

- 객체는 추상화 뒤로 자료를 숨긴 채 자료를 다루는 함수만 공개한다.
- 반면, 자료 구조는 자료를 그대로 공개하며 별다른 함수는 제공하지 않는다.

두 정의는 본질적으로 상반된다. 두 개념은 사실상 정반대다. 사소한 차이로 보일지 모르지만 그 차이가 미치는 영향은 굉장히다.

목록 6-5 절차적인 도형 (Procedural Shape)

```java
public class Square { 
  public Point topLeft; 
  public double side;
}

public class Rectangle { 
  public Point topLeft; 
  public double height; 
  public double width;
}

public class Circle { 
  public Point center; 
  public double radius;
}

public class Geometry {
  public final double PI = 3.141592653589793;
  
  public double area(Object shape) throws NoSuchShapeException {
    if (shape instanceof Square) { 
      Square s = (Square)shape; 
      return s.side * s.side;
    } else if (shape instanceof Rectangle) { 
      Rectangle r = (Rectangle)shape; 
      return r.height * r.width;
    } else if (shape instanceof Circle) {
      Circle c = (Circle)shape;
      return PI * c.radius * c.radius; 
    }
    throw new NoSuchShapeException(); 
  }
}
```

이 코드에 있는 클래스가 절차적이라 비판한다면 맞는 말이지만, 100% 옳다고 말하기는 어렵다.

만약 `Geometry` 클래스에 둘레 길이를 구하는 `perimeter()` 함수를 추가하고 싶지만 도형 클래스와 도형 클래스에 의존하는 다른 클래스는 아무 영향도 받지 않는다.

반대로 새 도형을 추가하고 싶다면, `Geometry` 클래스에 속한 함수를 모두 고쳐야 한다. 그래서 두 조건은 완전히 정반대라고 할 수 있다.

목록 6-6 다형적인 도형 (Polymorphic Shape)

```java
public class Square implements Shape { 
  private Point topLeft;
  private double side;
  
  public double area() { 
    return side * side;
  } 
}

public class Rectangle implements Shape { 
  private Point topLeft;
  private double height;
  private double width;

  public double area() { 
    return height * width;
  } 
}

public class Circle implements Shape { 
  private Point center;
  private double radius;
  public final double PI = 3.141592653589793;

  public double area() {
    return PI * radius * radius;
  } 
}
```

이 코드를 보면, 객체 지향적인 도형 클래스고, 여기서 `area()`는 `다형(polymorphic)` 메서드다. `Geometry` 클래스는 필요 없어 새 도형을 추가해도 기존 함수에 아무런 영향을 미치지 않는다. 하지만, 새 함수를 추가하고 싶다면 도형 클래스 전부를 고쳐야 한다.

앞서도 말했듯이, 목록 6-5와 목록 6-6은 상호 보완적인 특질이 있다.

그래서 객체와 자료구조는 근본적으로 양분된다.

> (자료 구조를 사용하는) 절차적인 코드는 기존 자료 구조를 변경하지 않으면서 새 함수를 추가하기 쉽다. 반면, 객체 지향 코드는 기존 함수를 변경하지 않으면서 새 클래스를 추가하기 쉽다.

반대쪽도 참이다.

> 절차적인 코드는 새로운 자료 구조를 추가하기 어렵다. 그러려면 모든 함수를 고쳐야 한다. 객체 지향 코드는 새로운 함수를 추가하기 어렵다. 그러려면 모든 클래스를 고쳐야 한다.

✔ **객체 지향 코드에서 어려운 변경은 절차적인 코드에서 쉬우며, 절차적인 코드에서 어려운 변경은 객체 지향 코드에서 쉽다.**

복잡한 시스템을 짜다 보면 새로운 함수가 필요할 경우거나, 새로운 자료 타입이 필요한 경우가 생긴다. 이때 상황에 맞게 **클래스 & 객체 지향 기법**을 사용하거나, **절차적인 코드와 자료 구조**를 적절하게 사용하는 것이 좋다.

분별 있는 프로그래머는 모든 것이 객체라는 생각이 **미신**임을 잘 안다. 때로는 단순한 자료 구조와 절차적인 코드가 가장 적합한 상황도 있다.

## 디미터 법칙

: 휴리스틱; heuristic(경험에 기반하여 문제를 해결하거나 학습하거나 발견해 내는 방법)으로, 모듈은 자신이 조작하는 객체의 속사정을 몰라야 한다는 법칙

좀 더 정확히 표현하자면, 디미터 법칙은 "클래스 C의 메서드 f는 다음과 같은 객체의 메서드만 호출해야 한다"고 주장한다.

- 클래스 C
- f가 생성한 객체
- f 인수로 넘어온 객체
- C 인스턴스 변수에 저장된 객체

하지만 위 객체에서 허용된 메서드가 반환하는 객체의 메서드는 호출하면 안 된다. 다시 말해, 낯선 사람은 경계하고 친구랑만 놀라는 의미이다.

### 기차 충돌

```java
final String outputDir = ctxt.getOptions().getScratchDir().getAbsolutePath();
```

이 코드를 보면, 여러 객차가 한 줄로 이어진 기차처럼 보이기 때문에 이와 같은 코드를 **기차 충돌(train wreck)**이라 부른다.

하지만, 이는 일반적으로 조잡하다 여겨지는 편으로 피하는 편이 좋고, 다음과 같이 나누는 편이 좋다.

```java
Options opts = ctxt.getOptions();
File scratchDir = opts.getScratchDir();
final String outputDir = scratchDir.getAbsolutePath();
```

위 예제가 디미터 법칙을 위반하는지 여부는 위의 변수들이 객체인지 자료 구조인지에 달렸다. 객체라면 내부 구조를 숨겨야 하므로 확실히 디미터 법칙을 위반한다. 반면, 자료 구조라면 당연히 내부 구조를 노출하므로 문제되지 않는다.

### 잡종 구조

이런 혼란으로 말미암아 때때로 절반은 객체, 절반은 자료 구조인 잡종 구조가 나온다.

이는 중요한 기능을 수행하는 함수와 공개 변수 or 비공개 변수를 그대로 노출하는 공개 조회/설정 함수도 있다.

덕분에 다른 함수가 절차적인 프로그래밍의 자료 구조 접근 방식처럼 비공개 변수를 사용하고픈 유혹에 빠지기 십상이다.

✔ 이런 잡종 구조는 새로운 함수는 물론이고, 새로운 자료 구조도 추가하기 어려워 단점만 모아놓은 구조로 되도록 피하는 편이 좋다.

### 구조체 감추기

객체라면 내부 구조를 감춰야 하는데, 그러면 임시 디렉터리의 절대 경로는 어떻게 얻어야 좋을까?

다음 두 코드를 살펴보자.

```java
// 방법-1) ctxt 객체에 공개해야 하는 메서드가 너무 많아진다.
ctxt.getAbsolutePathOfScratchDirectoryOption();

// 방법-2) getScratchDirectoryOption()이 객체가 아닌, 자료 구조를 반환한다고 가정해도 별로 좋지 않다.
ctx.getScratchDirectoryOption().getAbsolutePath();
```

그렇다면, 절대 경로를 얻어 어디에 쓸려고 그러는 걸까? 다음 코드를 보자.

```java
String outFile = outputDir + "/" + className.replace('.', '/') + ".class"; 
FileOutputStream fout = new FileOutputStream(outFile); 
BufferedOutputStream bos = new BufferedOutputStream(fout);
```

추상화 수준을 뒤섞어 놓아 다소 불편하다. 점, 슬래시, 파일 확장자, File 객체를 부주의하게 마구 뒤섞으면 안 된다. 어찌 되었거나, 위 코드에 따르면 경로를 얻으려는 이유가 임시 파일을 생성하기 위함을 알 수 있다.

그렇다면 ctxt 객체에 임시 파일을 생성하라고 시키면 어떨까?

```java
BufferedOutputStream bos = ctxt.createScratchFileStream(classFileName);
```

객체에게 맡기기에 적당한 임무로 보인다! ctxt는 내부 구조를 드러내지 않으며, 모듈은 자신이 몰라야 하는 여러 객체를 탐색할 필요가 없다. 따라서 디미터 법칙을 위반하지 않는다.

## 자료 전달 객체

자료 구조체의 전형적인 형태는 **공개 변수만 있고 함수가 없는** 클래스다. 이를 때로는 **자료 전달 객체(Data Transfer Object, DTO)**라고 한다.

- 굉장히 유용한 구조체
- **DB와 통신**하거나, **socket**에서 받은 **메시지 구문을 분석**할 때 유용하다.
- DB에 저장된 가공되지 않은 정보를 애플리케이션 코드에서 사용할 객체로 변환하는 일련의 단계에서 가장 처음으로 사용하는 구조체
- **빈(bean) 구조**
    - 비공개 변수를 조회/설정 함수로 조작 ⇒ **사이비 캡슐화**. 별다른 이익 제공 X

```java
public class Address { 
  public String street; 
  public String streetExtra; 
  public String city; 
  public String state; 
  public String zip;

	public Address(String street, String streetExtra, String city, String state, String zip) {
		this.street = street;
		this.streetExtra = streetExtra;
		this.city = city;
		this.state = state;
		this.zip = zip;
  	}

	public String getStreet() {
		return street;
	}
	public String getStreetExtra() {
		return streetExtra;
	}
	public String getCity() {
		return city;
	}
	public String getState() {
		return state;
	}
	public String getZip() {
		return zip;
	}
}
```

### 활성 레코드

- DTO의 특수한 형태
- 공개 변수가 있거나 비공개 변수에 조회/설정 함수가 있는 자료 구조
    - 대게 `save`나 `find`와 같은 탐색 함수도 제공
- DB 테이블이나 다른 소스에서 자료를 직접 변환한 결과

불행히도 활성 레코드에 비즈니스 규칙 메서드를 추가해 이런 자료 구조를 객체로 취급하는 개발자가 흔하지만, 이는 바람직하지 않다.

⇒ 자료 구조도 아니고, 객체도 아닌 잡종 구조가 나오기 때문

해결책은 활성 레코드는 자료 구조로 취급하기 때문에, 비즈니스 규칙을 담으면서 내부 자료를 숨기는 객체를 따로 생성한다. (여기서 내부 자료는 활성 레코드의 인스턴스일 가능성이 높다.)

## 결론

- 객체는 **동작을 공개**하고 **자료를 숨긴다.**
    - 기존 동작을 변경하지 않으면서 새 객체 타입 추가는 쉽지만, 기존 객체에 새 동작 추가는 어렵다.
- 자료 구조는 **별다른 동작 없이 자료를 노출**한다.
    - 기존 자료 구조에 새 동작 추가는 쉬우나, 기존 함수에 새 자료 구조 추가는 어렵다.

✔ 어떤 시스템을 구현할 때, **새로운 자료 타입을 추가**하는 유연성이 필요한 것은 **객체**가, **새로운 동작을 추가**하는 유연성이 필요한 것은 **자료 구조**와 **절차적인 코드**가 더 적합하다.
