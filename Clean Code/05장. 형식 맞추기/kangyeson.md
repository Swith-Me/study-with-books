# 05. 형식 맞추기

- **코드 형식이란?** 의사소통의 일환. 전문 개발자의 일차적인 의무
- **구현 스타일과 가독성 수준 = 유지보수 용이성과 확장성**

> ❓ 원활한 소통을 장려하는 코드 형식이란 무엇일까
<br>

## I. 적절한 행 길이를 유지하라
✔ 큰 파일보다 작은 파일 <br>
✔ 파일 길이는 200줄 미만이 바람직

#### 1. 신문 기사처럼 작성하라
- 소스 파일의 이름은 간단하면서도 설명이 가능하게
- 첫 부분은 고차원 개념과 알고리즘을 설명
- 아래로 내려갈수록 의도를 세세하게 묘사
- 마지막에는 가장 저차원 함수와 세부 내역<br><br>

#### 2. 개념은 빈 행으로 분리하라
수식이나 절을 나타내는 일련의 행 묶음(=완결된 생각 하나) 사이에는 빈 행을 넣어 **개념 분리** 

```javascript
package fitnesse.wikitext.widgets;

import java.util.regex.*;

public class BoldWidget extends ParentWidget {
  ...
}
```
➔ 빈 행은 새로운 개념을 시작하는 시각적 단서가 되어, 가독성을 높여준다. <br><br>
 
#### 3. 세로 밀집도
세로로 가까이 놓여 밀접한 코드 행은 **연관성**을 의미
```javascript
public class ReporterConfig {
  private String m_className;
  private List<Property> m_properties = new ArrayList<Property>();
  public void addProperty(Property property) {
    m_properties.add(property);
  }
}
```
➔ 코드가 '한눈'에 들어와, 하나의 클래스 구조를 파악할 수 있다. <br><br>

#### 4. 수직 거리
> 같은 파일에 속할 정도로 밀접한 두 개념은 세로(수직) 거리로 연관성을 표현한다.
- 변수 선언
  - 변수는 사용하는 위치에 최대한 가까이 선언한다.
- 인스턴스 변수
  - 인스턴스 변수는 클래스 맨 처음에 선언한다.
  - 변수 간에 세로로 거리를 두지 않는다.
- 종속 함수
  - 두 종속 함수는 세로로 가까이, 호출하는 함수를 호출되는 함수 위에 배치한다.
- 개념적 유사성
  - 개념적인 친화도가 높을수록 코드를 가까이 배치한다. 
  - 개념적 친화도 : 서로를 호출, 종속 관계, 동일한 명명법과 유사한 기본 기능<br><br>
  
#### 5. 세로 순서
- 호출되는 함수를 호출하는 함수보다 나중에 배치
- 가장 중요한 개념을 가장 먼저 표현
- 세세한 사항은 가장 마지막에 표현

<br><br>

## II. 가로 형식 맞추기
✔ 짧은 행이 바람직 <br>
✔ 행 길이는 120자 정도가 적당

#### 1. 가로 공백과 밀집도
- 밀접한 개념과 느슨한 개념 표현
- 할당문, 인수, 연산자 우선순위 등의 요소를 공백으로 구분 <br><br>

#### 2. 가로 정렬
- 선언문과 할당문의 가로 정렬은 불필요하다.

```javascript
private   Socket          socket;
private   InputStream     output;
// 👇
private Socket socket;
private InputStream output;
```
<br>

#### 3. 들여쓰기
- 들여쓰기를 사용하여 범위(scope)로 이루어진 계층 표현
- 들여쓰기 정도 = 코드가 자리잡은 수준 <br><br>

#### 4. 가짜 범위
빈 while문이나 for문은 괄호로 감싸고, 세미콜론(;)은 새 행에 제대로 들여써서 넣어준다.
```javascript
while (dis.read(buf, 0, readBufferSize) != -1)
;
```

<br><br>

## III. 팀 규칙
> 팀에 속한다면 자신이 선호해야 할 규칙은 바로 팀 규칙이다.

팀에서 합의한 한 가지 규칙을 따라, 소프트웨어를 **일관적인** 스타일로 만들어야 한다.


<br>

***

⚡ **코드는 사라질지라도 개발자의 스타일과 규율은 사라지지 않는다.** <br>
⚡ **한 소스 파일에서 봤던 형식이 다른 소스 파일에도 쓰이리라는 신뢰감을 독자에게 줘야 한다.**
