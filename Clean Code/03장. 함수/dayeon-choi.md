

> 함수를 잘 만드는 법



## 작게 만들어라

👉 **2~4줄**의 함수, 간결하고 명백하게 읽힌다.
👉 if 문/else 문/while 문 등에 들어가는 블록은 **한 줄**이어야 한다.



## 한 가지만 해라!

👉 함수는 **한 가지**만을, **잘** 해야한다.

```java
public static String renderPageWithSetupsAndTeardowns(
  PageData pageData, boolean isSuite)
	if(isTestPage(pageData)){
    	includeSetupAndTearddownPages(pageData, isSuite);
    return pageData.getHtml();
}
```
👉 한 가지란? **하나의 추상화 수준**
위 코드가 하는 일은
1. 페이지가 테스트 페이지인지 판단한다.
2. 그렇다면 설정 페이지와 해제 페이지를 넣는다.
3. 페이지를 HTML로 랜더링한다.

👉 하나의 추상화 수준이란?
<img src='https://images.velog.io/images/dayeon-choi/post/77d30c08-c437-4f74-bc53-31cf55b09a85/image.png' /><br>

👉 함수 내 섹션
함수 내 섹션이 존재한다는 것은 한 함수에서 여러 작업을 한다는 것!



## 함수 당 추상화 수준은 하나로!

👉 함수가 확실히 한 가지의 작업만 하려면 함수 내 모든 문장의 추상화 수준이 동일해야 한다.
👉 추상화 수준이 **동일**하다는 것?
추상화 되어있는 정도가 동일하다는 것 → 직접적인 정도 or 추상적인 정도

### 위에서 아래로 코드 읽기 : 내려가기 규칙
👉 위에서 아래로 내려가듯 읽히는 코드가 좋은 코드
👉 함수의 추상화 수준이 아래로 갈 수록 한 번에 **한 단계씩 낮아지는 코드**
👉 아래로 갈 수록 상위에서 사용한 함수를 설명하는 느낌으로 (상위에서 쓴 함수를 하위에 붙여서 작성하는 식)



## Switch 문

👉 Switch 문을 작게 만들기는 어렵다.

👉 저차원 클래스에 숨기고 절대로 반복하지 않는 방법은 있다. (다형성 이용)

👉 Switch 문 함수, 다음의 원칙 준수하기
- SRP (단일 책임 원칙)
    - 클래스는 단 하나의 책임을 가져야하며, 변경되는 이유도 단 하나여야 한다.
    - 참고 : https://siyoon210.tistory.com/155
- OCP (개방 폐쇄의 원칙)
    - 확장에 대해서는 개방되어야 하지만, 변경에 대해서는 폐쇄되어야 한다.
    - 기존의 코드를 변경하지 않으면서 기능을 추가할 수 있도록 설계되어야 한다는 의미이다.
    - 참고 : https://dublin-java.tistory.com/48
    


## 서술적인 이름을 사용하라

👉 겁먹지 말 것. 짧고 어려운 이름보다 길고 **서술적인 이름**이 좋다.

👉 여러 단어가 **쉽게 읽히는 명명법**을 사용해 함수의 기능을 **잘 표현**하는 이름의 조합을 선택한다.

👉 IDE를 활용하면 이름 바꾸기가 더 쉬워진다.

👉 명명법 종류
- PascalCasing (파스칼 케이싱) EX) ```UtilityBox```
- CamelCasing (카멜 케이싱) EX) ```utilityBox```
- GNU Naming Convention EX) ```utility_box```
- Hungarian notation (헝가리안 표기법) EX) ```g_bTrue```
- BREW Naming Convention EX) ```IDISPLAY_ClearScreen```
- Constant (상수) EX) ```DEFAULT_COUNTRY_CODE```
- 참고 : https://yeop-blog.github.io/2017/09/29/2017-09-29-old-blog-post29/



## 함수 인수

👉 함수에서 이상적인 인수 개수는 **0개(무항)**이다.
👉 삼항은 가능한 피하는 것이 좋다. 
<span style='color:gray;font-size:small'>인수가 3개 이상으로 넘어가면 유효한 값으로 모든 조합을 구성해 테스트하기가 상당히 부담스러워진다.</span>
👉 4개 이상(다항)은 특별한 이유가 필요하다. <span style='color:gray;font-size:small'>있어도 비추</span>
👉 출력 인수는 비추한다. <span style='color:gray;font-size:small'>익숙하지 않고 이해하기 어려워 코드를 재차 확인하게 만든다.</span>

### 많이 쓰는 단항 형식
👉 함수에 인수 1개를 넘기는 경우 2가지 <span style='color:gray;font-size:small'>함수에 이름을 지을 때는 두 경우를 분명히 구분한다.</span>
* 인수에 질문을 던지는 경우
* 인수를 뭔가로 변환해 결과를 반환하는 경우

### 플래그 인수
👉 플래그 인수, 함수로 부울 값을 넘기는 관례는 추하다.
👉 함수가 여러가지 일을 한다는 뜻이니까.

### 이항 함수
👉 인수 2개는 한 값을 표현하는 두 요소여야 한다.
👉 인수 2개의 요소가 자연적인 순서가 없어 인위적으로 기억해야하는 함수는 비추다.
👉 무조건 나쁘다는 뜻이 아니라, 그만큼 위험하다는 것이다.

### 삼항 함수
👉 주춤하게 되는, 무시해야 하는 문제가 두 배 이상 늘어난다.
👉 ```assertEquals(1.0,amount,.001)```와 같은 경우, 부동소수점 비교가 상대적이기 때문에 가치가 충분하다고 느낀다고 한다.
 
### 인수 객체
👉 객체를 생성해 인수를 줄이는 방법은 변수를 묶어 넘기려면 이름을 붙여야하므로 개념을 표현하게 된다. 눈속임조차 될 수 없음.

### 인수 목록
👉 가변 인수를 취하는 함수는 단항, 이항, 삼항 함수로 취급할 수 있다. 하지만 이를 넘어서는 함수를 사용할 경우는 문제!

### 동사와 키워드
👉 단항 함수는 함수와 인수가 동사/명사 쌍을 이뤄야 한다. EX) ```writeField(name)```



## 부수 효과를 일으키지 마라!
👉 부수 효과는 거짓말! 남몰래 한 가지 일 이상을 하니까.
👉 많은 경우 시간적인 결합이나 순서 종속성을 초래한다.
👉 시간적인 결합이 필요하다면 함수 이름에 분명히 명시한다.

### 출력 인수
👉 출력 인수 대신 **this**를 사용하라.
👉 EX) ```appendFooter(s)``` → ```report.appendFooter()```
👉 함수에서 상태를 변경해야 한다면 함수가 속한 객체 상태를 변경하는 방식을 택하라.



## 명령과 조회를 분리하라!

👉 함수는 뭔가를 수행하거나 뭔가에 답하거나 **둘 중 하나만** 해야 한다.
👉 객체 상태를 변경하거나 객체 정보를 반환하거나 둘 중 하나다.



## 오류 코드보다 예외를 사용하라!

👉 **명령 함수에서 오류 코드를 반환**하는 방식은 명령/조회 분리 규칙을 **위반**한다.
👉 오류 처리 역시 한 가지 작업이다.

### Try/Catch 블록 뽑아내기
👉 코드 구조에 혼란을 일으키고, 정상 동작과 오류 처리 동작을 뒤섞는다.
👉 try/catch 블록을 **별도 함수로** 뽑아내는 편이 좋다.
```java
public void delete(Page page){
	try{
    	deletePageAndAllReferences(page);
    }catch(Exception e){
    	logError(e);
    }
}
private void deletePageAndAllReferences(page page) throws Exception{
	//deletePage...
}
private void logError(Exception e){
	//log...
}
```

### Error.java 의존성 자석
👉 오류 코드를 반환한다는 것은 어디선가 오류 코드를 정의한다는 뜻이다.
👉 오류 코드 대신 **예외**를 사용하면 새 예외는 Exception 클래스에서 파생된다. 따라서 재컴파일/재배치 없이도 새 예외 클래스를 추가할 수 있다.



## 반복하지 마라!

👉 중복 제거 전략
* 구조적 프로그래밍
절차적 프로그래밍의 하위 개념. GOTO 문을 없애거나 GOTO 문에 대한 의존성을 줄임.
return 문이 하나여야 하며, 루프 안에서 break, continue, goto 사용 금지.
* AOP (관점 지향 프로그래밍)
어떤 로직을 기준으로 핵심적인 관점, 부가적인 관점으로 나누어서 보고 그 관점을 기준으로 각각 모듈화.
* COP (컴포넌트 지향 프로그래밍)
논리를 분리하고 캡슐화.



## 구조적 프로그래밍

👉 ❌
👉 구조적 프로그래밍의 목표와 규율은 함수가 작다면 별 이익을 얻지 못함.
👉 **함수가 작다면** return, break, continue를 여러 차례 사용해도 괜찮음. 



## 함수를 어떻게 짜죠?

👉 처음부터 짜내긴 어렵다.
👉 처음을 다듬고, 함수를 만들고, 이름을 바꾸고, 중복을 제거하고, 메서드를 줄이고, 순서를 바꾼다. 때로는 전체 클래스를 쪼개기도 한다.
👉 와중에 코드는 항상 단위 테스트를 통과한다.



## 결론
👉 작성하는 함수가 분명하고 정확한 언어로 깔끔하게 같이 맞아떨어져야 이야기를 풀어가기 쉬워진다.



## 인상 깊었던...
> _어떤 코드든 절대로 무시하면 안된다. 무시한 코드에 오류가 숨어드니까._

> _많은 원칙과 기법이 중복을 없애거나 제어할 목적으로 나왔다._

> _대가 프로그래머는 시스템을 '구현할' 프로그램이 아니라 '풀어갈' 이야기로 여긴다._

> _프로그래밍 언어라는 수단을 사용해 좀 더 풍부하고 좀 더 표현력이 강한 언어를 만들어 이야기를 풀어나간다._