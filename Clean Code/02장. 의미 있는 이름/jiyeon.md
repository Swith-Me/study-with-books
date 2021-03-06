## 2장 의미 있는 이름

### 의도를 분명히 밝혀라
* 변수(혹은 함수나 클래스)의 존재 이유는? 수행 기능은? 사용 방법은?
* 주석이 필요하다면 의ㄷ도를 분명히 드러내지 못했다는 것

#### 아무 의미도 드러나지 않은 변수
```
int d;
```

#### 의도가 드러나는 변수를 사용하여 코드 이해와 변경이 쉬워짐
```
int elapsedTimeInDays;
int daysSinceCreation;
int daysSinceModification;
int fileAgeInDays;
```
------------
### 그릇된 정보를 피하라
그릇된 단서는 코드 의미를 흐림
* 널리 쓰이는 의미가 있는 단어를 다른 의미리 사용 X
* 여러 계정을 그룹으로 묶을 때, 실제 리스트가 아니라면 사용하지 않는다
* 서로 흡사한 이름을 사용하지 않는다
* 유사한 개념은 유사한 표기법을 사용한다
* 소문자 l과 대문자 O는 가급적 사용을 자제하거나, 글꼴를 바꾸어 차이를 드러낸다
------------
### 의미 있게 구분하라
* **연속된 숫자**를 덧붙이거나 **불용어**를 추가하는 것은 적절하지 못함
------------
### 발음하기 쉬운 이름을 사용하라
* 사람들은 단어에 능숙하고, 프로그래밍은 사회활동이기 때문에 발음하기 쉬운 이름을 선택할 것
------------
### 검색하기 쉬운 이름을 사용하라
* 숫자 7이나 영어 e는 사용 자제
* 긴 이름이 짧은 이름보다 좋음
* 이름의 길이는 범위 크기에 비례해야 한다
------------
### 인코딩을 피하라
* 헝가리식 표기법 - 변수 이름에 타입을 인코딩할 필요 X
* 멤버 변수 접두어 - 멤버 변수에 접두어 붙일 필요 X, 함수와 클래스는 접두어가 필요없을 정도로 작아야 마땅함.
* 인터페이스 클래스와 구현 클래스 - 인터페이스 이름은 접두어를 붙이지 않는 편이 좋음
  클래스 이름과 구현 클래스 이름 중 하나를 인코딩 해야한다면 구현 클래스 이름이 좋다
------------
### 자신의 기억력을 자랑하지 마라
* 코드를 읽으면서 변수 이름을 자신이 아는 이름으로 변환해야 한다면 바람직하지 못한 코드
* 문자 하나만 사용하는 변수 이름은 문제가 있음.
* 전문 프로그래머는 자신의 능력을 좋은 방향으로 사용해 남들이 이해하는 코드를 내놓음
------------
### 클래스 이름
* 클래스 이름이나 객체 이름은 명사나 명사구가 적합함
* Manager, Processor, Data, Info 등과 같은 단어는 피하고, 동사는 사용하지 않는다
------------
### 메서드 이름
* 메서드 이름은 동사나 동사구가 중요하다
* 접근자, 변경자, 조건자는 표준에 따라 get, set, is를 붙인다
* 생성자를 중복정의할 때는 정적 팩토리 메서드를 사용하고, 메서드는 인수를 설명하는 이름을 사용한다.

#### 아래있는 코드보다 위에 있는 코드가 더 좋다.
``` 
Complex fulcrumPoint = Complex.FormRealNumber(23.0);
```
```
Complex fulcrumPoint = new Complex(23.0);
```
------------
### 기발한 이름은 피하라
* 이름이 너무 기발하면 저자와 유머 감각비 시슷하거나, 농담을 기억하는 동안만 기억남
* 재밌는 이름보다는 명료한 이름을 선택할 것
* 특정 문화에서만 사용되는 농담은 피하는 것이 좋음
------------
### 한 개념에 한 단어를 사용하라
* 메서드 이름은 독자적이고 일관적일 것
------------
### 말장난을 하지 마라
* 한 단어를 두 가지 목적으로 사용하지 말 것
* 프로그래머는 집중적인 탐구가 필요한 코드가 아니라 대충 훑어봐도 이해할 코드 작성이 목표
------------
### 해법 영역에서 가져온 이름을 사용하라
* 전산 용어, 알고리즘 이름, 패턴 이름, 수학 용어를 사용해도 괜찮음
* 모든 이름을 문제 영역에서 가져오는 정책은 현명하지 못함
* 기술 개념에는 기술 이름이 가장 적합한 선택
------------
### 문제 영역에서 가져온 이름을 사용하라
* 우수한 프로그래머라면 해법 영역과 문제 영역을 구분할 줄 ㅇ라아야한다.
------------
### 의미 있는 맥락을 추가하라
* 클래스, 함수, 이름 공간에 넣어 맥락을 부여하고, 모든 방법이 실패하면 접두어를 붙인다
------------
### 불필요한 맥락을 없애라
* 이름이 분명한 경우에 한해서 짧은 이름보다 긴 이름이 좋음
