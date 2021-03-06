## 창발적 설계로 깔끔한 코드를 구현하자

켄트 벡의 **단순한 설계 규칙**

- 모든 테스트를 실행한다.
- 중복을 없앤다.
- 프로그래머 의도를 표현한다.
- 클래스와 메서드 수를 최소로 줄인다.

이 규칙을 따르면 설계는 '단순하다'고 말이 나올 것이다.  

<br>

## 단순한 설계 규칙 1: 모든 테스트를 실행하라

먼저, 설계는 의도한 대로 돌아가는 시스템을 내놓아야 한다.

왜냐하면 의도한 대로 시스템이 돌아가는지 검증할 방법이 없다면, 그 노력과 가치는 인정받기 어렵기 때문이다.  

결합도가 높으면 테스트 케이스를 작성하기 어렵다.

하지만, "테스트 케이스를 만들고, 계속 돌려라"라는 규칙을 따르면 시스템은

**낮은 결합도**와 **높은 응집력**과 **객체 지향 방법론이 지향하는 목표**를 **달성**한다.  

✔️ 테스트 케이스를 작성하여 설계 품질을 높이자.  

<br>

## 단순한 설계 규칙 2~4: 리팩터링

코드를 점진적으로 리팩터링하고, 테스트 케이스를 돌려 기존 기능을 깨뜨리지 않았다는 사실을 확인한다.

리팩터링 단계에서는 소프트웨어 설계 품질을 높이는 다양한 방법을 동원하여 적용해도 좋다.

또한, 이 단계에서는 단순한 규칙 중, 중복 제거 / 프로그래머 의도 표현 / 클래스, 메서드 수를 최소로 줄이는 단계이기도 하다.  

<br>

## 중복을 없애라

**중복이란?**

- 추가 작업, 추가 위험, 불필요한 복잡도를 뜻해 우수한 설계에서 커다란 적이 된다.
- 똑같은 코드, 구현 중복 등 여러가지 형태로 표출된다.
- 중복 제거 시 '소규모 재사용'을 이용하면 시스템 복잡도를 극적으로 줄여주고, 더 나아가 대규모 재사용을 사용할 수 있다.

<br>

## 표현하라

소프트웨어 프로젝트 비용 중 대다수는 장기적인 유지보수에 들어간다.

시스템이 점점 복잡해지면, 유지보수 개발자는 이를 이해하는데 시간을 많이 보내며 코드를 오해할 수도 있다.

이 때문에 개발자는 코드를 명백하게 구현하여 의도를 분명히 표현해야 한다.

그러면 결함이 줄어들고, 유지보수 비용이 적게 든다.  

**코드의 의도를 명백하게 하는 방법**

1. 좋은 이름 선택
2. 가능한 작은 크기의 함수와 클래스
3. 표준 명칭 사용
4. 단위 테스트 케이스를 꼼꼼히 작성  

✔️ 하지만, 표현력을 높이는 가장 중요한 방법은 **노력**이다.

나중에 이 코드를 어떤 사람이 읽게 될 지는 모르지만, 자신일 가능성이 높다는 사실을 명심하여 작성하자.  

<br>

## 클래스와 메서드 수를 최소로 줄여라

- 중복을 제거하고, 의도를 표현하고, SRP를 준수한다는 기본적인 개념이 극단으로 치달으면 득보다 실이 많아진다.
- 클래스마다 무조건 인터페이스를 생성하면 클래스/메서드 수가 늘어난다.
- 클래스, 함수 수를 줄이는 것 < **테스트 케이스를 만들어 중복 제거, 의도 표현하는 것**

<br>

## 결론

경험을 대신할 단순한 개발 기법은 없지만, **단순한 설계 규칙**을 따른다면 우수한 기법과 원칙을 단번에 활용할 수 있다.
