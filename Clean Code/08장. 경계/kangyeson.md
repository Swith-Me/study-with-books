# 08. 경계

> **목표**✔ <br>
> 소프트웨어 경계(외부 코드와 내 코드)를 깔끔하게 처리하는 기법과 기교 익히기
<br>

## I. 외부 코드 사용하기
```java
// 경계 인터페이스인 Map을 Sensors 안으로 숨긴다. 
// ➔ 따라서 Map 인터페이스가 변하더라도 나머지 프로그램에는 영향을 미치지 않는다.

public class Sensors {
  private Map sensors = new HashMap();
  
  public Senor getById(String id) {
    return (Sensor) sensors.get(id);
  }
  // 생략
}
```

- 경계 인터페이스를 이용할 떄는 이를 이용하는 클래스나 클래스 계열 밖으로 노출되지 않도록 주의한다.
- 캡슐화 등의 방법을 사용하여 경계 인스턴스를 공개 API의 인수로 넘기거나 반환값으로 사용하지 않는다.

<br><br>

## II. 경계 살피고 익히기
> 외부 코드를 익히기는 어렵다. 외부 코드를 통합하기도 어렵다. 두 가지를 동시에 하기는 두 배나 어렵다.

 ➔ 먼저 테스트 케이스를 작성해 외부 코드를 익히고 시작하라. 즉, **학습 테스트**를 이용하라

#### ❓ 학습 테스트
* 프로그램에서 사용하려는 방식대로 외부 AIP 호출
  * 통제된 환경에서 API를 제대로 이해하는지 확인
* API를 사용하려는 **목적**에 집중

<br><br>

## III. log4j 익히기
로깅 기능을 직접 구현하는 대신 아파치의 log4j 패키지를 사용할 수 있다.

<br><br>

## IV. 학습 테스트는 공짜 
> 학습 테스트에 드는 비용은 없다. 어쨌든 AIP를 배워야 하므로, 오히려 필요한 지식만 확보하는 손쉬운 방법이다.

투자하는 노력보다 얻는 성과가 더 크다. 새 버전의 패키지에 버그가 없는지, 이전과 같이 호환되는지를 편리하게 알아낼 수 있다.<br>
➔ 패키지의 새 버전으로 이전하기 쉬워지고, 필요 이상으로 낡은 버전을 사용하지 않을 수 있다.

<br><br>

## V. 아직 존재하지 않는 코드를 사용하기
> 경계와 관련해 또 다른 유형은 아는 코드와 모르는 코드를 **분리**하는 경계다.

지식이 없어 구현할 수 없는 코드 영역이 있는 경우, 우선 **우리가 바라는 인터페이스**를 구현하라.
- 인터페이스를 전적으로 통제
- 높은 코드 가독성
- 분명한 코드 의도 

<br><br>

## VI. 깨끗한 경계
> 통제하지 못하는 코드를 사용할 때는 너무 많은 투자를 하거나 향후 변경 비용이 지나치게 커지지 않도록 각별히 주의해야 한다.

- 경계에 위치한 코드는 깔끔히 **분리**
- 기대치를 정의하는 **테스트 케이스** 작성
- 통제가 불가능한 외부 패키지에 의존❌ 우리 코드에 의존⭕
- 외부 패키지 호출 코드 간략화
  - 새로운 클래스로 경계 감싸기
  - **ADAPTER 패턴** 사용하여 인터페이스를 패키지가 제공하는것으로 변환

###### ❓ ADAPTER 패턴 | 한 클래스의 인터페이스를 클라이언트에서 사용하고자하는 다른 인터페이스로 변환하는 것
<br>




