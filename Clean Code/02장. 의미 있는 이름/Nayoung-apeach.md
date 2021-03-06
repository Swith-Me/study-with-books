# 2. 의미 있는 이름💌

### 의도를 분명히 밝혀라

☑**변수나 함수, 클래스 이름은 3가지 질문에 모두 답해야 한다.** 

1. 변수(함수, 클래스)의 존재 이유는?
2. 수행 기능은?
3. 사용 방법은?

❌이름 d는 아무 의미도 드러나지 않는다. 

```python
int d;
```

⭕이처럼 의도가 드러나는 이름을 사용해야 코드 이해와 변경이 쉬워진다. 

```python
int elapsedTimeInDays;
int daysSinceCreation;
int daysSinceModification;
int fileAgeInDays;
```

### 그릇된 정보를 피하라

☑**프로그래머는 코드에 그릇된 단서를 남겨서는 안 된다.** 

- 널리 쓰이는 의미가 있는 단어를 다른 의미로 사용하면 안 된다.

    ex) hp, aix, sco는 변수 이름으로 적합하지 않다. 유닉스 플랫폼이나 유닉스 변종을 가리키는 이름이기 때문이다. 

- 프로그래밍에 특수한 의미를 가지는 단어는 사용하면 안 된다.

    ex) list 변수

- 숫자와 헷갈리는 소문자 L 이나 대문자 O는 사용하면 안 된다.

### 의미 있게 구분하라

☑**컴파일러나 인터프리터만 겨우 통과하는 코드를 짜지 말아야 한다.** 

- 연속된 숫자를 덧붙이거나 불용어(분석에 큰 의미가 없는 단어)를 추가하지 말아야 한다.

    ex) a1, a2, ... aN 과 같은 이름은 그릇된 정보를 제공하는 이름도 아니고, 정보를 제공하는 이름도 아니다. 

- 구분이 되지 않는 변수를 사용하면 안 된다.

    ex) moneyAmount 와 money 는 구분이 되지 않는다. 

### 발음하기 쉬운 이름을 사용하라

☑**사람들은 단어에 능숙하다.**

- 발음하기 쉬운 단어들을 사용해야 지적인 대화가 가능해진다.

    ex) genymdhms 보다 generationTimestamp가 더 알아듣고 말하기 쉽다.

### 검색하기 쉬운 이름을 사용하라

☑**이름 길이는 범위 크기에 비례해야 한다.**

변수나 상수를 코드 여러 곳에서 사용한다면 검색하기 쉬운 이름이 바람직하다.

ex) 

```python
for (int j=0; j<34; j++){
	s += (t[j]*4)/5;
}
```

와

```python
int realDaysPerIdealDay = 4;
const int WORK_DAYS_PER_WEEK = 5;
int sum = 0; 
for(int j=0; j<NUMBER_OF_TASKS; j++){
	int realTaskDays = taskEstimate[j] * realDaysPerIdealDay;
	int realTaskWeeks = (realTaskDays / WORK_DAYS_PER_WEEK);
	sum += realTaskWeeks;
}
```

이 코드에서 sum이 유용한 변수는 아니나 **검색**이 가능하다.

### 인코딩을 피하라

☑**헝가리식 표기법**

변수 이름에 타입을 인코딩할 필요가 없다.

☑**멤버 변수 접두어**

변수에 m_이라는 접두어를 붙일 필요도 없다. 

클래스와 함수는 접두어가 필요없을 정도로 작아야 마땅하기 때문이다.

☑**인터페이스 클래스와 구현 클래스**

인터페이스 클래스 이름과 구현 클래스 이름 중 하나를 인코딩해야 한다면 구현 클래스의 이름을 인코딩 해야한다.  

### 자신의 기억력을 자랑하지 마라

☑**명료함이 최고다.**

반복문의 i,j,k를 제외하고는 문자 하나만 사용하는 변수 이름은 문제가 있다.

자신보다 남들이 이해하는 코드를 내놓아야 한다. 

### 클래스 이름

클래스 이름과 객체 이름은 명사나 명사구가 적당하다.

동사는 사용하지 않는다. 

### 메서드 이름

메서드 이름은 동사나 동사구가 적당하다. 

접근자, 변경자, 조건자는 **javabean** 표준에 따라 값 앞에 get, set, is 를 붙인다.

### 기발한 이름은 피하라

명료한 이름을 선택한다.

의도를 분명하고 솔직하게 표현해야 한다. 

### 한 개념에 한 단어를 사용하라

☑**추상적인 개념 하나에 단어 하나를 선택해 이를 고수한다.**

똑같은 메서드를 클래스마다 다르게 부르면 혼란스럽기 때문이다.

일관된 용어를 써야한다.

### 말장난을 하지 마라

☑**한 단어를 두 가지 목적으로 사용하지 말아야 한다.**

예를들어 add 메서드가 두개의 값을 더하는 역할이었다면 새로운 것을 작성하는 메서드의 이름은 insert 또는 append라는 이름이 적당하다.

### 해법 영역에서 가져온 이름을 사용하라

☑**코드를 읽을 사람도 프로그래머이다.** 

전산 용어, 알고리즘 이름, 패턴 이름, 수학 용어 등을 사용해도 알아들을 수 있다. 

### 문제 영역에서 가져온 이름을 사용하라

적절한 "프로그래머 용어"가 없다면 문제 영역에서 이름을 가져온다. 

### 의미 있는 맥락을 추가하라

☑**스스로 의미가 분명한 단어가 없지는 않지만 대부분의 이름은 그렇기 못하다.**

클래스, 함수, 이름 공간에 넣어 맥락을 부여한다. 

모든 방법이 실패하면 마지막 수단으로 접두어를 붙인다.

### 불필요한 맥락을 없애라

짧은 이름이 긴 이름보다 좋다. (단, 의미가 분명한 경우)

이름에 불필요한 맥락을 추가하지 않도록 한다. 

### 마치면서

이름을 나름대로 바꾸었다가 누군가 질책할지도 모른다. 하지만 코드를 개선하려는 노력을 중단해서는 안 된다. 

- 기억에 남는 구절 📖

> 단기적인 효과는 물론 장기적인 이익도 보장한다.

> 특정 문화에서만 사용하는 농담은 피하는 편이 좋다.

> 우수한 프로그래머와 설계자 라면 해법 영역과 문제 영역을 구분할 줄 알아야 한다.
