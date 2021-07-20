시스템에 들어가는 모든 소프트웨어를 직접 개발하는 경우는 드물다.

- 패키지를 사거나, 오픈 소스를 이용하거나, 사내 다른 팀이 제공하는 컴포넌트를 사용한다.

## 외부 코드 사용하기

- 인터페이스 제공자와 인터페이스 사용자 사이에는 특유의 긴장이 존재한다.
- 패키지/프레임워크 제공자는 더 많은 환경에서 돌려야 더 많은 고객이 구매하기 때문에, 적용성을 최대한 넓히려 한다.
- 반면, 사용자는 자신의 요구에 집중하는 인터페이스를 바란다.

이런 긴장으로 인해 시스템 경계에서 문제가 생길 소지가 많다.

|Map이 제공하는 메서드|
|-----------------|
clear() void – Map
containsKey(Object key) boolean – Map
containsValue(Object value) boolean – Map
clear() void – Map
containsKey(Object key) boolean – Map
containsValue(Object value) boolean – Map
entrySet() Set – Map
equals(Object o) boolean – Map
get(Object key) Object – Map
getClass() Class<? extends Object> – Object
hashCode() int – Map
isEmpty() boolean – Map
keySet() Set – Map
notify() void – Object
notifyAll() void – Object
put(Object key, Object value) Object – Map
putAll(Map t) void – Map
remove(Object key) Object – Map
size() int – Map
toString() String – Object
values() Collection – Map
wait() void – Object
wait(long timeout) void – Object
wait(long timeout, int nanos) void – Object

예제

```java
// Map 생성
Map sensors = new HashMap();

// Sensor 객체를 가져와 사용
Sensor s = (Sensor)sensors.get(sensorId);

// => 위 두 코드를 제네릭스(Generics)을 사용하여 코드 가독성을 높이자.
Map<String, Sensor> sensors = new HashMap<Sensor>();
...
Sensor s = sensors.get(sensorId);

// 제네릭스를 사용하지 않고도 캡슐화를 사용해 깔끔하게 사용할 수 있다.
public class Sensors {
	private Map sensors = new HashMap();

	public Sensor getById(String id) {
		return (Sensor) sensors.get(id);
	}
	
	// 이하 생략
}
```

## 경계 살피고 익히기

외부 코드를 사용하면 적은 시간에 더 많은 기능을 출시하기 쉬워진다.

만약, 외부에서 가져온 패키지를 사용하고 싶다면? 어디서 부터 시작해야 될까?

- 타사 라이브러리를 가져와 문서를 읽으며 사용법을 결정한다.
- 그 다음 우리쪽 코드를 작성해 라이브러리가 예상대로 동작하는지 테스트한다.
- 그리고, 디버깅을 통해 발생한 버그를 찾아낸다...

외부 코드를 익히기도, 통합하기도 어렵지만, **간단한 테스트 케이스**를 작성해 외부 코드를 익힐 수 있다.

이를 **짐 뉴커크; Jim Newkirk**는 **학습 테스트**라 부른다.

학습 테스트는 

- 프로그램에서 사용하려는 방식대로 외부 API를 호출해 **통제된 환경**에서 API를 제대로 이해하는지 확인
- **API 사용이** 목적

## log4j 익히기

```java
		// 1.
    // 우선 log4j 라이브러리를 다운받고,
    // "hello"가 출력되도록 아래의 테스트 코드를 작성해보자.
    @Test
    public void testLogCreate() {
        Logger logger = Logger.getLogger("MyLogger");
        logger.info("hello");
    }

    // 2.
    // 위 테스트 케이스는 "**Appender** 라는게 필요하다"는 오류가 발생한다.
    // 문서를 조금 더 읽어보니 **ConsoleAppender**라는 클래스가 있었다.
    // 그래서 **ConsoleAppender**라는 객체를 생성해 다시 테스트 케이스를 돌린다.
    @Test
    public void testLogAddAppender() {
        Logger logger = Logger.getLogger("MyLogger");
        ConsoleAppender appender = new ConsoleAppender();
        logger.addAppender(appender);
        logger.info("hello");
    }

    // 3.
    // 위와 같이 하면 "**Appender**에 출력 스트림이 없다"고 한다.
    // 이상하다. 가지고 있는게 이성적일것 같은데...
    // 구글의 도움을 빌려, 다음과 같이 해보았다.
    @Test
    public void testLogAddAppender() {
        Logger logger = Logger.getLogger("MyLogger");
        logger.removeAllAppenders();
        logger.addAppender(new ConsoleAppender(
            new PatternLayout("%p %t %m%n"),
            ConsoleAppender.SYSTEM_OUT));
        logger.info("hello");
    }
    
    // 성공했다. 하지만 **ConsoleAppender**를 만들어놓고 **ConsoleAppender.SYSTEM_OUT**을 받는건 이상하다.
    // 그래서 빼봤더니 잘 돌아간다.
    // 하지만 **PatternLayout**을 제거하니 돌아가지 않는다.
    // 그래서 문서를 살펴봤더니 "**ConsoleAppender**의 기본 생성자는 **unconfigured(설정되지 않은)** 상태"란다.
    // 명백하지도 않고 실용적이지도 않다... **log4j** 버그이거나, 적어도 일관성 부족으로 여겨진다.
```

목록 8-1 LogTest.java

```java
// 조금 더 구글링, 문서 읽기, 테스트를 거쳐 log4j의 동작법을 알아냈고 그것을 간단한 유닛테스트로 기록했다.
// 이제 이 지식을 기반으로 log4j를 래핑하는 클래스를 만들수 있다.
// 나머지 코드에서는 log4j의 동작원리에 대해 알 필요가 없게 됐다.

public class LogTest {
    private Logger logger;
    
    @Before
    public void initialize() {
        logger = Logger.getLogger("logger");
        logger.removeAllAppenders();
        Logger.getRootLogger().removeAllAppenders();
    }
    
    @Test
    public void basicLogger() {
        BasicConfigurator.configure();
        logger.info("basicLogger");
    }
    
    @Test
    public void addAppenderWithStream() {
        logger.addAppender(new ConsoleAppender(
            new PatternLayout("%p %t %m%n"),
            ConsoleAppender.SYSTEM_OUT));
        logger.info("addAppenderWithStream");
    }
    
    @Test
    public void addAppenderWithoutStream() {
        logger.addAppender(new ConsoleAppender(
            new PatternLayout("%p %t %m%n")));
        logger.info("addAppenderWithoutStream");
    }
}
```

## 학습 테스트는 공짜 이상이다

- 비용이 들지 않는다.
- 필요한 **지식**만 **확보**하는 손쉬운 방법이다.
- **이해도**를 높여주는 **정확한 실험**이다.
- 투자하려는 노력 < **얻는 성과**
- 패키지 새 버전이 나온다면 학습 테스트를 돌려 차이가 있는지 확인한다.
    - 패키지가 예상대로 도는지 검증한다.
    - 패키지 작성자는 버그 수정과 기능 추가를 한다.
    - 새로운 패키지 버전이 나올 때마다 새로운 위험이 생겨 우리 코드와 호환되지 않으면 이 사실을 곧바로 밝혀낸다.
- 실제 코드와 동일한 방식으로 인터페이스를 사용하는 **테스트 케이스**가 **필요**하다.

## 아직 존재하지 않는 코드 사용하기

- 경계의 또다른 유형은 아는 코드와 모르는 코드를 **분리하는 경계**다.
- 우리가 바라는 인터페이스를 구현하면, **인터페이스**를 전적으로 **통제**할 수 있고, **코드 가독성**이 **높아지고**, **코드 의도**가 **분명**해진다.

## 깨끗한 경계

- 경계에는 변경과 같은 흥미로운 일이 많이 벌어진다.
- 소프트웨어 설계가 우수한다면 변경하는데 많은 투자와 재작업이 필요하지 않다.
- 통제하지 못하는 코드를 사용할 때는 너무 많은 투자를 하거나 향후 변경 비용이 지나치게 커지지 않도록 각별히 주의해야 한다.
- 경계에 위치하는 코드는 깔끔히 **분리**하고, 기대치를 정의하는 **테스트 케이스**도 작성한다.
- 통제 불가능한 외부 패키지에 의존하는 것보단 **통제 가능한** 우리 코드에 의존하는 편이 훨씬 좋다.
- 외부 패키지를 호출하는 코드를 가능한 줄여 경계를 관리하자.
    - 새로운 클래스로 경계를 감싸거나 **ADAPTER 패턴**을 사용해 패키지가 제공하는 인터페이스로 변환하자.
    - 그러면 코드 가독성과 일관성이 높아지고, 외부 패키지가 변했을 때 변경할 코드도 줄어든다.
