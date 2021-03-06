# ๐ [14์ฅ] ์ ์ง์ ์ธ ๊ฐ์ 

---

> ๊นจ๋ํ ์ฝ๋๋ฅผ ์ง๋ ค๋ฉด ๋จผ์  ์ง์ ๋ถํ ์ฝ๋๋ฅผ ์ง  ๋ค์ ์ ๋ฆฌํด์ผ ํ๋ค๋ ์๋ฏธ์ด๋ค.

---

## ์ฝ๋ ์ ๋ฆฌ

- Args.java

```java
import java.util.*;

public class Args {
    private String schema;
    private Map<Character, ArgumentMarshaler> marshalers =
                                             new HashMap<Character, ArgumentMarshaler>();
    private Set<Character> argsFound = new HashSet<Character>();
    private Iterator<String> currentArgument;
    private List<String> argsList;

    public Args(String schema, String[] args) throws ArgsException {
        this.schema = schema;
        argsList = Arrays.asList(args);
        parse();
    }

    private void parse() throws ArgsException {
        parseSchema();
        parseArguments();
    }

    private boolean parseSchema() throws ArgsException {
        for (String element : schema.split(",")) {
            if (element.length() > 0) {
                parseSchemaElement(element.trim());
            }
        }
        return true;
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
        else
            throw new ArgsException(ArgsException.ErrorCode.INVALID_FORMAT,
                elementId, elementTail);
    }

    private void validateSchemaElementId(char elementId) throws ArgsException {
        if (!Character.isLetter(elementId)) {
            throw new ArgsException(ArgsException.ErrorCode.INVALID_ARGUMENT_NAME,
                elementId, null);
        }
    }

    private void parseArguments() throws ArgsException {
        for (currentArgument = argsList.iterator(); currentArgument.hasNext();) {
            String arg = currentArgument.next();
            parseArgument(arg);
        }
    }

    private void parseArgument(String arg) throws ArgsException {
        if (arg.startsWith("-"))
            parseElements(arg);
    }

    private void parseElements(String arg) throws ArgsException {
        for (int i = 1; i < arg.length(); i++)
            parseElement(arg.charAt(i));
    }

    private void parseElement(char argChar) throws ArgsException {
        if (setArgument(argChar))
            argsFound.add(argChar);
        else {
            throw new ArgsException(ArgsException.ErrorCode.UNEXPECTED_ARGUMENT,
                argChar, null);
        }
    }

    private boolean setArgument(char argChar) throws ArgsException {
        ArgumentMarshaler m = marshalers.get(argChar);
        if (m == null)
            return false;
        try {
            m.set(currentArgument);
            return true;
        } catch (ArgsException e) {
            e.setErrorArgumentId(argChar);
            throw e;
        }
    }
    public int cardinality() {
        return argsFound.size();
    }

    public String usage() {
        if (schema.length() > 0)
            return "-[" + schema + "]";
        else
            return "";
    }

    public boolean getBoolean(char arg) {
        ArgumentMarshaler am = marshalers.get(arg);
        boolean b = false;
        try {
            b = am != null && (Boolean) am.get();
        } catch (ClassCastException e) {
            b = false;
        }
        return b;
    }

    public String getString(char arg) {
        ArgumentMarshaler am = marshalers.get(arg);
        try {
            return am == null ? "" : (String) am.get();
        } catch (ClassCastException e) {
            return "";
        }
    }

    public int getInt(char arg) {
        ArgumentMarshaler am = marshalers.get(arg);
        try {
            return am == null ? 0 : (Integer) am.get();
        } catch (Exception e) {
            return 0;
        }
    }


    public double getDouble(char arg) {
        ArgumentMarshaler am = marshalers.get(arg);
        try {
            return am == null ? 0 : (Double) am.get();
        } catch (Exception e) {
            return 0.0;
        }
    }

    public boolean has(char arg) {
        return argsFound.contains(arg);
    }
}
```

- ArgsException.java

```java
public class ArgsException extends Exception {
    private char errorArgumentId = '\0';
    private String errorParameter = "TILT";
    private ErrorCode errorCode = ErrorCode.OK;

    public ArgsException() {}

    public ArgsException(String message) {super(message);}

    public ArgsException(ErrorCode errorCode) {
        this.errorCode = errorCode;
    }
    public ArgsException(ErrorCode errorCode, String errorParameter) {
        this.errorCode = errorCode;
        this.errorParameter = errorParameter;
    }

    public ArgsException(ErrorCode errorCode, char errorArgumentId,
        String errorParameter) {
        this.errorCode = errorCode;
        this.errorParameter = errorParameter;
        this.errorArgumentId = errorArgumentId;
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

    public String errorMessage() throws Exception {
        switch (errorCode) {
            case OK:
            throw new Exception("TILT: Should not get here.");
            case UNEXPECTED_ARGUMENT:
            return String.format("Argument -%c unexpected.", errorArgumentId);
            case MISSING_STRING:
            return String.format("Could not find string parameter for -%c.",
                errorArgumentId);
            case INVALID_INTEGER:
            return String.format("Argument -%c expects an integer but was '%s'.",
                errorArgumentId, errorParameter);
            case MISSING_INTEGER:
            return String.format("Could not find integer parameter for -%c.",
                errorArgumentId);
            case INVALID_DOUBLE:
            return String.format("Argument -%c expects a double but was '%s'.",
                errorArgumentId, errorParameter);
            case MISSING_DOUBLE:
            return String.format("Could not find double parameter for -%c.",
                errorArgumentId);
        }
        return "";
    }

    public enum ErrorCode {
        OK, INVALID_FORMAT, UNEXPECTED_ARGUMENT, INVALID_ARGUMENT_NAME,
        MISSING_STRING,
        MISSING_INTEGER, INVALID_INTEGER,
        MISSING_DOUBLE, MISSING_BOOLEAN, INVALID_BOOLEAN, INVALID_DOUBLE}
    }
```

- ArgumentMarshaler.java

```java
import java.util.Iterator;

public interface ArgumentMarshaler {
    void set(Iterator<String> currentArgument) throws ArgsException;
    Object get();
}
```

- BooleanArgumentMarshaler.java

```java
import java.util.Iterator;
import java.util.NoSuchElementException;

import static clean.code.chapter14.refactored.second.ArgsException.ErrorCode.INVALID_BOOLEAN;
import static clean.code.chapter14.refactored.second.ArgsException.ErrorCode.MISSING_BOOLEAN;

public class BooleanArgumentMarshaler implements ArgumentMarshaler {
  private boolean booleanValue = false;

  public void set(Iterator<String> currentArgument) throws ArgsException {
    String parameter = null;
    try {
      parameter = currentArgument.next();
      booleanValue = Boolean.parseBoolean(parameter);
    } catch (NoSuchElementException e) {
      throw new ArgsException(MISSING_BOOLEAN);
    } catch (NumberFormatException e) {
      throw new ArgsException(INVALID_BOOLEAN, parameter);
    }
  }

  public Object get() {
    return booleanValue;
  }
}
```

- DoubleArgumentMarshaler.java

```java
import java.util.Iterator;
import java.util.NoSuchElementException;

import static clean.code.chapter14.refactored.second.ArgsException.ErrorCode.INVALID_DOUBLE;
import static clean.code.chapter14.refactored.second.ArgsException.ErrorCode.MISSING_DOUBLE;

public class DoubleArgumentMarshaler implements ArgumentMarshaler {

  private double doubleValue = 0;

  public void set(Iterator<String> currentArgument) throws ArgsException {
    String parameter = null;
    try {
      parameter = currentArgument.next();
      doubleValue = Double.parseDouble(parameter);
    } catch (NoSuchElementException e) {
      throw new ArgsException(MISSING_DOUBLE);
    } catch (NumberFormatException e) {
      throw new ArgsException(INVALID_DOUBLE, parameter);
    }
  }

  public Object get() {
    return doubleValue;
  }
}
```

- IntegerArgumentMarshaler.java

```java
import java.util.Iterator;
import java.util.NoSuchElementException;

import static clean.code.chapter14.refactored.second.ArgsException.ErrorCode.INVALID_INTEGER;
import static clean.code.chapter14.refactored.second.ArgsException.ErrorCode.MISSING_INTEGER;

public class IntegerArgumentMarshaler implements ArgumentMarshaler {
  private int intValue = 0;

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

  public Object get() {
    return intValue;
  }
}
```

- StringArgumentMarshaler.java

```java
import java.util.Iterator;
import java.util.NoSuchElementException;

import static clean.code.chapter14.refactored.second.ArgsException.ErrorCode.MISSING_STRING;

public class StringArgumentMarshaler implements ArgumentMarshaler {
  private String stringValue = "";

  public void set(Iterator<String> currentArgument) throws ArgsException {
    try {
      stringValue = currentArgument.next();
    } catch (NoSuchElementException e) {
      throw new ArgsException(MISSING_STRING);
    }
  }

  public Object get() {
    return stringValue;
  }
}
```

<br/>

---

<br/>

## ์ด๋ป๊ฒ ์งฐ๋๋๊ณ ?

- 1์ฐจ ์ด์์ ์์์ผ๋ก ๊ณ์ ๊ณ ์ณ ์ต์ข์์ ๋ง๋ค์ด๋ผ!
- **"์ ์ง์ ์ธ ๊ฐ์ "**

<br/>

## ๊ทธ๋์ ๋ฉ์ท๋ค

- ์ด๋๋ก ์งํํ๋ค๊ฐ, ์ฝ๋๊ฐ ํจ์ฌ ๋ ๋๋น ์ง ๊ฒ์ด๋ผ๋ ํ์ ์ด ์๋ค๋ฉด ๋ฉ์ถฐ๋ผ
- ์ ์ง๋ณด์ํ๊ธฐ ์ข์ ์ํ๋ก ๋ง๋ค ์ ๊ธฐ๋ฅผ ๋์น์ง ๋ง๋ผ!
- ๋ฉ์ท๋ค๋ฉด ๊ธฐ๋ฅ ์ถ๊ฐ๋ฅผ ์ ์ ๊ทธ๋งํ๊ณ , **๋ฆฌํฉํฐ๋ง**์ ์์ํ๊ธฐ

<br/>

## ์ ์ง์ ์ผ๋ก ๊ฐ์ ํ๋ค

- ๊ฐ์ ์ด๋ผ๋ ์ด๋ฆ ์๋ ๊ตฌ์กฐ๋ฅผ ํฌ๊ฒ ๋ค์ง๋ ๊ฒ์ ํ๋ก๊ทธ๋จ์ ๋ง์น๋ ํ์
- **ํ์คํธ ์ฃผ๋ ๊ฐ๋ฐ ๊ธฐ๋ฒ**์ ์ ๊ทน์ ์ผ๋ก ํ์ฉ!

<br/>

## ๊ฒฐ๋ก 

- ๋์ ์ฝ๋๋ ์ง๊ธ ๋น์ฅ ์ ๋ฆฌํ์. ์ฉ๊ธฐ ์ ์!

<br/>

---

## ์ธ์ ๊น์๋...

> _๋์ฑ ์ค์ํ๊ฒ๋ ์ฌ๋ฌ๋ถ์ด ๊นจ๋ํ๊ณ  ์ฐ์ํ ํ๋ก๊ทธ๋จ์ ํ ๋ฐฉ์ ๋ฉ๋ฑ ๋ด๋์ผ๋ฆฌ๋ผ ๊ธฐ๋ํ์ง ์๋๋ค._

> _๊ทธ์  ๋์๊ฐ๋ ์ฝ๋๋ง์ผ๋ก๋ ๋ถ์กฑํ๋ค._

---
