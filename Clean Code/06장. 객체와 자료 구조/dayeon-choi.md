# π [6μ₯] κ°μ²΄μ μλ£κ΅¬μ‘°

---

## μλ£ μΆμν

- λ³μ μ¬μ΄μ ν¨μλΌλ κ³μΈ΅μ λ£λλ€κ³  κ΅¬νμ΄ μ μ λ‘ κ°μΆ°μ§μ§λ μλλ€. β μΆμν νμν μ΄μ 
- **μΆμ μΈν°νμ΄μ€**λ₯Ό μ κ³΅ν΄ μ¬μ©μκ° **κ΅¬νμ λͺ¨λ₯Έ μ±** μλ£μ ν΅μ¬μ μ‘°μν  μ μμ΄μΌ μ§μ ν μλ―Έμ ν΄λμ€
- μλ£λ₯Ό μΈμΈνκ² κ³΅κ°νκΈ°λ³΄λ€ **μΆμμ μΈ κ°λμΌλ‘ νν**νλ κ²μ΄ μ’λ€.

```java
public interface Vehicle{
	double getPercentFuelRemaining();
}
```

---

## μλ£/κ°μ²΄ λΉλμΉ­

- κ°μ²΄μ μλ£ κ΅¬μ‘°λ κ·Όλ³Έμ μΌλ‘ μλΆλλ€.
- κ°μ²΄ μ§ν₯ μ½λμ μ μ°¨ μ§ν₯ μ½λλ μνΈ λ³΄μμ μΈ νΉμ§μ΄ μλ€.
- μλ‘μ΄ **μλ£ νμ**μ΄ νμν  λ β **κ°μ²΄ μ§ν₯ μ½λ** μ¬μ©
- μλ‘μ΄ **ν¨μ**κ° νμν  λ β **μ μ°¨ μ§ν₯ μ½λ** μ¬μ©

---

## λλ―Έν° λ²μΉ

- λλ―Έν° λ²μΉμ μ μλ €μ§ <mark style='background-color: #fff5b1'>ν΄λ¦¬μ€ν±</mark>μ΄λ€.
- **λλ―Έν° λ²μΉ**μ΄λ λͺ¨λμ μμ μ΄ μ‘°μνλ **κ°μ²΄μ μμ¬μ μ λͺ°λΌμΌ νλ€**λ λ²μΉμ΄λ€.
- ν΄λμ€ Cμ λ©μλ fλ λ€μκ³Ό κ°μ κ°μ²΄μ λ©μλλ§ νΈμΆν΄μΌ νλ©°, λ€μκ³Ό κ°μ <mark style='background-color: #ffdce0'>κ°μ²΄μμ νμ©λ λ©μλκ° λ°ννλ λ©μλλ νΈμΆ</mark>νλ©΄ μλλ€.
  1. ν΄λμ€ C
  2. fκ° μμ±ν κ°μ²΄
  3. fμΈμλ‘ λμ΄μ¨ κ°μ²΄
  4. CμΈμ€ν΄μ€ λ³μμ μ μ₯λ κ°μ²΄

<br/>
 π ν΄λ¦¬μ€ν± : κ°νΈ μΆλ‘ μ λ°©λ²μΌλ‘, μ²΄κ³μ μ΄λ©΄μ ν©λ¦¬μ μΈ νλ¨μ΄ κ΅³μ΄ νμνμ§ μμ μν©μμ μ¬λλ€μ΄ λΉ λ₯΄κ² μ¬μ©ν  μ μκ² κ΅¬μ±λ κ² μ΄λ€.
 
<br/>
<br/>

### 1) κΈ°μ°¨ μΆ©λ

- β : μΌλ°μ μΌλ‘ μ‘°μ‘νλ€ μ¬κ²¨μ§λ λ°©μ
- μ¬λ¬ κ°μ²΄κ° ν μ€λ‘ μ΄μ΄μ§ κΈ°μ°¨μ²λΌ λ³΄μ΄λ μ½λ.

```java
final String outputDir = ctxt.getOptions().getScratchDir().getAbsolutePath();
```

<center>β</center>

```java
Options opts = ctxt.getOptions();
File scratchDir = opts.getScratchDir();
final String outputDir = ctxt.options.scratchDir.absolutePath;
```

### 2) κ΅¬μ‘°μ²΄ κ°μΆκΈ°

- λ§μ½ `ctxt`, `scratchDir` κ°μ²΄κ° μ§μ§λΌλ©΄ μ μ½λμ²λΌ μ€μ€μ΄ μ?μ΄μλ X
- λλ ν°λ¦¬μ κ²½λ‘λ₯Ό μ½λλ‘ μμ±ν  λ μ , μ¬λμ, νμΌ νμ₯μ, File κ°μ²΄λ₯Ό λΆμ£Όμνκ² λ€μμ΄μλ X
<center>β</center>
<center>πβοΈ  ctxt κ°μ²΄μ μμ νμΌμ μμ±νλΌκ³  μν€λ κ²!</center>

---

## μλ£ μ λ¬ κ°μ²΄

- μλ£ κ΅¬μ‘°μ²΄μ μ νμ μΈ νν : **κ³΅κ° λ³μλ§** μκ³  ν¨μκ° μλ ν΄λμ€, μ΄λ₯Ό λλ‘λ **<mark style='background-color: #fff5b1'>DTO</mark>** λΌκ³  νλ€.
- μ μκ° λ§νλ DTOλ
  - λ°μ΄ν°λ² μ΄μ€μ ν΅μ νκ±°λ μμΌμμ λ°μ λ©μμ§μ κ΅¬λ¬Έμ λΆμν  λ μ μ©
  - λ°μ΄ν°λ² μ΄μ€μ μ μ₯λ κ°κ³΅λμ§ μμ μ λ³΄λ₯Ό μ νλ¦¬μΌμ΄μ μ½λμμ μ¬μ©ν  κ°μ²΄λ‘ λ³ννλ λ¨κ³μμ κ°μ₯ μ²μμΌλ‘ μ¬μ©νλ κ΅¬μ‘°μ²΄
  - μΌλ°μ μΈ ννλ bean(λΉ) κ΅¬μ‘° <span style='color:gray'>βλΉμ λΉκ³΅κ° λ³μλ₯Ό μ‘°ν/μ€μ  ν¨μλ‘ μ‘°μνλ€. μ μλ λΉμΆνλ λ― νλ€.</span>

<br/>
 π DTO(μλ£ μ λ¬ κ°μ²΄, λ°μ΄ν° μ μ‘ κ°μ²΄) : νλ‘μΈμ€ κ°μ λ°μ΄ν°λ₯Ό μ λ¬νλ κ°μ²΄
 
<br/>
<br/>

### 1. νμ± λ μ½λ

- DTOμ νΉμν νν <span style='color:gray'>β<mark style='background-color: #fff5b1'>μΆκ°μ </mark>μΌλ‘ λκ² saveλ findμ κ°μ νμ ν¨μλ μ κ³΅</span>
- λ°μ΄ν°λ² μ΄μ€ νμ΄λΈμ΄λ λ€λ₯Έ μμ€μμ μλ£λ₯Ό μ§μ  λ³νν κ²°κ³Ό
- νμ§λ§, <mark style='background-color: #ffdce0'>νμ± λ μ½λμ λΉμ¦λμ€ κ·μΉμ μΆκ°ν΄ κ°μ²΄λ‘ μ·¨κΈνλ κ²μ **λμ°ν μ‘μ’ κ΅¬μ‘°**κ° λμ€λ κ²μ΄λ―λ‘ μ³μ§ μλ€.</mark>
<center>β</center>
<center>πβοΈ  νμ± λ μ½λλ μλ£ κ΅¬μ‘°λ‘ μ·¨κΈνμ!</center> 
<center>λΉμ¦λμ€ κ·μΉμ λ΄μΌλ©΄μ λ΄λΆ μλ£λ₯Ό μ¨κΈ°λ κ°μ²΄λ λ°λ‘ μμ±νκΈ°!</center>


<br/>
 π μΆκ°μ μΌλ‘ - κΈ°λ³Έμ μΌλ‘λ κ³΅κ° λ³μκ° μκ±°λ λΉκ³΅κ° λ³μμ μ‘°ν/μ€μ  ν¨μκ° μλ μλ£ κ΅¬μ‘°

---

## κ²°λ‘ 

- κ°μ²΄λ λμμ κ³΅κ°νκ³  μλ£λ₯Ό μ¨κΉ β λ³κ²½ μμ΄ κ°μ²΄ νμ μΆκ°λ μ¬μ
- μλ£ κ΅¬μ‘°λ λ³ λ€λ₯Έ λμ μμ΄ μλ£λ₯Ό λΈμΆ β μ μλ£ κ΅¬μ‘° μΆκ°λ μ΄λ €μ
- **μ΄λ€ μ μ°μ±μ΄ νμνκ°μ λ°λΌ μ΅μ μ ν΄κ²°μ±μ μ ννλΌ!**

---

## μΈμ κΉμλ...

> _κ°λ°μλ κ°μ²΄κ° ν¬ν¨νλ μλ£λ₯Ό ννν  κ°μ₯ μ’μ λ°©λ²μ μ¬κ°νκ² κ³ λ―Όν΄μΌ νλ€. μλ¬΄ μκ° μμ΄ μ‘°ν/μ€μ  ν¨μλ₯Ό μΆκ°νλ λ°©λ²μ΄ κ°μ₯ λμλ€._

> _μ°μν μννΈμ¨μ΄ κ°λ°μλ νΈκ²¬ μμ΄ μ΄ μ¬μ€(μ μ°¨μ  μ½λμ κ°μ²΄μ νΉμ§)μ μ΄ν΄ν΄ μ§λ©΄ν λ¬Έμ μ μ΅μ μΈ ν΄κ²°μ±μ μ ννλ€._

---
