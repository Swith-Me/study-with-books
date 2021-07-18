# 06. 객체와 자료 구조

> 목표✔ <br> 조회(get)함수와 설정(set)함수를 공개하여 비공개 변수를 외부에 노출하는 코드를 고치자
<br>

## I. 자료 추상화
- 변수 사이에 함수 계층을 넣는다고 구현이 저절로 감춰지지는 않는다.
➔ 추상화 필요
- 추상 인터페이스를 제공해 사용자가 **구현을 모른 채 자료의 핵심을 조작**할 수 있어야 진정한 의미의 클래스

```javascript
// 구체적인 클래스 ❌
public interface Vehicle {
  double getFuelTankCapacityInGallons();
  double getGallonsOfGasoline();
}

// 추상적인 클래스 ⭕
public interface Vehicle {
  double getPercentFuelRemaining();
}
```

<br><br>

## II. 자료/객체 비대칭
- **객체** : 추상화 뒤로 자료를 숨긴 채 자료를 다루는 함수만 공개
- **자료 구조** : 자료를 그대로 공개하며 별다른 함수 제공 X
- 본질적으로 상반된 개념, 상호 보완적인 특질
  - 새로운 자료타입이 필요한 경우 : 객체 지향 기법
  - 새로운 함수가 필요한 경우 : 자료 구조


<br><br>

## III. 디미터 법칙
> 휴리스틱(heuristic), 모듈은 자신이 조작하는 객체의 속사정을 몰라야 한다는 법칙

➔ 클래스 C의 메서드는 f는 다음과 같은 객체의 메서드만 호출해야 한다

- 클래스 C
- f가 생성한 객체
- f 인수로 넘어온 객체
- C 인스턴스 변수에 저장된 객체


###### ❓ 휴리스틱이란 | 체계적이면서 합리적인 판단이 굳이 필요하지 않은 상황에서 사람들이 빠르게 사용할 수 있게 보다 용이하게 구성된 간편추론의 방법

<br>

#### 1. 기차 충돌
```javascript
final String outputDir = ctxt.getOptions().getScratchDir().getAbsolutePath();
```
 ☝ 한 줄로 이어진 기차처럼 보이는 기차 충돌 코드. 조잡하다고 여겨지기 때문에 분할작업이 필요
 
```javascript
Options opts = ctxt.getOptions();
File scratchDir = opts.getScratchDir();
final String outputDir = scratchDir.getAbsolutePath();
```

<br>

#### 2. 잡종 구조
절반은 객체, 절반은 자료 구조인 잡종 구조 <br>
➔ 양쪽의 단점만 모아놓은 구조 <br><br>

#### 3. 구조체 감추기
❓ 내부 구조를 감춰야 하는 객체, 임시 디렉토리의 절대 경로는 어떻게 얻을까 <br>
```javascript
ctxt.getAbsolutePathOfScratchDirectoryOption();
ctx.getScratchDirectoryOption().getAbsolutePath();
```
➔ ctxt가 객체라면 뭔가를 하라고 말해야지 속을 드러내라고 말하면 안 된다. <br><br>

이때, 임시 디렉토리의 절대 경로를 얻으려는 이유가 임시 파일을 생성하기 위한 목적일 경우
```javascript
BufferedOutputStream bos = ctxt.createScratchFileStream(classFileName);
```
➔ ctxt 객체에 맡기는 임시 파일을 생성 임무는 적당하다. <br><br>
✔ **ctxt는 내부 구조를 드러내지 않으며, 모듈에서 해당 함수는 자신이 몰라야 하는 여러 객체를 탐색할 필요가 없다.**


<br><br>

## IV. 자료 전달 객체 (DTO)
> 자료 구조체의 전형적인 형태는 **공개 변수**만 있고 **함수가 없는** 클래스다.

- 데이터베이스와 통신하거나 소켓에서 받은 메시지의 구분을 분석할 때 유용
- **가공되지 않은 정보**를 코드에서 사용할 **객체로 변환하는** 일련의 단계에서 가장 **처음으로 사용**하는 구조체
- 일반적인 형태는 빈(bean) 구조 : 비공개 변수를 조회/설정 함수로 조작

#### 활성 레코드
- DTO의 특수한 형태
- 데이터베이스 테이블이나 다른 소스에서 자료를 직접 변환한 결과
- 자료 구조로 취급, 비즈니스 규칙을 담으면서 내부 자료를 숨기는 객체는 따로 생성


<br><br>

## V. 결론

#### 객체
- 동작을 공개하고 자료를 숨긴다.
- 기존 동작을 변경하지 않으면서 **새 객체 타입**을 추가하기 쉽다.
- 기존 객체에 **새 동작**을 추가하기 어렵다.

➔ 새로운 자료 타입을 추가하는 유연성이 필요할때 적합

#### 자료 구조
- 별다른 동작 없이 자료를 노출한다.
- 기존 함수에 **새 자료 구조**를 추가하기 어렵다.
- 기존 자료 구조에 **새 동작**을 추가하기 쉽다.

➔ 새로운 동작을 추가하는 유연성이 필요할때 적합


***

⚡ **우수한 소프트웨어 개발자는 편견 없이 위 사실들을 이해해 문제에 최적인 해결책을 선택한다.**

