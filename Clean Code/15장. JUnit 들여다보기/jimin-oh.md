# 15장 JUnit 들여다보기
JUnit은 자바 프레임워크 중에서 가장 유명하다.
일반적인 프레임워크가 그렇듯 개념은 단순하며 정의는 정밀하고 구현은 우아하다. 

### JUnit 프레임워크
JUit은 저자가 많다. 시작은 켄브 벡과 에림 감마, 두 사람이다.

ComparisonCompactor.java 저자들이 모듈을 아주 좋은 상태로 남겨두었지만  <br/>
우린 보이스카우트 규칙에 따라 처음 왔을 때보다 더 깨끗하게 해놓고 떠나야 한다.
* 접두사 f를 모두 제거하자
* 의도를 명확하게 표현하기 위해 조건문을 캡슐화하자.<br/>즉 조건문을 메서드로 뽑아내 적절한 이름을 붙인다.
* 이름을 명확하게 붙인다.
* 첫 문장 if를 긍정으로 만들어 조건문을 반전한다. 
* 함수 사용방식을 일관적이게 한다.
* 숨겨진 시간적인 결함을 외부에 노출하기
* 원래 했던 변경을 되돌릴 수도 있다.


**** 
**느낀점**
* 세상에 개선이 불필요한 모듈은 없다. 우수한 모듈이라도 조금 더 깨끗하게 만들 수 있다.

