## 9. 단위테스트⏲

### TDD 법칙 세 가지
1. 실패하는 단위 테스트를 작성할 때까지 실제 코드를 작성하지 않는다.
2. 컴파일은 실패하지 않으면서 실행이 실패하는 정도로만 단위 테스트를 작성한다.
3. 현재 실패하는 테스트를 통과할 정도로만 실제 코드를 작성한다.

이렇게 일하면 실제 코드를 사실상 전부 테스트하는 테스트 케이스가 나온다.  
하지만 실제 코드와 맞먹을 정도로 방대한 테스트 코드는 심각한 관리 문제를 유발하기도 한다.

### 깨끗한 테스트 코드 유지하기
> 테스트는 유연성, 유지보수성, 재사용성을 제공한다.
- 테스트 코드는 실제 코드 못지 않게 중요하다.
- 실제 코드 못지 않게 깨끗하게 짜야 한다.
- 테스트 코드를 깨끗하게 유지하지 않으면 결국은 잃어버린다.
- 테스트 코드가 지저분하면 코드를 변경하는 능력이 떨어지고, 코드 구조를 개선하는 능력도 떨어진다.

### 깨끗한 테스트 코드
> 가독성, 가독성, 가독성 => 깨끗한 코드를 만들기 위한 것
- 명료성, 단순성, 풍부한 표햔력이 필요하다.

### F.I.R.S.T
> 깨끗한 테스트는 아래 5가지 규칙을 지켜야 한다.

**Fast** : 테스트는 빨라야 한다. <br />
**Independent** : 각 테스트는 서로 의존하면 안 된다.<br />
**Repeatable** : 테스는 어떤 환경에서도 반복 가능해야 한다.<br />
**Self-Validating** : 테스트는 부울(bool) 값으로 결과를 내야 한다.<br />
**Timely** : 테스트는 적시에 작성해야 한다.

### 결론
- 테스트 코드는 지속적으로 깨끗하게 유지하자.
- 테스트 코드가 방치되어 망가지면 실제 코드도 망가진다.

### 느낀점
- 테스트코드의 중요성을 더욱 깨닫게 되었다.
- 훗날 테스트코드를 작성할 때 유용하게 사용될 것같다.
