# 9장 단위테스트

## TDD 법칙 세가지
* **첫째 법칙:**  실패하는 단위 테스트를 작성할 떄까지 실제 코드를 작성하지 않는다.
*  **둘쨰 법칙:** 컴파일은 실패하지 않으면서 실행이 실패하는 정도로만 단위 테스트를 작성한다.
*  **셋째 법칙:** 현재 실패하는 테스트를 통과할 정도로만 실제 코드를 작성한다.

## 깨끗한 테스트 코드 유지하기
 **테스트 코드는 실제 코드 못지 않게 중요하다** <br/>
 테스트는 유연성, 유지보수성, 재사용성을 제공한다.
 * 테스트 케이스가 없으면 모든 변경이 잠정적인 버그다.
 * 테스트 케이스가 있으면 변경이 쉬워진다. 
 * 테스트 코드가 지저분하면 실제 코드도 지저분해진다. <br/>
 코드에 유연성, 유지보수성, 재사용성을 제공하는 버팀목이 바로 **단위 테스트**다.
 
## 깨끗한 테스트 코드
> **가독성**은 테스트 코드에 굉장히 중요하다.
테스트 코드에 가독성을 높이려면 명료성, 단순성, 풍부한 표현력이 필요하다.
#### 도메인에 특화된 테스트 언어
테스트를 구현하는 당사자와 나중에 테스트를 읽어볼 독자를 도와주는 **테스트 언어**다
#### 이중 표준
테스트 API코드에 적용하는 표준은 실제 코드에 적용하는 표준과 확실히 다르다.
* 실제 코드만큼 효율적인 필요는 없다.
* 실제 환경에서는 절대 안 되지만 테스트 환경에서는 전혀 문제없는 방식이 있다.

## 테스트 당 assert 하나
* assert 문 개수는 최대한 줄여야 좋다.
* 테스트 함수마다 한 개념만 테스트 하라

## F.I.R.S.T
* **빠르게** Fast <br/>
테스트는 빨리 돌아야 한다. <br/>
테스트가 느리면 자주 돌릴 엄두를 못낸다.<p/>
* **독립적으로** Independent <br/>
각 테스트는 서로 의존하면 안 된다.<br/>
각 테스트는 독립적으로, 어떤 순서로 실행해도 괜찮아야한다.<p/>
* **반복가능하게** Repeatable<br/>
테스트는 어떤 환경에서도 반복 가능해야 한다. <p/>
* **자가검증하는** Self-Validating <br/>
테스트는 bool 값으로 결과를 내야한다(성공 아니면 실패다)<p/>
* **적시에** Timely <br/>
테스트는 적시에 작성해야 한다.
단위 테스트는 테스트하려는 실제 코드를 구현하기 직전에 구현한다.

* * *
#### **느낀점**
* 깨끗한 테스트 코드에 대해 생각해본적이 없는것같다. 
* 테스트 코드가 방치되어 망가지면 실제 코드도 망가진다. 라는 말이 테스트 코드를 유지해야겠다는 결심을 만들었다.
