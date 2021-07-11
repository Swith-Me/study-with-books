✨ 제 2장, 의미있는 이름
----------------------

#### 의도를 분명히 밝혀라 
> 좋은 이름을 지으려는 시간 < 좋은 이름을 지음으로써 절약되는 시간 

> 변수나 함수, 클래스 이름들에게 해야할 질문 <br>
> * 존재 이유? <br>
> * 수행 기능? <br>
> * 사용 방법? <br>

아무 의미도 드러나지 않는 이름 d
~~~java
int d; //경과 시간 (단위: 날짜)
~~~

명확한 의미를 가진 이름을 사용 -> 코드 이해와 변경에 유용 
~~~java
int elapsedTimeInDays;
int daysSinceCreation;
int daySinceModification;
int fileAgeInDays;
~~~

#### 그릇된 정보를 피하라 
> 널리 쓰이는 의미가 있는 단어는 다른 의미로 사용 X <br>
> * EX) hp, aix, sco 는 변수 이름으로 적합하지 않음 -> 유닉스 플랫폼, 유닉스 변종을 가르키는 이름이기 때문 <br>
>   직각삼각현의 빗변을 구현할 때, hp는 훌륭한 약어로 보일지라도 hp라는 변수는 독자에게 그릇된 정보를 제공
> 서로 흡사한 이름을 사용 X
>  * EX) 'XYZControllerForEfficientHandlingOfStrings' | 'XYZConrollerForEfficientStorageOfStrings'
> 유사한 개념은 유사한 표기법을 사용 

#### 의미 있게 구분하라 
> 연속된 숫자를 덧붙이거나 불용어를 추가하는 방식은 금지 

#### 발음하기 쉬운 이름을 사용하라
> 프로그래밍은 사회 활동, 발음하기 어려운 이름은 토론하기도 어렵 

#### 검색하기 쉬운 이름을 사용하라 
> 이름 길이는 범위 크기에 비례 <br> 
> 변수나 상수를 코드 여러 곳에 사용한다면, 검색하기 쉬운 이름이 바람직 <br>

WORK_DAYS_PER_WEEK 찾기 쉬움 
~~~java
int realDaysPerIdealDay = 4;
const int WORK_DAYS_PER_WEEK = 5;
int sum = 0;
~~~

#### 인코딩을 피하라 
> 헝가리식 표기법 <br>
>  * 변수 이름에 타입을 인코딩 할 필요 X <br>
>  * IDE는 코드를 컴파일하지 않고 타입오류를 감지할 정도로 발전 <br>
>    -> 헝가리식 표기법, 기타 인코딩 방식이 오히려 방해 
> 멤버 변수 접두어 <br>
>  * 'm_' 이라는 접두어를 붙일 필요 X <br>
> 인터페이스 클래스와 구현 클래스 <br>
>  * 인터페이스 이름은 접두어를 붙이지 않는 것이 베스트 <br>

#### 자신의 기억력을 자랑하지 마라
> 명료함이 최고라는 사실을 이해, 남들이 이해하는 코드 

#### 클래스 이름 
> 클래스 이름, 객체 이름은 명사나 명사구가 적합 <br>
> O ; 'Customer' | 'WikiPage' | 'Accoount' | 'AddressParser' <br>
> X ; 'Manager' | 'Processor' | 'Data' | 'Info' | '동사' <br>


















