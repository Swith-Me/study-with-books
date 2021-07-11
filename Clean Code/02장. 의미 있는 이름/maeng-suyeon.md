## 의도를 분명히 밝혀라

- 좋은 이름으로 인해 `이름을 짓는 시간 < 절약하는 시간`의 효과를 볼 수 있다.
- 의도가 불분명할 때 오는 질문들
    1. 변수 또는 함수 또는 클래스의 존재 이유는?
    2. 수행 기능은?
    3. 사용 방법은?

    ```java
    int d;    // 경과 시간 (단위 : 날짜)
    ```
    이름 `d`는 아무 의미도 드러나지 않는다. 경과 시간이나 날짜라는 느낌이 들지 않아 측정하려는 값과 단위를 표현하는 이름이 필요하다.

    ```java
    int elapsedTimeInDays;
    int daysSinceCreation;
    int daysSinceModification;
    int fileAgeInDays;
    ```

    이처럼 의도가 드러나는 이름을 사용하면 코드 이해와 변경이 쉬워진다.

    다음 코드는 무엇을 할까?

    ```java
    public List<int[]> getThem() {
    	List<int[]> list1 = new ArrayList<int[]>();
    	for (int[] x : theList)
    		if(x[0] == 4)
    			list1.add(x)
    	return list1;
    }
    ```

    이러한 코드의 문제점은 단순성이 아닌 코드의 **함축성**이다.

    ⇒ 코드 맥락이 코드 자체에 명시적으로 드러내지 않는 것

    1. theList에 무엇이 들어있는가?
    2. theList에서 0번째 값이 어째서 중요한가?
    3. 값 4는 무슨 의미인가?
    4. 함수가 반환하는 리스트 list1을 어떻게 사용하는가?

    이와 같은 정보가 위 코드 예제에선 드러나지 않지만, 정보 제공은 **충분히 가능했었다.**

    이 코드를 지뢰찾기 게임으로 바꿔보면...

    ```java
    public List<int[]> getFlaggedCells() {
    	List<int[]> flaggedCells = new ArrayList<int[]>();
    	for (int[] cell : gameBoard)
    		if (cell[STATUS_VALUE == FLAGGED)
    			flaggedCells.add(cell);
    	return flaggedCells;
    }
    ```
    앞서 한 예제 코드의 단순성과 들여쓰기 단계가 동일하지만, 코드는 더욱 명확해진 것을 볼 수 있다.

    이 코드를 한 단계 더 발전시켜보면...

    ```java
    public List<Cell> getFlaggedCells() {
    	List<Cell> flaggedCells = new ArrayList<Cell>();
    	for (Cell cell : gameBoard)
    		if (cell.isFlagged())
    			flaggedCells.add(cell);
    	return flaggedCells;
    }
    ```
    int 배열 대신 isFlagged라는 좀 더 명시적인 함수를 사용해 FLAGGED 상수를 감춰, 더 간단한 클래스로 바꾸었다.

    ✔ 이처럼 단순히 이름만 고쳤는데도, 함수가 하는 일을 이해하기 쉬워졌다. 바로 이것이 좋은 이름이 주는 위력이다.

## 그릇된 정보를 피하라

- 그릇된 단서는 코드 의미를 흐린다.
- 나름대로 널리 쓰이는 의미가 있는 단어를 다른 의미로 사용해선 안된다.

    ex- hp, aix, sco는 변수 이름으로 적합하지 않다. (유닉스 플랫폼 & 유닉스 변종을 가리키는 이름)

    ex- 실제 List가 아닐 때 accountList로 명명하는 것 ⇒ accountGroup, bunchOfAccounts, Account로 명명하자.

- 서로 흡사한 이름을 사용하지 않도록 주의한다.
    - 유사한 개념은 유사한 표기법 사용 ⇒ **정보**
    - 일관성이 떨어지는 표기법 ⇒ **그릇된 정보**
- 각각의 개념 차이가 명백히 드러난다면 코드 자동 완성 기능은 굉장히 유용해진다.
    - 그릇된 정보를 제공하는 끔찍한 예

        ```java
        int a = 1;
        if (O == 1)
        a = Ol;
        else
        l = 01;
        ```
        소문자 L과 대문자 O를 같이 사용하여 소문자 L은 숫자 1처럼, 대문자 O는 숫자 0처럼 보인다.

        ⇒ 글꼴을 바꿔 이런 문제를 해결하자.

## 의미 있게 구분하라

- 컴파일러 or 인터프리터만 통과하려는 생각으로 코드 구현 ⇒ 문제 발생

    ex- 동일한 범위 안에서 다른 두 개념에 같은 이름 사용 X

    - 컴파일러를 통과할 지라도 연속된 숫자를 덧붙이거나 불용어(noise word)를 추가하는 방식은 적절치 X ⇒ 이름이 달라야 한다면 의미도 달라져야 O

    ```java
    public static void copyChars(char a1[], char a2[]){
    	for (int i=0; i<a1.length; i++)
    		a2[i] = a1[i];
    	}
    }
    ```
    함수 인수 이름으로 source와 destination을 사용한다면 코드 읽기가 훨씬 더 쉬워진다.

    - 불용어를 추가한 이름 역시 아무런 정보 제공 X

        ex- Info, Data는 a, an, the와 마찬가지로 의미가 불분명한 불용어

        - a나 the와 같은 접두어를 사용하지 말라는 것은 X 의미가 분명히 다르다면 사용해도 무방하다.
        - 불용어는 중복이다. 변수 이름에 variable이라는 단어는 단연코 금물! (표 이름에 table도 마찬가지)

        ```java
        getActiveAccount();
        getActiveAccounts();
        getActiveAccountInfo();
        ```
        이 프로젝트에 참여한 프로그래머는 어느 함수를 호출할 지 어떻게 알까?

        ✔ 이처럼 읽는 사람이 차이를 알도록 이름을 짓자.

## 발음하기 쉬운 이름을 사용하라

- 발음하기 어려운 이름은 토론하기도 어렵다.
- 프로그래밍은 **사회 활동**이기 때문에 발음하기 쉬운 이름은 중요하다.

    ```java
    class DtaRcrd102 {
    	private Date genymdhms;
    	private Date modymdhms;
    	private final String pszqint = "102";
    	/* ... */
    };

    // 와

    class Customer {
    	private Date generationTimestamp;
    	private Date modificationTimestamp;
    	private final String recordId = "102";
    	/* ... */
    };
    ```
    이렇게 밑의 코드로 변수명을 바꾸면 지적인 대화가 가능해진다.

## 검색하기 쉬운 이름을 사용하라

- 문자 하나를 사용하는 이름과 상수는 텍스트 코드에서 쉽게 눈에 띄지 않는다.

    ex- 숫자 7과 영어 e , 둘 다 자주 쓰이는 문자이고, 검색 시 모든 파일 이름, 수식이 검색된다.

- 이를 해결하기 위해 간단한 메서드에서 로컬 변수만 한 문자를 사용하는 것이다.
    - **단, 이름 길이는 범위 크기에서 비례해야 한다.**

    ```java
    for (int j=0; j<34; j++){
    	s += (t[j]*4)/5;
    }

    // 와

    int realDaysPerIdealDay = 4;
    const int WORK_DAYS_PER_WEEK = 5;
    int sum = 0;
    for (int j=0; j<NUMBER_OF_TASKS; j++) {
    	int realTaskDays = taskEstimate[j] * realDaysPerIdealDay;
    	int realTaskWeeks = (realTeskDays / WORK_DAYS_PER_WEEK);
    	sum += realTaskWeeks;
    }
    ```
    위 코드에서 sum이 별로 유용하지 않지만, 최소한 검색이 가능하다.

    ✔ 이름을 의미 있게 지으면 함수가 길어지지만, 검색 시 찾기가 수월하다.

## 인코딩을 피하라

- 인코딩한 이름은 발음하기 어렵고, 해독하기 어렵다.

    ⇒ 문제 해결에 집중하는 개발자에게 인코딩은 불필요한 정신적 부담.

- 헝가리식 표기법

    : 컴퓨터 프로그래밍에서 변수나 함수의 이름에 그 종류, 곧 흔히 데이터 타입 따위를 명시하는 표기법

- 멤버 변수 접두어

    : 멤버 변수에 m_이라는 접두어 생략

- 인터페이스 클래스와 구현 클래스

    : 인터페이스 클래스 앞에 접두어 I를 붙이는 것보다는 구현 클래스 이름을 인코딩하자.

## 자신의 기억력을 자랑하지 마라

- 코드를 읽으면서 변수 이름을 자신이 아는 이름으로 변환해야 한다면, 그 변수 이름은 바람직하지 못하다.

    ⇒ 문제 영역 or 해법 영역에서 사용하지 않는 이름을 선택하여 생기는 문제.

- 전문가 프로그래머는 **명료함이 최고**다.

    → 자신의 능력을 좋은 방향으로 사용해 남들이 이해하는 코드를 내놓는다.

## 클래스 이름

- 클래스 / 객체 이름은 `명사` or `명사구`가 적합하다.

    좋은 예  Customer, WikiPage, Account  등

    나쁜 예  Manager, Processor, Data, Info 등

- 동사는 사용하지 않는다.

## 메서드 이름

- 메서드 이름은 `동사` or `동사구`가 적합하다.

    ex- postPayment, deletePage, save 등

- 접근자(Accessor), 변경자(Mutator), 조건자(Predicate)는 [javabean 표준](https://ko.wikipedia.org/wiki/%EC%9E%90%EB%B0%94%EB%B9%88%EC%A6%88)에 따라 값 앞에 `get`, `set`, `is` 를 붙인다.

    ```java
    string name = employee.getName();
    customer.setName("mike");
    if (paycheck.isPosted())...
    ```

- 생성자(Constructor)를 중복정의(overload)할 때는 정적 팩토리 메서드를 사용한다.
    - 메서드는 인수를 설명하는 이름을 사용한다.

        ```java
        Complex fulcrumPoint = Complex.FromRealNumber(23.0);

        // 위 코드가 아래 코드보다 좋다.

        Complex fulcrumPoint = new Complex(23.0);
        ```
        생성자 사용을 제한하려면 해당 생성자를 private로 선언한다.

## 기발한 이름은 피하라

- 재미난 이름보다 명료한 이름을 선택해라.
- 특정 문화에서만 사용하는 농담은 피하는 편이 좋다.

    ex- kill() 대신 whack() / Abort() 대신 eatMyShort()

✔ 의도를 분명하고 솔직하게 표현해라.

## 한 개념에 한 단어만 사용하라

- 추상적인 개념 하나에 단어 하나를 선택해 이를 고수한다.

✔ 메서드 이름은 **독자적**이고 **일관적**이어야 한다.

## 말장난을 하지 마라

- 한 단어를 두 가지 목적으로 사용하지 마라.
    - 다른 개념에 같은 단어를 사용한다면 그것은 말장난에 불과하다.
- 프로그래머는 집중적인 탐구가 필요한 코드가 아닌, 대충 훑어봐도 이해할 코드 작성이 목표다.

## 해법 영역에서 가져온 이름을 사용하라

- 코드를 읽을 사람도 프로그래머라는 사실을 명심하자.
    - 전산 용어, 알고리즘 이름, 패턴 이름, 수학 용어 등을 사용해도 괜찮다.
- 모든 이름을 문제 영역에서 가져오는 정책은 현명하지 않다.

✔ 기술 개념에는 기술 이름이 가장 적합한 선택이다.

## 문제 영역에서 가져온 이름을 사용하라

- 적절한 '프로그래머 용어'가 없다면, 문제 영역에서 이름을 가져온다.
- 해법 영역과 문제 영역을 구분할 줄 알아야한다.

## 의미 있는 맥락을 추가하라

- 스스로 의미가 분명한 이름은 없지 않지만, 대다수 이름은 그렇지 못하다.
    - 클래스, 함수, 이름 공간에 넣어 맥락을 부여한다.
    - 모든 방법이 실패하면 마지막 수단으로 `접두어`를 붙인다.

## 불필요한 맥락을 없애라

- 짧은 이름이 긴 이름보다 좋다.
    - 단, 의미가 분명한 경우에 한해서 좋다는 거다. 이름에 불필요한 맥락을 추가하지 않도록 주의한다.

## 마치면서

- 좋은 이름을 선택하려면 설명 능력이 뛰어나야 하고, 문화적인 배경이 같아야 한다.
- 좋은 이름을 선택하는 능력은 기술, 비즈니스, 관리 문제가 아닌 교육 문제다.
- 우리들 대다수는 자신이 짠 클래스 이름과 메서드 이름을 모두 암기하지 못한다.
- 다른 사람이 짠 코드를 손 본다면 리팩터링 도구를 사용해 문제 해결 목적으로 이름을 개선하라. 단기적인 효과는 물론 장기적인 이익도 보장한다.
