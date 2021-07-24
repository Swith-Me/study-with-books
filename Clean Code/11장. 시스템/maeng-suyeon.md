> "복잡성은 죽음이다. 개발자에게서 생기를 앗아가며, 제품을 계획하고 제작하고 테스트하기 어렵게 만든다."  - 레이 오지; Ray Ozzie, 마이크로소프트 최고 기술 책임자; CTO

## 도시를 세운다면?

적절한 추상화와 모듈화 때문에 도시가 돌아가고 개인과 개인이 관리하는 '구성요소'가 효율적으로 돌아간다.

흔히 소프트웨어 팀도 도시처럼 구성하지만, 제작하는 시스템이 관심사를 분리하거나 추상화를 이뤄내지 못한다.  

✔️ 깨끗한 코드를 구현하면 낮은 추상화 수준에서 관심사를 분리하기 쉬워진다.  

## 시스템 제작과 시스템 사용을 분리하라

우선 **제작; construction**은 **사용; use**과 아주 다르다는 사실을 명심하자.

소프트웨어 시스템은 애플리케이션 객체를 제작하고, 의존성을 서로 '연결'하는 **준비 과정과** 준비 과정 이후에 이어지는 **런타임 로직을 분리해야 한다.**  

시작 단계는 모든 애플리케이션이 풀어야 할 **관심사; concern**이며, **관심사 분리**는 이 분야에서 가장 오래되고 중요한 설계 기법 중 하나다.

```java
// "Lazy Initialization/Evaluation(게으른 초기화)"의 일반적인 형태
public Service getService() {
      if (service == null)
          service = new MyServiceImpl(...); // 모든 상황에 적합한 기본값일까?
      return service;
  }
```

이는 불필요한 초기화 코스트의 최적화, `null` 반환 방지 등의 이점을 가지는 코드이다. 

하지만 이 코드로 인해 우리의 시스템은 `MyServiceImpl` 객체에 대한 의존성을 가지게 되었고 `MyServiceImpl`의 사용 여부와 관계 없이 무조건 이 의존성을 만족해야 하게 되었다. 

테스트 수행에도 문제가 발생한다. 

만약 `MyServiceImpl` 객체가 무거운 객체라면 테스트를 위한 `Test Double 1 / Mock Object`를 `service` 필드에 대입해야 하며, 이는 기존의 `runtime` 로직에 관여하기 때문에 모든 가능한 경우의 수를 고려해야 하는 문제도 발생한다.  

이러한 생성/사용의 분산은 모듈성을 저해하고 코드의 중복을 가져오므로,

잘 정돈된 견고한 시스템을 만들기 위해서는 **전역적이고 일관된 의존성 해결 방법**을 통해 위와 같은 작은 편의 코드들이 **모듈성의 저해를 가져오는 것을 막아야 한다.**  

### Main 분리

![image](https://user-images.githubusercontent.com/46524540/126873361-fc70cb00-6650-4d82-8a23-0f3dcb43e855.png)

생성과 사용을 분리하는 가장 간단한 방법은 모든 생성과 관련된 로직을 main으로 옮기는 것이다. 

어플리케이션에서는 사용할 모든 객체들이 main에서 잘 생성되었을 것이라 여기고 나머지 디자인에 집중할 수 있다.  

### 팩토리

![image](https://user-images.githubusercontent.com/46524540/126873374-7bd3f58b-1a31-4d25-a7c7-0a716ef8286c.png)

객체의 생성 시기를 직접 결정하려면 main에서 완성된 객체를 던져주기 보다 factory 객체를 만들어서 던져주자.

만약 자세한 구현을 숨기고 싶다면 **Abstract Factory 패턴**을 사용하자.  

### 의존성 주입; Dependency Injection

: 제어 역전; Inversion of Control, IoC 기법을 의존성 관리에 적용한 메커니즘

제어 역전에서는 한 객체가 맡은 보조 책임을 새로운 객체에게 전적으로 떠넘긴다.

새로운 객체는 넘겨받은 책임만 맡으므로 **단일 책임 원칙; Single Responsibility Principle, SRP**을 지키게 된다.

```java
// JNDI 검색을 이용해 의존성 주입을 '부분적으로' 구현한 기능
// 객체는 디렉터리 서버에 이름 제공, 그 이름에 일치하는 서비스 요청함
MyService myService = (MyService)(jndiContext.lookup(“NameOfMyService”));
```

위 코드를 호출하는 쪽에서는 실제로 `lookup` 메서드가 무엇을(어떤 구현체를) 리턴하는지에 대해 관여하지 않으면서 의존성을 해결할 수 있다.  

진정한 의존성 주입은 여기에서 한 단계 더 나아가 완전히 수동적인 형태를 지닌다. 

의존성을 필요로 하는 객체가 직접 의존성을 해결(생성, 연결)하는 대신 `생성자/setter` 등을 통해 DI 컨테이너가 해당 의존성을 해결하도록 도와준다. (DI / IoC)  

## 확장

반복적이고, 점진적인 **애자일 방식**의 핵심인 주어진 사용자 스토리에 맞춰 시스템을 구현하고, 다음엔 새로운 스토리에 맞춰 시스템을 조정하고 확장한다.

**테스트 주도 개발; Test-driven Development, TDD, 리팩터링, 깨끗한 코드**는 코드 수준에서 시스템을 조정하고 확장하기 쉽게 만든다.

소프트웨어 시스템 또한 마찬가지이다. 만약 우리가 Concern들을 적절히 분리할 수 있다면, 소프트웨어 시스템은 물리적인 시스템(ex, 건축)과는 다르게 점진적으로 커질 수 있다.  

스케일링을 고려하지 않은 구조에 대해 EJB1/EJB2를 예시로 알아보자.

```java
// Bank EJB용 EJB2 지역 인터페이스
package com.example.banking;
import java.util.Collections;
import javax.ejb.*;

public interface BankLocal extends java.ejb.EJBLocalObject {
    String getStreetAddr1() throws EJBException;
    String getStreetAddr2() throws EJBException;
    String getCity() throws EJBException;
    String getState() throws EJBException;
    String getZipCode() throws EJBException;
    void setStreetAddr1(String street1) throws EJBException;
    void setStreetAddr2(String street2) throws EJBException;
    void setCity(String city) throws EJBException;
    void setState(String state) throws EJBException;
    void setZipCode(String zip) throws EJBException;
    Collection getAccounts() throws EJBException;
    void setAccounts(Collection accounts) throws EJBException;
    void addAccount(AccountDTO accountDTO) throws EJBException;
}
```

   

```java
// 상응하는 EJB2 엔티티 빈 구현
package com.example.banking;
import java.util.Collections;
import javax.ejb.*;

public abstract class Bank implements javax.ejb.EntityBean {
    // 비즈니스 논리
    public abstract String getStreetAddr1();
    public abstract String getStreetAddr2();
    public abstract String getCity();
    public abstract String getState();
    public abstract String getZipCode();
    public abstract void setStreetAddr1(String street1);
    public abstract void setStreetAddr2(String street2);
    public abstract void setCity(String city);
    public abstract void setState(String state);
    public abstract void setZipCode(String zip);
    public abstract Collection getAccounts();
    public abstract void setAccounts(Collection accounts);
    
    public void addAccount(AccountDTO accountDTO) {
        InitialContext context = new InitialContext();
        AccountHomeLocal accountHome = context.lookup("AccountHomeLocal");
        AccountLocal account = accountHome.create(accountDTO);
        Collection accounts = getAccounts();
        accounts.add(account);
    }
    
    // EJB 컨테이너 논리
    public abstract void setId(Integer id);
    public abstract Integer getId();
    public Integer ejbCreate(Integer id) { ... }
    public void ejbPostCreate(Integer id) { ... }
    
    // 나머지도 구현해야 하지만 일반적으로 비어있다.
    public void setEntityContext(EntityContext ctx) {}
    public void unsetEntityContext() {}
    public void ejbActivate() {}
    public void ejbPassivate() {}
    public void ejbLoad() {}
    public void ejbStore() {}
    public void ejbRemove() {}
}
```

위 코드와 같은 전형적인 EJB2 객체 구조는 아래와 같은 문제점을 가지고 있다.  

1. 비지니스 로직이 EJB2 컨테이너에 타이트하게 연결되어 있다. Entity를 만들기 위해 컨테이너 타입을 subclass하고 필요한 `lifecycle` 메서드를 구현해야 한다.
2. 실제로 사용되지 않을 테스트 객체의 작성을 위해 `mock` 객체를 만드는 데에도 무의미한 노력이 많이 든다. EJB2 구조가 아닌 다른 구조에서 재사용할 수 없는 컴포넌트를 작성해야 한다.
3. **OOP** 또한 등한시되고 있다. 상속도 불가능하며 쓸데없는 DTO(Data Transfer Object)를 작성하게 만든다.

### 횡단; cross-cutting 관심사

cross-cutting : 이론적으로는 독립된 형태로 구분될 수 있지만 실제로는 코드에 산재하기 쉬운 부분들 (transaction, authorization, logging ...)  

영속성과 같은 관심사는 애플리케이션의 자연스러운 객체 경계를 넘나드는 경향이 있다.

그래서 모든 객체가 전반적으로 동일한 방식을 이용하게 만들어야 한다.

- ex- 특정 DBMS / 독자적인 파일 사용, 같은 명명 관례를 따른 테이블과 열, 일관적인 트랜잭션 의미

원론적으로는 모듈화되고 캡슐화된 방식으로 영속성 방식을 구상할 수 있지만, 현실적으로는 영속성 방식을 구현한 코드가 온갖 객체로 흩어져서 **횡단 관심사**란 용어가 등장했다.   

## 자바 프록시

자바 프록시는 단순한 상황에 적합해 개별 객체나 클래스에서 메서드 호출을 감싸는 경우가 좋은 예다.  

```java
// 자바 프록시를 사용해 객체의 변경이 자동으로 persistant framework에 저장되는 구조
// Bank.java (패키지 이름을 감춘다.)
import java.utils.*;

// 은행 추상화
public interface Bank {
    Collection<Account> getAccounts();
    void setAccounts(Collection<Account> accounts);
}

// BankImpl.java
import java.utils.*;

// 추상화를 위한 POJO(“Plain Old Java Object”) 구현
public class BankImpl implements Bank {
    private List<Account> accounts;

    public Collection<Account> getAccounts() {
        return accounts;
    }
    
    public void setAccounts(Collection<Account> accounts) {
        this.accounts = new ArrayList<Account>();
        for (Account account: accounts) {
            this.accounts.add(account);
        }
    }
}
// BankProxyHandler.java
import java.lang.reflect.*;
import java.util.*;

// 프록시 API가 필요한 "InvocationHandler"
public class BankProxyHandler implements InvocationHandler {
    private Bank bank;
    
    public BankHandler (Bank bank) {
        this.bank = bank;
    }
    
    // InvocationHandler에 정의된 메서드
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        String methodName = method.getName();
        if (methodName.equals("getAccounts")) {
            bank.setAccounts(getAccountsFromDatabase());
            
            return bank.getAccounts();
        } else if (methodName.equals("setAccounts")) {
            bank.setAccounts((Collection<Account>) args[0]);
            setAccountsToDatabase(bank.getAccounts());
            
            return null;
        } else {
            ...
        }
    }
    
    // 세부사항은 여기에 이어진다.
    protected Collection<Account> getAccountsFromDatabase() { ... }
    protected void setAccountsToDatabase(Collection<Account> accounts) { ... }
}

// 다른 곳에 위치하는 코드
Bank bank = (Bank) Proxy.newProxyInstance(
    Bank.class.getClassLoader(),
    new Class[] { Bank.class },
    new BankProxyHandler(new BankImpl())
);
```

위에서는 프록시로 감쌀 인터페이스 Bank와 비즈니스 논리를 구현하는 POJO; Plain Old Java Object **BankImpl**을 정의했다.

단순한 예제지만, 코드가 상당히 많으며 복잡하다. 바이트 조작 라이브러리를 사용하더라도 만만찮게 어렵다.

<b>코드 '양'</b>과 <b>'크기'</b>는 프록시의 두 가지 단점으로, 깨끗한 코드를 작성하기 어려우며 시스템 단위로 실행 '지점'을 명시하는 메커니즘도 제공하지 않는다.  

## 순수 자바 AOP 프레임워크

위 Java Proxy API의 단점들은 Spring, JBoss와 같은 순수 자바 AOP 프레임워크를 통해 해결할 수 있다. 

예를 들어 Spring에서는 비지니스 로직을 POJO로 작성해 자신이 속한 도메인에 집중하게 한다. 

결과적으로 의존성은 줄어들고 테스트 작성에 필요한 고민도 줄어든다. 이러한 심플함은 user story의 구현과 유지보수, 확장 또한 간편하게 만들어 준다.

```java
// 스프링 2.X 설정 파일
<beans>
    ...
    <bean id="appDataSource"
        class="org.apache.commons.dbcp.BasicDataSource"
        destroy-method="close"
        p:driverClassName="com.mysql.jdbc.Driver"
        p:url="jdbc:mysql://localhost:3306/mydb"
        p:username="me"/>
    
    <bean id="bankDataAccessObject"
        class="com.example.banking.persistence.BankDataAccessObject"
        p:dataSource-ref="appDataSource"/>
    
    <bean id="bank"
        class="com.example.banking.model.Bank"
        p:dataAccessObject-ref="bankDataAccessObject"/>
    ...
</beans>
```

`Bank` 도메인 객체는 자료 접근자 객체; Data Accessor Object, DAO로 프록시 되었으며, 자료 접근자 객체는 JDBC 드라이버 자료 소스로 프록시 되었다. (하단 그림 참조)

![image](https://user-images.githubusercontent.com/46524540/126873407-f1571670-f389-438d-9795-95258c322753.png)

이 프록시된 Bank객체를 생성하는 방법은 아래와 같다.

```java
XmlBeanFactory bf = new XmlBeanFactory(new ClassPathResource("app.xml", getClass()));
Bank bank = (Bank) bf.getBean("bank");
```

위와 같이 최소한의 **Spring-specific**한 코드만 작성하면 되므로, 프레임워크와 "거의" **decouple**된 어플리케이션을 작성할 수 있다.  

EJB3로 `Bank` 객체를 다시 작성해보면...

```java
package com.example.banking.model;

import javax.persistence.*;
import java.util.ArrayList;
import java.util.Collection;

@Entity
@Table(name = "BANKS")
public class Bank implements java.io.Serializable {
    @Id @GeneratedValue(strategy=GenerationType.AUTO)
    private int id;
    
    @Embeddable // An object “inlined” in Bank’s DB row
    public class Address {
        protected String streetAddr1;
        protected String streetAddr2;
        protected String city;
        protected String state;
        protected String zipCode;
    }
    
    @Embedded
    private Address address;
    @OneToMany(cascade = CascadeType.ALL, fetch = FetchType.EAGER, mappedBy="bank")
    private Collection<Account> accounts = new ArrayList<Account>();
    public int getId() {
        return id;
    }
    
    public void setId(int id) {
        this.id = id;
    }
    
    public void addAccount(Account account) {
        account.setBank(this);
        accounts.add(account);
    }
    
    public Collection<Account> getAccounts() {
        return accounts;
    }
    
    public void setAccounts(Collection<Account> accounts) {
        this.accounts = accounts;
    }
}
```

원래 EJB2 코드보다 훨씬 더 깨끗하고 명료한 코드를 산출해 유지보수, 테스트하기 편한 장점을 갖게 되었다.  

## AspectJ 관점

관심사를 관점으로 분리하기 위해 AspectJ 언어를 사용한다.

- AspectJ : 언어 차원에서 관점을 모듈화 구성으로 지원하는 자바 언어 확장

하지만, 새 도구 / 언어 문법, 사용법을 익혀야 한다는 단점이 있다.

이를 보완하기 위해 AspectJ '애너테이션 폼'이 등장했다.

- 애너테이션 폼 : 순수한 자바 코드에 자바 5 애너테이션을 사용해 관점 정의

## 테스트 주도 시스템 아키텍처 구축

- 코드 레벨에서부터 아키텍쳐와 분리된(decouple된) 프로그램 작성은 당신의 아키텍쳐를 test drive하기 쉽게 만들어 준다.
- 처음에는 작고 간단한 구조에서 시작하지만, 필요에 따라 새로운 기술을 추가해 정교한 아키텍쳐로 진화할 수 있다.
- 또한 decouple된 코드는 user story, 규모 변화와 같은 변경사항에 더 빠르게 대처할 수 있도록 우리를 도와 준다.
- 도리어 BDUF(Big Design Up First)와 같은 방식은 변경이 생길 경우 기존의 구조를 버려야 한다는 심리적 저항, 아키텍쳐에 따른 디자인에 대한 고민 등 변화에 유연하지 못한 단점들을 가져오게 된다.
- 초기 EJB와 같이 너무 많은 엔지니어링이 가미되어 많은 concern들을 묶어버리지 않으며 오히려 많은 부분들을 숨기는 것이 아름다운 구조를 가져올 것이다.

✔️ 이상적인 시스템 아키텍쳐는 각각 POJO로 만들어진 모듈화된 관심 분야 영역(modularized domains of concern)으로 이루어져야 한다. 

다른 영역끼리는 Aspect의 개념을 사용해 최소한의 간섭으로 통합되어야 한다. 이러한 아키텍쳐는 코드와 마찬가지로 test-driven될 수 있다.  

## 의사 결정을 최적화하라

- 모듈을 나누고 관심사를 분리하면 지엽적인 관리와 결정이 가능해진다.
- 우리는 최대한 정보를 모아 최선의 결정을 내리기 위해 **가능한 마지막 순간까지 결정을 미루는 방법**이 최선이라는 사실을 까먹는다.
- 성급한 결정은 불충분한 지식으로 내린 결정이다.

    너무 일찍 결정하면 고객 피드백을 더 모으고, 프로젝트를 더 고민하고, 구현 방안을 더 탐험할 기회가 사라진다.

✔️ 이 때문에 POJO 시스템을 사용해 최신 정보에 기반한 최선의 시점에 최적의 결정을 내려 결정의 복잡성을 줄인다.

## 명백한 가치가 있을 때 표준을 현명하게 사용하라

단지 표준이라는 이유만으로 EJB2를 많은 팀이 사용했지만, **아주 과장되게 포장된 표준**에 집착하는 바람에 고객 가치가 뒷전으로 밀려난 사례가 종종 나타났다.  

표준을 사용하면...

- 아이디어와 컴포넌트를 재사용하기 쉽다.
- 적절한 경험을 가진 사람을 구하기 쉽다.
- 좋은 아이디어를 캡슐화하기 쉽다.
- 컴포넌트를 엮기 쉽다.
- 표준 만드는 시간이 너무 오래걸려 업계가 기다리지 못한다.
- 어떤 표준은 원래 표준을 제정한 목적을 잊어버리기도 한다.

## 시스템은 도메인 특화 언어가 필요하다

대다수 도메인과 마찬가지로, 건축 분야도 필수적인 정보를 명료하고 정확하게 전달하는 어휘, 관용구, 패턴이 풍부하다.

- 소프트웨어 분야에서 **DSL; Domain-Specific Language**가 등장해 새롭게 조명받기 시작했다.
    - DSL : 간단한 스크립트 OR 표준 언어로 구현한 API. 구조적인 산문처럼 읽힌다.
- 좋은 DSL은 도메인 개념과 그것을 구현한 코드 사이에 존재하는 '의사소통간극'을 줄여준다.
    - = 애자일 기법
- 도메인 전문가가 사용하는 언어로 도메인 논리를 구현해 잘못 구현할 가능성을 줄여준다.
- 추상화 수준을 코드 관용구나 디자인 패턴 이상으로 끌어올려 적절한 추상화 수준에서 코드 의도를 표현할 수 있다.
- 고차원 정책 → 저차원 세부사항 까지 모든 추상화 수준과 도메인을 POJO로 표현할 수 있다.

## 결론

- 깨끗하지 못한 아키텍처는 도메인 논리를 흐리며 기민성을 떨어뜨린다.
    - 도메인 논리가 흐려지면 떨어진 제품 품질과 버그가 숨어들기 쉽고, 스토리 구현이 어렵다.
    - 기민성이 떨어지면 생산성이 낮아져 TDD가 제공하는 장점이 사라진다.
- 모든 추상화 단계에서 의도는 명확히 표현해야 한다.
    - POJO를 작성하고, 관점 혹은 관점과 유사한 메커니즘을 사용해 각 구현 관심사를 분리한다.

✔️ 실제로 돌아가는 **가장 단순한 수단**을 사용해야 한다는 사실을 명심하자.
