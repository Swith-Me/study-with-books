## 익스트림 프로그래밍 소개

### 익스트림 프로그램 실천방법
단순하면서도 서로 의존적인 실천방법의 집합

### 고객 팀 구성원
XP 팀의 고객은 기능 요소를 정의하고 우선순위를 매기는 개인 또는 그룹
XP 프로젝트에서는 고객은 누구든 간에 팀의 멤버이며 팀에서 일할 수 있음

고객은 개발자와 가까운 거리에 있어야 함. <br/>
그래야만 진정한 팀원이 될 수 있음. <br/>
=> 힘들다면 내 주위에 가까울 수 있는 사람, 실제 고객을 대신할 수 있고 그런 의지가 있는 사람을 찾자!

### 짧은 반복
XP프로젝트는 개발중인 소프트웨어를 2주마다 공개한다.

#### 반복계획
개발자가 세운 예산에 따라 고객이 선택한 사용자 스토리 집합
반복은 보통 2주 단위로 진행 (마이너 공개)

#### 릴리즈계획
개발자가 제시한 예산에 맞춰 고객이 선택한, 우선 순위가 정해진 사용자 스토리의 묶음으로 구성
릴리즈는 보통 3개월 동안을 의미 (메이저 공개)

### 인수 테스트 
사용자 스토리의 세부사항은 고객이 명시한 인수테스트 형태로 기록
(인수테스트는 시스템이 고객이 명시한 대로 동작하는 지 여부를 검증)

### 짝 프로그래밍
모든 운영 코드는 같은 회사에서 일하는 프로그래머 짝들에 의해 작성된다. 

짝의 한 멤버는 키보드를 잡고 코드를 입력하고, 다른 한 멤버는 입력되는 코드를 보면서 에러아 개선점을 찾는다. 
(짝의 역활과 사람을 재주 바껴야 한다.)

### 테스트 주도 개발 
모든 운영 코드는 실패하는 단위 테스트를 통과하기 위해 작성된다.
테스트 케이스와 코드를 작성하는 사이의 간격은 1분 정도로 매우 빠르다.

테스트 케이스의 완성된 본문은 코드와 함께 발전한다.  (리팩토링의 굉장히 용이)

### 공동 소유권
팀은 어떤 모듈이라도 점검하고 개선할 권리를 가진다. 
모든 팀원이 다른 사람보다 더한 권한을 갖지 않는다.

### 지속적인 통합
프로그래머는 자신의 코드를 커밋하고 하루에 몇 번씩 그것을 통합한다.
첫번째로 커밋한 사람을 우선으로 나머지 사람의 코드를 병합한다.

### 지속 가능한 속도 
소프트웨어 프로젝트는 단거리 경주가 아니라 마라톤이기에 <br/>
꾸준히 적당한 속도로 달려야한다.

### 열린 작업 공간
팀은 열린 공간에서 함께 일한다.

### 단순한 설계
* 어떻게든 동작하는 가장 단순한 것을 생각한다.
* 필요하지 않을 것이라는 가정에서 시작한다.
* 코드를 중복해서 쓰지 않는다.

### 리팩토링
코드는 부해하기 쉽기에 리팩토링을 계속 수행한다. 
리팩토링을 통해, 가능한 깔끔하고 단순하며 의미있는 코드를 유지할 수 있다.


