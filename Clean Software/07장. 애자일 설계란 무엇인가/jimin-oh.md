## 애자일 설계란 무엇인가.
소프트웨어 시스템의 설계는 우선적으로 소스 코드에 의해 문서화 되며, 소스 코드를 표현하는 다이어그램은 설계에서 부수적인 것일 뿐, 설계 그 자체는 아니라고 주장 -잭 리브스-

### 소프트웨어에서 어떤 것이 잘못되는가
### 설계의 악취: 부패하고 있는 소프트웨어의 냄새
1. 경직성 
시스템은 변경하기 어렵다. 변경을 하려면 시스템의 다른 부분들까지 많이 변경해야 하기 때문이다.
2. 취약성
변경을 하면 시스템에서 그 부분과 개념적으로 아무런 관련이 없는 부분이 망가진다.
3. 부동성
시스템을 다른 시스템에서 재사용할 수 있는 컴포넌트로 구분하기가 어렵다.
4. 점착성
옳은 동작을 하는 것이 잘못된 동작을 하는 것보다 더 어렵다. 
5. 불필요한 복잡성
직접적인 효용이 전혀 없는 기반구조가 설계에 포함되어 있다.
6. 불필요한 반복
단일 추상 개념으로 통합할 수 있는 반복적인 구조가 설계에 포함되어 있다.
7. 불투명성
읽고 이해하기 어렵다. 그 의도를 잘 표현하지 못한다. 

#### 무엇이 소프트웨어의 부패를 촉진하는가?
초기 설계에서 예상하지 않았던 요구사항 변경때문에 설계가 퇴화하게 된다. 
그러나 요규사항은 변경되기 마련이다. 
이런 변경에서도 탄력적인 설계를 만드는 방식을 찾아야 하고
그것이 부패하지 않도록 보호할 수 있는 방식을 사용해야 한다.

### 애자일 팀은 소프트웨어가 부패하도로고 내버려두지 않는다.
애자일 팀은 변경을 보람을 삼는다. 
시스템의 설계를 가능한 명료하고 단순하게 유지하고,  
이것을 많은 단위 테스트와 인수 테스트로 뒷받침한다. 

#### 요구사항은 언제나 변한다.

### 애자일 개발자는 해야할 일을 어덯게 알았는가
1. 그들은 애자일 실천 방법을 따라 하며 문제를 찾아냈다.
2. 그들은 설계 원칙을 적용해 문제르 진단했다.
3. 그리고 적절한 디자인 패턴을 적용해 문제를 해결했다.

### 가능한 좋은 상태로 설계 유지하기 
설계와 소스 코드는 명료한 상태로 유지되어야 한다. 
