✨ 제 6장, 객체와 자료 구조 
----------------------

### 자료 추상화  
> * 변수 사이에 함수라는 계층을 넣는다고 구현이 저절로 감춰지지 X
>   - 추상화를 해야함 
> * 조회 함수와 설정 함수로 변수를 다룬다고 클래스가 되지는 X
>   - 추상 인터페이스를 제공해 사용자가 구현을 모른 채 자료의 핵심을 조절할 수 있어야 진정한 의미의 클래스

<br/>

### 자료/객체 비대칭
> * 객체 ; 춧상화 뒤로 자료를 숨긴 채 자료를 다루는 함수만 공개
> * 자료 구조 ; 자료를 그대로 공개하며 별다른 함수 비제공
> * 절차지향코드
>   - 기존 자료구조 변경하지 않고 새 함수 추가하기 쉬움
>   - 새로운 자료구조 추가가 어려움 
> * 객체지향코드
>   - 기존 함수를 변경하지 않으면, 새 클래스 추가가 어려움 
>   - 새료운 함수 추가가 어려움 

<br/>

### 디미터 법칙 
> * 모듈은 자신이 조작하는 객체의 속사정을 몰라야한다는 법칙 
> * "클래스 C의 메서드 f는 다음과 같은 객체의 메서드만 호출해야 한다"
>   - 클래스 C
>   - f 가 생성한 객체
>   - f 인수로 넘어온 객체
>   - C 인스턴스 변수에 저장된 객체 

>기차 충돌 
> * 일반적으로 조잡하다 여겨지는 방식 
  ~~~java
  final String outputDir = ctxt.getOptions().getScratchDir().getAbsolutePath();
  ~~~
  위 코드를 다음과 같이 나눔
  ~~~java
  Options opts = ctxt.getOptions();
  File scratchDir = opts.getScratchDir();
  final String outputDir = scratchDir.getAbsolutePAth();
  ~~~
  
>잡종 구조
> * 중요한 기능을 수행하는 함수도, 공개 변수나 공개 조회/설정 함수도 존재 
> * 새로운 함수, 자료구조 추가가 어렵 
>   - 양쪽의 단점만 모아놓은 꼴, 되도록 사용 X

>구조체 감추기 
> * ctxt가 객체라면, 뭔가를 하라고 말해야지 속으로 드러내라고 말 X 

<br/>

### 자료 전달 객체 (DTO)
> * 공개 변수만 존재하고 함수는 없는 클래스 
> * 미가공 정보를 애플리케이션 코드에서 사용할 객체로 변환하는 일련의 단계에서 처음으로 사용하는 구조체 
> * 빈 (bean) 구조
>   - 비공개 변수를 조회/설정 함수로 조작 
>   - 일종의 사이비 캡슐화 

>활성레코드 
> * DTO의 특수한 형태 
> * 공개 변수가 있거나 비공개 변수에 조회/설정 함수가 있는 자료구조 (자료구조로 취급)
> * save, find 들의 탐색 함수도 제공

