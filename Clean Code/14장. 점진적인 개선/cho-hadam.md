# 📚 14장. 점진적인 개선

<br />

## Args 구현

### Args.java
```java
package com.objectmentor.utilities.args;

import static com.objectmentor.utilities.args.ArgsException.ErrorCode.*;
import java.util.*;

public class Args {
  private Map<Character, ArgumentMarshaler> marshalers;
  private Set<Character> argsFound;
  private ListIterator<String> currentArgument;

  public Args(String schema, String[] args) throws ArgsException {
    marshalers = new HashMap<Character, ArgumentMarshaler>();
    argsFound = new HashSet<Character>();

    parseSchema(schema);
    parseArgumentStrings(Arrays.asList(args));
  }

  private void parseSchema(String schema) throws ArgsException {
    for (String element : schema.split(",")) {
      if (element.length() > 0)
        parseSchemaElement(element.trim());
    }
  }

  private void parseSchemaElement(String element) throws ArgsException {
    char elementId = element.charAt(0);
    String elementTail = element.substring(1);
    validateSchemaElementId(elementId);
    if (elementTail.length() == 0)
      marshalers.put(elementId, new BooleanArgumentMarshaler());
    else if (elementTail.equals("*"))
      marshalers.put(elementId, new StringArgumentMarshaler());
    else if (elementTail.equals("#"))
      marshalers.put(elementId, new IntegerArgumentMarshaler());
    else if (elementTail.equals("##"))
      marshalers.put(elementId, new DoubleArgumentMarshaler());
    else if (elementTail.equals("[*]"))
      marshalers.put(elementId, new StringArrayArgumentMarshaler());
    else
      throw new ArgsException(INVALID_ARGUMENT_FORMAT, elementId, elementTail);
  }

  private void validateSchemaElementId(char elementId) throws ArgsException {
    if (!Character.isLetter(elementId))
      throw new ArgsException(INVALID_ARGUMENT_NAME, elementId, null);
  }

  private void parseArgumentStrings(List<String> argsList) throws ArgsException {
    for (currentArgument = argsList.listIterator(); currentArgument.hasNext();) {
      String argString = currentArgument.next();
      if (argString.startsWith("-")) {
        parseArgumentCharacters(argString.substring(1));
      } else {
        currentArgument.previous();
        break;
      }
    }
  }

  private void parseArgumentCharacters(String argChars) throws ArgsException {
    for (int i = 0; i < argChars.length(); i++) {
      parseArgumentCharacter(argChars.charAt(i));
    }
  }

  private void parseArgumentCharacter(char argChar) throws ArgsException {
    ArgumentMarshaler m = marshalers.get(argChar);
    if (m == null) {
      throw new ArgsException(UNEXPECTED_ARGUMENT, argChar, null);
    } else {
      argsFound.add(argChar);
      try {
        m.set(currentArgument);
      } catch (ArgsException e) {
        e.setErrorArgumentId(argChar);
        throw e;
      }
    }
  }

  public boolean has(char arg) {
    return argsFound.contains(arg);
  }

  public int nextArgument() {
    return currentArgument.nextIndex();
  }

  public boolean getBoolean(char arg) {
    return BooleanArgumentMarshaler.getValue(marshalers.get(arg));
  }

  public String getString(char arg) {
    return StringArgumentMarshaler.getValue(marshalers.get(arg));
  }

  public int getInt(char arg) {
    return IntegerArgumentMarshaler.getValue(marshalers.get(arg));
  }

  public double getDouble(char arg) {
    return DoubleArgumentMarshaler.getValue(marshalers.get(arg));
  }

  public String[] getStringArray(char arg) {
    return StringArgumentMarshaler.getValue(marshalers.get(arg));
  }
}
```

### ArgumentMarshaler.java
```java
package com.objectmentor.utilities.args;

import java.util.Iterator;

public interface ArgumentMarshaler {
  void set(Iterator<String> currentArgument) throws ArgsException;
}
```

### BooleanArgumentMarshaler.java
```java
package com.objectmentor.utilities.args;

import java.util.Iterator;

public class BooleanArgumentMarshaler implements ArgumentMarshaler {
  private boolean booleanValue = false;

  @Override
  public void set(Iterator<String> currentArgument) throws ArgsException {
    booleanValue = true;
  }

  public static boolean getValue(ArgumentMarshaler am) {
    if (am != null && am instanceof BooleanArgumentMarshaler)
      return ((BooleanArgumentMarshaler) am).booleanValue;
    else
      return false;
  }
}
```

### StringArgumentMarshaler
```java
package com.objectmentor.utilities.args;

import static com.objectmentor.utilities.args.ArgsException.ErrorCode.*;
import java.util.Iterator;
import java.util.NoSuchElementException;

public class StringArgumentMarshaler implements ArgumentMarshaler {
  private String stringValue = "";

  @Override
  public void set(Iterator<String> currentArgument) throws ArgsException {
    try {
        stringValue = currentArgument.next();
    } catch (NoSuchElementException e) {
        throw new ArgsException(MISSING_STRING);
    }
  }

  public static String getValue(ArgumentMarshaler am) {
    if (am != null && am instanceof StringArgumentMarshaler)
      return ((StringArgumentMarshaler) am).stringValue;
    else
      return "";
  }
}
```

### IntegerArgumentMarshaler
```java
package com.objectmentor.utilities.args;

import static com.objectmentor.utilities.args.ArgsException.ErrorCode.*;
import java.util.Iterator;
import java.util.NoSuchElementException;

public class IntegerArgumentMarshaler implements ArgumentMarshaler {
  private int intValue = 0;

  @Override
  public void set(Iterator<String> currentArgument) throws ArgsException {
    String parameter = null;
    try {
      parameter = currentArgument.next();
      intValue = Integer.parseInt(parameter);
    } catch (NoSuchElementException e) {
      throw new ArgsException(MISSING_INTEGER);
    } catch (NumberFormatException e) {
      throw new ArgsException(INVALID_INTEGER, parameter);
    }
  }

  public static int getValue(ArgumentMarshaler am) {
    if (am != null && am instanceof IntegerArgumentMarshaler)
      return ((IntegerArgumentMarshaler) am).intValue;
    else
      return 0;
  }
}
```

### ArgsException.java
```java
package com.objectmentor.utilities.args;

import static com.objectmentor.utilities.args.ArgsException.ErrorCode.*;

public class ArgsException extends Exception {
  private char errorArgumentId = '\0';
  private String errorParameter = null;
  private ErrorCode errorCode = OK;

  public ArgsException() {}

  public ArgsException(String message) {
    super(message);
  }

  public ArgsException(ErrorCode errorCode) {
    this.errorCode = errorCode;
  }

  public ArgsException(ErrorCode errorCode, String errorParameter) {
    this.errorCode = errorCode;
    this.errorParameter = errorParameter;
  }

  public ArgsException(ErrorCode errorCode, char errorArgumentId, String errorParameter) {
    this.errorCode = errorCode;
    this.errorArgumentId = errorArgumentId;
    this.errorParameter = errorParameter;
  }

  public char getErrorArgumentId() {
    return errorArgumentId;
  }

  public void setErrorArgumentId(char errorArgumentId) {
    this.errorArgumentId = errorArgumentId;
  }

  public String getErrorParameter() {
    return errorParameter;
  }

  public void setErrorParameter(String errorParameter) {
    this.errorParameter = errorParameter;
  }

  public ErrorCode getErrorCode() {
    return errorCode;
  }

  public void setErrorCode(ErrorCode errorCode) {
    this.errorCode = errorCode;
  }

  public String errorMessage() {
    switch (errorCode) {
      case OK:
        return "TILT: Should not get here.";
      case UNEXPECTED_ARGUMENT:
        return String.format("Argument -%c unexpected.", errorArgumentId);
      case MISSING_STRING:
        return String.format("Could not find string parameter for -%c.", errorArgumentId);
      case INVALID_INTEGER:
        return String.format("Argument -%c expects an integer but was '%s'.", errorArgumentId, errorParameter);
      case MISSING_INTEGER:
        return String.format("Could not find integer parameter for -%c.", errorArgumentId);
      case INVALID_DOUBLE:
        return String.format("Argument -%c expects an double but was '%s'.", errorArgumentId, errorParameter);
      case MISSING_DOUBLE:
        return String.format("Could not find double parameter for -%c.", errorArgumentId);
      case INVALID_ARGUMENT_NAME:
        return String.format("'%c' is not a valid argument name.", errorArgumentId);
      case INVALID_ARGUMENT_FORMAT:
        return String.format("'%s' is not a valid argument format.", errorParameter);
    }
    return "";
  }

  public enum ErrorCode {
    OK, INVALID_ARGUMENT_FORMAT, UNEXPECTED_ARGUMENT,
    INVALID_ARGUMENT_NAME,
    MISSING_STRING,
    MISSING_INTEGER, INVALID_INTEGER,
    MISSING_DOUBLE, INVALID_DOUBLE
  }
}
```

위 프로그램을 처음부터 저렇게 구현하지 않았다

**깨끗한 코드를 짜려면 먼저 지저분한 코드를 짠 뒤에 정리해야 한다**

<br />

## Args: 1차 초안

코드는 돌아가지만 엉망이다

### 적기

추가할 인수 유형이 적어도 두 개는 더 있었는데 그러면 코드가 훨씬 더 나빠지리라는 사실이 자명했다   
계속 밀어붙이면 프로그램은 어떻게든 완성하겠지만 그랬다가는 너무 커서 손대기 어려운 골칫거리가 생겨날 참이었다

### 점진적으로 개선하다

프로그램을 망치는 가장 좋은 방법 중 하나는 개선이라는 이름 아래 구조를 크게 뒤집는 행위다

<br />

## 리팩터링

리팩터링을 하다 보면 코드를 넣었다 뺐다 하는 사례가 아주 흔하다   
리팩터링은 큰 목표 하나를 이루기 위해 자잘한 단계를 수없이 거친다

적절한 장소를 만들어 코드만 분리해도 설계가 좋아진다   
관심사를 분리하면 코드를 이해하고 보수하기 훨씬 더 쉬워진다

<br />

## 결론

처음부터 코드를 깨끗하게 유지하기란 상대적으로 쉽다

코드는 언제나 최대한 깔끔하고 단순하게 정리하자   
절대로 썩어가게 방치하면 안 된다

<br />

## 몰랐던 것

**ListIterator**   
Iterator```컬렉션에 저장된 요소를 읽어오는 방법을 정의한 인터페이스```를 상속받은 인터페이스로, 양방향 이동을 지원한다

**Arrays.asList**   
일반 배열을 ArrayList로 변환한다

**Character.isLetter**   
char 값이 문자인지의 여부를 리턴한다

<br />

## 기억에 남은 부분

> _프로그램을 망치는 가장 좋은 방법 중 하나는 개선이라는 이름 아래 구조를 크게 뒤집는 행위다._

> _그저 돌아가는 코드만으로는 부족하다._
