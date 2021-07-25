✨ 제 8장, 경계 
----------------------

### 외부 코드 사용하기  

Sensor라는 객체를 담는 Map 생성 
~~~java
Map sensors = new HashMap();
~~~

Sensor라는 객체가 필요한 코드는 Sensor 객체 가져옴 
~~~java 
Sensor s = (Sensor)sensors.get(sensorId);
~~~

Map을 사용하는 클라이언트에게는 Map이 반환하는 Object를 올바를 유형으로 변환할 책임이 존재 <br>
코드는 동작 but, 깨끗한 코드로 보기 어려울 뿐만 아니라 의도가 분명히 드러나지 않음 <br>

다음 코드와 같이 제네릭스를 사용하면 코드 가독성이 크게 높아짐
~~~java
Map<String, Sensor> sensors = new HashMap<Sensor>();
...
Sensor s = sensors.get(sensorId);
~~~

하지만, 이 코드도 "Map<String, Sensor>"이 사용자에게 필요치않은 기능가지 제공한다는 문제는 해결 X <br>
다음 코드는 Map을 더 깔끔하게 사용한 코드 
~~~java
public class Sensors {
 private Map sensors = new HashMap();
 
 public Sensor getById(String id) {
  return (Sensor) sensors.get(id);
 }
 // 이하 생략
}
~~~

> * 경계 인터페이스인 Map을 Sensors 안으로 숨김
>   - MAp 인터페이스가 변해도 나머지 프로그램에는 영향 X
>   - 프로그램에 필요한 인터페이스만 제공
>   - Sensors 클래스는 설계 규칙과 비즈니스 규칙을 따르도록 강제 O 
> * __경계 인터페이스를 이용할 때, 클래스나 클래스 계열 밖으로 노출되지 않도록 주의!__
> * __Map 인스턴스를 공개 API의 인수로 넘기거나 반환값으로 사용 X__

### 경계 살피고 익히기 
> * 학습 테스트
>   - 코드를 작성해 외부 코드를 호출하는 대신 먼저 간단한 테스트 케이스를 작성해 외부코드를 익히는 방법 

<br/>

### 학습 테스틀는 공짜 이상이다  
> * API를 배워야하기 때문에 학습 테스트에 발생하는 비용 X
> * 오히려 필요한 지식만 확보하기 손쉬운 방법 

<br/>

### 아직 존재하지 않는 코드를 사용하기
> * 우리가 바라는 인터페이스 구현 
>   - 인터페이스의 전적 통제권 
>   - 높아지는 코드 가독성과 분명해지는 코드 의도 

<br/>

### 깨끗한 경계
> * 통제 못 할 코드 사용 시 많은 투자, 향후 변경 비용이 커지지 않게 주의 
> * 경계에 위치하는 코드를 깔끔히 분리 
> * 외부 패키지를 호출하는 코드를 줄여 경계 관리 
