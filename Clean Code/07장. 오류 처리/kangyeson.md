# 07. 오류 처리

- 뭔가 잘못될 가능성은 늘 존재하고, 잘못을 바로 잡을 책임은 프로그래머에게 있다.
- 흩어진 오류 처리 코드 ➔ 실제 코드 파악 불가능
- 깨끗한 코드와 오류 처리는 연관되어 있다.

> **목표**✔ <br>
> 깨끗하고 튼튼한 코드, 우아하고 고상하게 오류를 처리하는 기법과 고려사항을 익히는 것
<br>

## I. 오류 코드보다 예외를 사용하라
- 오류 태그와 오류 코드 ❌
  - 복잡해지는 호출자 코드

- 예외 사용 ✔
  - 기능을 구현하는 알고리즘과 오류를 처리하는 알고리즘 분리
  - 독립적인 각 개념
  - 코드 품질 개선

<br><br>

## II. Try-Catch-Finally 문부터 작성하라
> 예외가 발생할 코드를 짤 때 Try-Catch-Finally 문으로 시작하는 편이 낫다.

- 예외에서 프로그램 안에다 **범위를 정의** <br>
- try 블록에서 무슨 일이 생기든 catch 블록은 프로그램 상태를 일관성 있게 유지 <br>
- 호출자가 기대하는 상태 정의가 쉬워짐 <br><br>

```java
public List<RecordGrip> retrieveSection(String sectionName) {
  try {
    FileInputStream stream = new FileInputStream(sectionName);
    stream.close();
  } catch (FileNotFoundException e) {
    throw new StorageException("retrieval error", e);
  }
  return new ArrayList<RecordeGrip>();
}
```
1. 코드가 예외를 던지므로 단위 테스트 성공 (**리펙토링** 가능) <br><br>
2. catch 블록에서 **예외 유형 축소** <br>
➔ FileInputStream 생성자가 던진 FileNotFoundException을 잡아낸다. <br><br>
3. try-catch로 범위 정의 완료 <br><br>
4. **TDD**를 사용하여 필요한 나머지 논리 추가 <br>
➔ stream.close()문 삽입 <br>
➔ 강제로 예외를 일으키는 테스트 케이스 작성 후, 테스트 통과 코드 작성 <br><br>

###### ❓ 리펙토링 | 외부 동작을 바꾸지 않으면서 내부 구조를 개선하는 방법
###### ❓ TDD | Test Driven Development(테스트 주도 개발), 테스트를 먼저 만들고 테스트를 통과하기 위한 것을 짜는 것

<br><br>

## III. 미확인 예외를 사용하라
확인된 예외란? 

<br><br>

## IV. 예외에 의미를 제공하라


<br><br>

## V. 호출자를 고려해 예외 클래스를 정의하라


<br><br>

## VI. 정상 흐름을 정의하라


<br><br>

## VII. null을 반환하지 마라


 
<br><br>

## VIII. null을 전달하지 마라


<br><br>

## IX. 결론

   
<br>

***

⚡ 




