# 03. 함수

- **함수란?** 어떤 프로그램에서든 가장 기본적인 단위
- **시스템이란?** 구현할 프로그램❌ 풀어갈 이야기⭕
- 함수는 이야기를 풀어갈 언어의 **동사**

> **목표**✔ <br>
> 분명하고 정확하여 언어와 깔끔하게 맞아떨어지는 함수를 작성해서, 시스템이라는 이야기를 풀어나가는 것
<br>

## I. 작게 만들어라
함수는 명백하게 이야기 하나만을 표현해야 한다. <br>
- if 문/else 문/ while 문의 블록은 한 줄이어야 한다.
- 중첩 구조가 생길만큼 함수가 커져서는 안 된다.
- 들여쓰기는 1단이나 2단을 넘어서면 안 된다.

함수의 길이는 아래 코드의 길이만큼이 바람직하다.
```javascript
public static String renderPageWithSetupsAndTeardowns (
  PageData pageData, boolean isSuite) throws Exception {
  if (isTestPage(pageData))
    includeSetupAndTeardownPages(pageData, isSuite);
  return pageData.getHtml();
}
```

<br><br>

## II. 한 가지만 해라
> 함수를 만드는 이유는 **큰 개념**을 다음 **추상화 수준**에서 **여러 단계**로 나눠 **수행**하기 위해서가 아니던가.

#### 한 가지란?
* 추상화 수준이 하나인지
  * 추상화 수준이 하나인 단계만 수행한다면 그 함수는 한 가지 작업만 한다.
  * 추상화 단계가 다양하면 그 함수는 여러 단계를 처리한다.
* 단순히 다른 표현이 아니라 의미 있는 이름으로 다른 함수를 추출할 수 있는지

#### ❓ 추상화 수준은 어떻게 판단할까

위 renderPageWithSetupsAndTeardowns 함수를 TO 문단으로 기술하여 추상화 수준을 살펴보자. <br>
```
페이지가 테스트 페이지인지 확인한 후
테스트 페이지라면 설정 페이지와 해체 페이지를 넣는다.
테스트 페이지든 아니든 페이지를 HTML로 렌더링한다.
```

 ➔ 세 단계로 보이지만 더 이상 줄이기란 불가능하다. 지정된 함수 이름 아래에서 추상화 수준이 하나다.

<br><br>

## III. 함수 당 추상화 수준은 하나로
> 함수가 확실히 '한 가지' 작업만 하려면 함수 내 **모든 문장의 추상화 수준이 동일**해야 한다.

#### 한 함수 내 추상화 수준이 섞이면?
- 특정 표현이 근본 개념인지 세부사항인지 구분이 어려워진다.
- 읽는 사람에게 혼란을 주고, 점점 세부사항이 추가될 것이다.

<br>

 ➔ **내려가기 규칙을 사용하자** <br>
 코드는 일련의 TO 문단을 읽듯이 **위에서 아래로** 이야기처럼 읽혀야 좋다. <br>
 즉, 함수 추상화 수준이 한 번에 한 단계씩 낮아진다.

<br><br>

## IV. 서술적인 이름을 사용하라
> 코드를 읽으면서 **짐작했던 기능**을 각 루틴이 그대로 수행한다면 깨끗한 코드라 불러도 되겠다.

- 길고 서술적인 이름이 짧고 어려운 이름, 서술적인 주석보다 좋다.
- 서술적인 이름은 설계를 뚜렷하게 만들어 코드 개선을 용이하게 한다.
- 일관성(같은 문구, 명사, 동사 등)이 있어야 한다.

```
ex)
includeSetupAndTeardownPages
includeSetupPages
includeSuiteSetupPage
includeSetupPage
```

<br>

### 함수 인수
함수에서 가장 이상적인 인수 개수는 0개로, 인수는 적을수록 좋다. 인수는 개념을 이해하기 어렵게 만들고 테스트 케이스를 작성하기 부담스럽게 한다. <br>

#### 1. 단항 형식(인수 1개)
단항 함수는 다음과 같은 경우들을 언제나 일관적이고 분명한 방식으로 구분해 사용해야 한다.
```javascript
// 1. 인수에 질문을 던지는 경우
boolean fileExists("MyFile")

// 2. 인수를 뭔가로 변환해 결과를 반환하는 경우
InputStream fileOpen("MyFile") ➔ String형의 파일 이름 InputStream으로 변환

// 3. 입력 인수만 있고 출력 인수는 없는 이벤트 함수의 경우
passwordAttemptFailedNtimes(int attempts)
```

위 3가지를 제외한 경우의 단항 함수는 가급적 피하라.


#### 2. 플래그 인수는 지양하라
플래그 인수는 함수가 한꺼번에 여러 가지를 처리한다고 공표하는 셈과 같다.

 
#### 3. 이항 함수, 삼항 함수
인수가 1개인 함수보다 2개인 함수가, 인수가 2개인 함수보다 3개인 함수가 훨씬 더 이해하기 어렵다. <br>
함수와 마주치면 잠시 주춤하며 어떤 인수를 무시해야 하는지 깨닫는 시간이 걸리기 때문이다. 하지만 어떤 코드든 무시할 경우 위험이 따른다. <br>


인수를 줄이기 위해 일부는 독자적인 클래스 변수로 선언할 수 있다.
```javascript
Circle makeCircle(double x, double y, double radius);
Circle makeCircle(Point center, double radius);
```

<br><br>

## V. 부수 효과를 일으키지 마라
> 부수 효과는 거짓말이다. 함수에서 한 가지를 하겠다고 약속하고선 남몰래 다른 짓도 하니까. 
```
부수 효과란❓
외부의 상태를 변경하는 것 또는 함수로 들어온 인자의 상태를 직접 변경하는 것
```

<br>

부수 효과는 시간적인 결합과 순서 종속성을 초래한다.

```javascript
public boolean checkPassword(String userName, String password){
	User user = UserGateway.findByName(userName);
    if(user != User.Null) {
    	String codePhrase = user.getPhraseEncodedByPassword();
        String phrase = crytographer.decrypt(codedPhrase, password);
        if("Valid Password".equals(phrase)) {
            Session.initialize();  //✔
            return true;
        }
    }
    return false;
}
```

userName과 password를 확인하는 무해한 함수처럼 보이지만, Session.initialize() 이 부수 효과를 일으킨다. 이 부수 효과로 인해 기존 세션 정보가 초기화될 위험이 있다. <br><br>

출력 인수또한 피해야 한다. <br>
함수에서 상태를 변경해야 한다면 함수가 속한 객체 상태를 변경하는 방식을 택한다.

<br><br>

## VI. 명령과 조회를 분리하라
> 함수는 뭔가를 수행하거나 뭔가를 답하거나 둘 중 하나만 해야 한다.

한 번에 객체 상태를 변경하거나 객체 정보를 반환하면 혼란을 초래한다.
```javascript
public boolean set(String attribute, String value);

if(set("username", "bob"))...
```
위 코드에선 "set"이라는 단어가 if문에 들어가, 동사가 아닌 형용사로 느껴진다. <br>
즉, "username"이 "unclebob"으로 설정되어 있는지 확인하는 코드인지, "username"울 "unclebob"으로 설정하는 코드인지 분간하기 어렵다. <br>
명령과 조회를 분리하여 아래처럼 해결할 수 있다.
```javascript
if(attributeExist("username")){
    setAttribute("username", "bob");
    ...
}
```

<br><br>

## VII. 오류 코드보다 예외를 사용하라
> 오류 코드를 반환하는 방식은 명령/조회 분리 규칙을 위반한다.

오류 코드를 반환하면 호출자가 오류 코드를 곧바로 처리해야 한다는 문제에 부딪히고, 아래와 같은 코드는 여러 단계로 중첩되어 있다.
```javascript
if (deletePage(page) == E_OK) {
  if (registry.deleteReference(page.name) == E_OK) {
    if (configKeys.deleteKey(page.name.makeKey()) == E_OK) {
      logger.log("page deleted");
    } else {
      logger.log("configKey not deleted");
    }
  } else {
    logger.log("deleteReference from registry failed");
  }
} else {
  logger.log("delete failed");
  return E_ERROR;
}
```

오류 코드 대신 예외를 사용하면 원래 코드에서 오류 처리 코드가 분리되어, 코드가 더 깔끔해진다.

```javascript
try {
   deletePage(page);
   registry.deleteReference(page.name);
   configKeys.deleteKey(page.name.makeKey());
}
   catch (Exception e) {
   logger.log(e.getMessage());
}
```

<br> 

- 오류 처리도 **한 가지** 작업이다. 오류를 처리하는 함수는 오류만 처리해야 마땅하다.
- try/catch 블록은 별도 함수로 뽑아내라.
- 새 오류 코드 추가 대신, 기존 오류 코드를 재사용하는 Error.java 의존성 자석을 사용하라.
 
<br><br>

## VIII. 반복하지 마라
> 어쩌면 중복은 소프트웨어에서 모든 악의 근원이다.


<br><br>

## IX. 구조적 프로그래밍
```
구조적 프로그래밍 원칙이란❓
함수는 return문이 하나만 존재. 루프 안에서 break나 continue를 사용해선 안 된다.
```
 ➔ **함수가 작다면** 구조적 프로그래밍 원칙을 지킬 필요 없다. <br>
   오히려 return, break, continue를 필요에 따라 여러 차례 사용하는 것이 의도를 표현하기에 좋다.
   
<br>

***

⚡ **소프트웨어를 짜는 행위는 여느 글짓기와 비슷하다.**




