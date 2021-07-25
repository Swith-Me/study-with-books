# 8. 경계🎭

### 외부 코드 사용하기

☑**인터페이스 제공자와 인터페이스 사용자 사이에는 특유의 긴장이 존재한다.**

- 패키지 제공자나 프레임워크 제공자는 적용성을 최대한 넓히려 애쓴다.
- Map은 굉장히 다양한 인터페이스로 수많은 기능을 제공한다.

Sensor라는 객체를 담는 Map를 생성한다.

```python
Map sensors = new HashMap();
```

Sensor객체가 필요한 코드는 다음과 같이 Sensor객체를 가져온다.

```python
Sensor s = (Sensor)sensors.get(sensorId);
```

가독성을 높인다.

```python
Map<String, Sensor> sensors = new HashMap<Sensor>();
'''
Sensor s = sensors.get(sensorId);
```

이 코드를 더 보완하면

```python
public class Sensors {
  private Map sensors = new HashMap();

  public Sensor getById(String id) {
    return (Sensor)sensors.get(id);
  }

  // 이하 생략
}
```

### 경계 살피고 익히기

☑**짐 뉴커크 학습 테스트**

- 간단한 테스트 케이스를 작성해 외부코드를 익힌다.
- 학습테스트는 API를 사용하려는 목적에 초점을 맞춘다.

### 학습 테스트는 공짜 이상이다.

☑**학습 테스트에 드는 비용은 없다.**

- 투자하는 노력보다 성과가 더 크다.
- 어쨌든 API를 배워야 하므로, 오히려 필요한 지식만 확보하는 손쉬운 방법이다.

### 아직 존재하지 않는 코드를 사용하기

- 경계와 관련해 또 다른 유형은 아는 코드와 모르는 코드를 분리하는 경계다.

### 깨끗한 경계

☑**경계에서는 흥미로운 일이 많이 벌어진다.**

- 소프트웨어 설계가 우수하다면 변경하는데 많은 투자와 재작업이 필요하지 않다.
- 경계에 위치하는 코드는 깔끔히 분리한다.
- 우리가 원하는 인터페이스를 패키지가 제공하는 인터페이스로 변환한다.

### 기억에 남는 구절 📖

> 지정한 주파수를 이용해 이 스트림에서 들어오는 자료를 아날로그 신호로 전송하라.
