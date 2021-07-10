# 📘 02장. 의미 있는 이름

## 의도를 분명히 밝혀라

[ ] 변수의 존재 이유는?   
[ ] 수행 기능은?   
[ ] 사용 방법은?   

<br />

```java
int d;  // 경과
```

이름 `d`는 아무 의미도 드러내지 않음

```java
int elapsedTimeInDays;
int daysSinceCreation;
int daysSinceModification;
int fileAgeInDays;
```

의도가 드러나는 이름을 사용하면 코드 이해와 변경이 쉬워짐

<br />

## 그릇된 정보를 피하라

- 널리 쓰이는 의미가 있는 단어를 다른 의미로 사용하면 안 된다
- 여러 계정을 그룹으로 묶을 때, 실제 List가 아니라면 사용하지 않는다
- 서로 흡사한 이름을 사용하지 않도록 주의한다

<br />

## 의미 있게 구분하라

- 불용어는 중복이다  
  ```Product```와 ```ProductInfo```는 개념을 구분하지 않은 채 이름만 달리한 경우다

<br />

## 발음하기 쉬운 이름을 사용하라

프로그래밍은 사회 활동이기 때문이다

```java
class DtaRcrd102 {
  private Date genymdhms; // generate date, year, month, day, hour, minute, second
  private Date modymdhms;
  private final String pszqint = "102";
};

class Customer {
  private Date generationTimestamp;
  private Date modificationTimestamp;
  private final String recordId = "102";
}
```

두 번째 코드는 지적인 대화가 가능해진다

<br />

## 검색하기 쉬운 이름을 사용하라

검색하기 쉬운 이름이 상수보다 좋다
```java
const int WORK_DAYS_PER_WEEK = 5;
```

```5```보다 ```WORK_DAYS_PER_WEEK``` 찾기가 훨씬 쉽다

<br />

## 인코딩을 피하라

- 헝가리식 표기법   
  변수 이름에 타입을 인코딩할 필요가 없다
- 멤버 변수 접두어
  멤버 변수에 ```m_```이라는 접두어를 붙일 필요가 없다
- 인터페이스 클래스와 구현 클래스
  인코딩이 필요한 경우로, 구현 클래스의 이름에 인코딩하는 것을 택하겠다

<br />

## 자신의 기억력을 자랑하지 마라

독자가 코드를 읽으면서 변수 이름을 자신이 아는 이름으로 변환해야 한다면 그 변수 이름은 바람직하지 못하다

<br />

## 클래스 이름

- **명사, 명사구**가 적합하다
- ```Manager```, ```Processor```, ```Data```, ```Info``` 등과 같은 단어는 피한다
- 동사는 사용하지 않는다

```
Customer
WikiPage
Account
AddressParser
```

<br />

## 메서드 이름

- **동사, 동사구**가 적합하다
- 접근자, 변경자, 조건자는 ```get, set, is```를 붙인다

```
postPayment
deletePage
save
```

- 생성자를 중복정의할 때는 정적 팩토리 메서드를 사용한다
```java
// Good
Complex fulcrumPoint = Complex.FromRealNumber(23.0);
// Bad
Complex fulcrumPoint = new Complex(23.0);
```

<br />

##  기발한 이름은 피하라

- 재미난 이름보다 명료한 이름을 선택하라
- 특정 문화에서만 사용하는 농담은 피하는 편이 좋다

<br />

## 한 개념에 한 단어를 사용하라

추상적인 개념 하나에 단어 하나를 선택해 이를 고수한다

예를 들어 똑같은 메서드를 클래스마다 ```retrieve```, ```fetch```, ```get```으로 제각각 부르면 혼란스럽다

<br />

## 말장난을 하지 마라

한 단어를 두 가지 목적으로 사용하지 마라

예를 들어 지금까지 구현한 ```add``` 메서드는 모두가 기존 값 두 개를 더하거나 이어서 새로운 값을 만든다고 가정했을 때, 집합에 값을 하나 추가하는 새로운 메서드는 ```add```로 불러서는 안 된다

<br />

## 해법 영역에서 가져온 이름을 사용하라

코드를 읽을 사람도 프로그래머이므로 전산 용어, 알고리즘 이름, 패턴 이름, 수학 용어 등을 사용해도 괜찮다

<br />

## 문제 영역에서 가져온 이름을 사용하라

적절한 '프로그래머 용어'가 없다면 문제 영역에서 이름을 가져온다

<br />

## 의미 있는 맥락을 추가하라

대부분의 이름이 스스로 의미가 분명하지 않아서 클래스, 함수, 이름 공간에 넣어 맥락을 부여한다. 하지만 모든 방법이 실패하면 마지막 수단으로 접두어를 붙인다.

```
firstName, lastName, street, houseNumber, city, state, zipcode
addrFirstName, addrLastName, addrState...
```
