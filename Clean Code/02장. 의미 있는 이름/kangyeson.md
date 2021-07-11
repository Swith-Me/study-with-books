# 02. 의미 있는 이름
변수, 함수, 인수, 클래스, 그리고 패키지 등등. 소프트웨어 어디서나 이름을 붙인다. <br>
이름을 잘 지으면 코드 가독성과 시간 절약으로, 단기적인 효과는 물론 장기적인 이익도 볼 수 있다. 
<br><br>

## 1. 의도를 분명히 밝혀라
> 좋은 이름을 지으려면 시간이 걸리지만 좋은 이름으로 절약하는 시간이 훨씬 더 많다.

- **존재 이유, 수행 기능, 사용 방법**이 명확히 드러나야 한다. <br>
- 아무리 단순한 코드라도 d나 theList같은 이름의 변수로는 코드를 이해할 수 없다. 이름에서 의미, 즉 **함축성** 자체가 없기 때문에 코드의 맥락을 읽을 수 없는 것이다.
- 독자가 변수의 의미와 쓰임을 전부 안다고 가정하지 말자. <br>
```java
int daysSinceCreation;  //<-int d; 
```
```java
for (int[] cell : gameBoard)  //<-for (int[] x : theList) 
```
<br><br>

## 2. 그릇된 정보를 피하라
> 유사한 개념은 유사한 표기법을 사용한다. 이것도 정보다. **일관성**이 떨어지는 표기법은 그릇된 정보다.

프로그래머는 코드에 그릇된 단서를 남겨서는 안 된다. 그릇된 단서는 코드 의미를 흐린다.<br>
### 그릇된 단서란? 
\= hp(hypotenuse, 빗변)와 같은 **약어**, 소문자L(l)과 대문자O처럼 숫자 1, 0으로 혼동할 수 있는 **유사 표기법**, **흡사한 이름** 등을 말한다. 

###### ( 프로그래머에게 List라는 단어는 특수한 의미이다. 실제 컨테이너가 List인 경우라도 컨테이너 유형을 이름에 넣지 않는 편이 바람직하다. )
<br><br>

## 3. 의미 있게 구분하라
> 컴파일러나 인터프리터만 통과하려는 생각으로 코드를 구현하는 프로그래머는 스스로 문제를 일으킨다.

연속적인 숫자를 덧붙인 이름(a1, a2, a3..)과 불용어(the, an, is, my..)를 추가한 이름은 의도가 전혀 드러나지 않으며, 아무런 정보도 제공하지 못한다.
<br><br><br>

## 4. 발음과 검색이 쉬운 이름을 사용하라
> 발음하기 쉬운 이름은 중요하다. 프로그래밍은 **사회 활동**이기 때문이다.

'genymdhms'과 같은 변수의 어려운 발음만으로도, 협업하는 팀원과 이 변수에 대해 이야기하는 것이 어려울 수 있다. <br>또한 발음으로 단어의 개념을 기억하는 우리 뇌에서 genymdhms 변수는 금세 잊혀질 것이다.
<br><br>

> 이름 길이는 범위 크기에 비례해야 한다.

숫자 7이나 문자 e와 같이 거의 모든 프로그램과 문장에 들어가 **검색범위가 넓은 것**은 변수 이름으로 적합하지 않다.<br>
긴 이름이 짧은 이름보다 좋고, 검색하기 쉬운 이름이 상수보다 좋다.
<br><br><br>

## 5. 자신의 기억력과 유머 감각을 뽐내지 마라
> 똑똑한 프로그래머와 전문가 프로그래머 사이에서 나타나는 차이점 하나만 들자면, 전문가 프로그래머는 **명료함**이 최고라는 사실을 이해한다.

- 독자가 코드를 읽으면서 변수 이름을 자신이 아는 이름으로 변환해야 한다면 그 변수 이름은 바람직하지 못하다. <br>
- kill() 대신 whack()을 사용하는 등, 특정 문화에서만 사용하는 농담은 피하는 편이 좋다. <br>
- 의도가 분명하고 솔직하며, 남들이 이해하는 코드를 짜자. 
<br><br><br>

## 6. 한 개념에 한 단어를 사용하라
> 추상적인 개념 하나에 단어 하나를 선택해 이를 고수한다.

클래스마다 fetch, retrieve, get으로 제각각 부르는 것은 독자에게 혼란을 준다. <br>
**일관성** 있는 어휘를 사용하자.
<br><br>

> 한 단어를 두 가지 목적으로 사용하지 마라. 다른 개념에 같은 단어를 사용한다면 그것은 말장난에 불과하다.

같은 맥락이 아닌데도 '일관성'을 고려해 다른 의미지만 같은 단어를 사용하는 경우가 있다. <br>
두 값을 더하는 메서드와, 값 하나를 추가하는 메서드에 동일하게 add를 붙이는 것은 말장난이다. <br>
add와 insert로 구분하는 것이 바람직하다.
<br><br><br>

## 7. 해법 영역과 문제 영역에서 가져온 이름을 사용하라
> 코드를 읽을 사람도 프로그래머라는 사실을 명심한다.

- 전산 용어, 알고리즘 이름, 패턴 이름, 수학 용어 등을 사용해도 괜찮다. <br>
- 만약 적절한 프로그래머 용어가 없다면 문제 영역에서 이름을 가져오자. 
<br><br><br>

## 8. 의미 있는 맥락을 추가하고 불필요한 맥락을 없애라
- 클래스, 함수, 이름 공간에 넣어 맥락을 부여한다. 
- 모든 방법이 실패하면 마지막 수단으로 접두어를 붙인다.
- 맥락이 불분명한 변수는, 함수를 작은 조각으로 쪼개어 개선할 수 있다. 
```java 
// 맥락이 불분명한 코드
private void printGuessStatistics(char candidate, int count) {
    String number;
    String verb;
    String pluralModifier;
    if(count == 0) {
        number = "no";
        verb = "are";
        pluralModifier = "s";
    } else if (count == 1) {
        number = "1";
        verb = "is";
        pluralModifier = "";
    } else {
        number = Integer.toString(count);
        verb = "are";
        pluralModifier = "s";
    }
    String guessMessage = String.format(
        "There %s %s %s%s", verb, number, candidate, pluralModifier
    );
    print(guessMessage);
}
```
```java
// 맥락이 분명한 코드
public class printGuessStatisticsMessage {
    private String number;
    private String verb;
    private String pluralModifier;
 
    public String make(char candidate, int count) {
        createPluralDependentMessageParts(count);
        return String.format(
            "There %s %s %s%s",
            verb, number, candidate, pluralModifier
        );
    }
 
    private void createPluralDependentMessageParts(int count) {
        if(count == 0) {
            thereAreNoLetters();
        } else if (count == 1) {
            thereIsOneLetter();
        } else {
            thereAreManyLetters(count);
        }
    }
 
    private void thereAreManyLetters(int count) {
        number = Integer.toString(count);
        verb = "are";
        pluralModifier = "s";
    }
 
    private void thereIsOneLetter() {
        number = "1";
        verb = "is";
        pluralModifier = "";
    }
 
    private void thereAreNoLetters() {
        number = "no";
        verb = "are";
        pluralModifier = "s";
    }
}
```
<br><br>

> 짧은 이름이 긴 이름보다 좋다. 단, 의미가 분명한 경우에 한해서다. 이름에 불필요한 맥락을 추가하지 않도록 주의한다.

불필요하게 붙어서 IDE를 방해하기만 하는 접두어는 생략하자.
<br><br><br>

***
## 인코딩을 피하라 
### 인코딩이란? 
\= 변수에 부가 정보를 덧붙여 표기하는 것을 말한다.
 - 변수명에 해당 변수의 타입(String, Int 등)을 적지 않는다.
 - 맴버 변수 접두어를 붙이지 않는다.
 - 인터페이스 클래스와 구현 클래스를 나눠야 한다면 구현 클래스의 이름에 정보를 인코딩한다.

## 클래스 이름
- 클래스 이름과 객체 이름은 명사나 명사구가 적합하다.
- Customer, WikiPage, Account, AddressParser 등이 적절하다.
- Manager, Processor, Data, Info 등은 피하고, 동사는 사용하지 않는다.
<br><br>

## 메서드 이름
- 동사나 동사구가 적합하다.
- postPayment, deletePage, save 등이 적절하다.
- 접근자, 변경자, 조건자는 값 앞에 get, set, is를 붙인다.
- 생성자를 오버로드할 경우 정적 팩토리 메서드를 사용하고 해당 생성자를 private으로 선언한다.


