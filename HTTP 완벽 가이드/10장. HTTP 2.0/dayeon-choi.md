# πΏ [10μ₯] HTTP/2.0

---

## 10.1 HTTP/2.0μ λ±μ₯ λ°°κ²½

- HTTP/1.1μ λ©μμ§ ν¬λ§·μ κ΅¬νμ λ¨μμ±, μ κ·Όμ±μ μ£Όμμ±μ λκ³  μ΅μ ν
- HTTP/1.1 νΉμ§ & λ¬Έμ 
  - μ»€λ₯μ νλλ₯Ό ν΅ν΄ μμ²­ νλλ₯Ό λ³΄λ΄κ³  κ·Έμ λν΄ μλ΅ νλλ§ λ°μ
  - νμ  μ§μ°(latency) λ¬Έμ . μλ΅ λ°μμΌλ§ λ€μ μμ²­μ λ³΄λΌ μ μμ
  - ν΄κ²°μ μν λ³λ ¬ μ»€λ₯μ, νμ΄νλΌμΈ μ»€λ₯μμ΄ λμμΌλ‘λ λΆμ‘±
- ν΄κ²° λΈλ ₯
  - HTTP-NG νλ‘μ νΈ
  - WAKA νλ‘ν μ½ μ μ
  - S+M(Speed+Mobility) νλ‘ν μ½ κ°λ°
  - SPDY(μ€νΌλ)
  - β SPDYμ μ΄μμ κ°μ Έμ HTTP/2.0 λ§λ€κΈ° μμ

---

## 10.2 κ°μ

- HTTP/2.0μ μλ²μ ν΄λΌμ΄μΈνΈ μ¬μ΄μ TCP μ»€λ₯μ μμμ λμ
  - TCP μ΄κΈ°νλ ν΄λΌμ΄μΈνΈκ° ν¨
  - μμ²­κ³Ό μλ΅μ μ μλ(<=16383byte) ν κ° μ΄μμ νλ μμ λ΄κΉ
  - HTTP ν€λλ μμΆλμ΄ λ΄κΉ
  - νλ μλ€μ λ΄κΉ μμ²­κ³Ό μλ΅μ μ€νΈλ¦Όμ ν΅ν΄ λ³΄λ΄μ§
  - νλμ μ»€λ₯μ μμ μ¬λ¬ κ°μ μ€νΈλ¦Όμ΄ λμμ λ§λ€μ΄μ§ μ μμ
    β μ¬λ¬ κ°μ μμ²­/μλ΅ λμ μ²λ¦¬ κ°λ₯
- κΈ°μ‘΄ μΉ μ νλ¦¬μΌμ΄μλ€κ³Ό νΈνμ±μ μ΅λν μ μ§νκΈ° μν΄ μμ²­/μλ΅ λ©μμ§ μλ―Έλ₯Ό HTTP/1.1κ³Ό κ°λλ‘ μ μ§

---

## HTTP/1.1κ³Όμ μ°¨μ΄μ 

### 10.3.1 νλ μ

- HTTP/2.0μμ λͺ¨λ  λ©μμ§λ νλ μμ λ΄κ²¨ μ μ‘
- λͺ¨λ  νλ μμ **8λ°μ΄νΈ** ν¬κΈ°μ **ν€λ**λ‘ μμ
- λ€μ΄μ΄ μ΅λ **16383λ°μ΄νΈ** ν¬κΈ°μ **νμ΄λ‘λ**κ° μ΄

> **νλ μ ν€λμ κ° νλ**

- R
  - μμ½λ 2λΉνΈ νλ. κ°μ μλ―Έ μ μ μλ¨. λ°λμ 0. λ°λ μͺ½μμλ μ΄ κ° λ¬΄μν΄μΌ ν¨
- κΈΈμ΄
  - νμ΄λ‘λμ κΈΈμ΄λ₯Ό λνλ΄λ 14λΉνΈ λ¬΄λΆνΈ μ μ. νλ μ ν€λμ ν¬ν¨λμ§ μλ κΈΈμ΄
- μ’λ₯
  - νλ μμ μ’λ₯
- νλκ·Έ
  - 8λΉνΈ νλκ·Έ. κ°μ μλ―Έλ νλκ·Έ μ’λ₯μ λ°λ¦
- R
  - μμ½λ 1λΉνΈ λΉνΈ. μ²« λ²μ§Έ Rκ³Ό λ§μ°¬κ°μ§λ‘ μλ―Έ μ μ μλ¨.
- μ€νΈλ¦Ό μλ³μ
  - 31λΉνΈ μ€νΈλ¦Ό μλ³μ. νΉλ³ν 0μ μ»€λ₯μ μ μ²΄μ μ°κ΄λ νλ μμ μλ―Έ

### 10.3.2 μ€νΈλ¦Όκ³Ό λ©ν°νλ μ±

- μ€νΈλ¦Όμ HTTP/2.0 μ»€λ₯μμ ν΅ν΄ ν΄λΌμ΄μΈνΈμ μλ² μ¬μ΄μμ κ΅νλλ νλ μλ€μ λλ¦½λ μλ°©ν₯ μνμ€
- ν μμ HTTP μμ²­κ³Ό μλ΅μ νλμ μ€νΈλ¦Όμ ν΅ν΄ μ΄λ£¨μ΄μ§
- HTTP/2.0μμλ νλμ μ»€λ₯μμ μ¬λ¬ κ°μ μ€νΈλ¦Όμ΄ λμμ μ΄λ¦΄ μ μμ
- μ€νΈλ¦Όμ μ°μ μμλ κ°μ§ μ μμ
- HTTP/2.0 μ»€λ₯μμμ νλ² μ¬μ©ν μ€νΈλ¦Ό μλ³μλ λ€μ μ¬μ©ν  μ μμ
  - μ΄λ μ€νΈλ¦Όμ ν λΉν  μ μλ μλ³μκ° κ³ κ°λ  μ μμ
  - μ»€λ₯μμ λ€μ λ§ΊμΌλ©΄ ν΄κ²°λ¨

### 10.3.3 ν€λ μμΆ

- HTTP/1.1μμ ν€λλ μλ¬΄λ° μμΆ μμ΄ μ μ‘
- μΉνμ΄μ§ νλλ₯Ό λ³΄κΈ° μν μμ²­μ κ°μκ° λ§μμ Έ ν€λμ ν¬κΈ°κ° λ¬Έμ κ° λ¨
- μ΄λ₯Ό κ°μ νκΈ° μν΄ HTTP/2.0μμλ HTTP λ©μμ§μ ν€λλ₯Ό μμΆνμ¬ μ μ‘
- HPACK λͺμΈμ μ μλ ν€λ μμΆ λ°©λ²μΌλ‘ μμΆλ λ€ **ν€λ λΈλ‘ μ‘°κ°**λ€λ‘ μͺΌκ° μ μ‘
- λ°λ μͺ½μμλ μ΄ μ‘°κ°λ€μ μ΄μ λ€ μμΆμ νμ΄ μλμ ν€λ μ§ν©μΌλ‘ λ³΅μ
- μμΆ, ν΄μ  μ **μμΆ μ½νμ€νΈ**λ₯Ό μ¬μ©

### 10.3.4 μλ² νΈμ¬

- HTTP/2.0μ μλ²κ° νλμ μμ²­μ λν΄ μλ΅μΌλ‘ μ¬λ¬ κ°μ λ¦¬μμ€λ₯Ό λ³΄λΌ μ μλλ‘ ν¨
- μ΄λ€ λ¦¬μμ€λ₯Ό μκ΅¬ν  κ²μΈμ§ λ―Έλ¦¬ μ μ μλ μν©μμ μ μ©
- λ¦¬μμ€λ₯Ό νΈμ¬νλ €λ μλ²λ λ¨Όμ  ν΄λΌμ΄μΈνΈμκ² μμμ νΈμ¬ν  κ²μμ `PUSH_PROMISE` νλ μμΌλ‘ λ―Έλ¦¬ μλ¦Ό
- ν΄λΉ νλ μ μ€νΈλ¦Όμ μμ½λ¨(μκ²©) μνκ° λ¨
- μ΄ μνμμ ν΄λΌμ΄μΈνΈλ `RST_STREAM` νλ μμ λ³΄λ΄μ΄ νΈμ¬ κ±°μ  κ°λ₯

---

## 10.4 μλ €μ§ λ³΄μ μ΄μ

### 10.4.1 μ€μ¬μ μΊ‘μν κ³΅κ²©

- HTTP/2.0 λ©μμ§λ₯Ό μ€κ°μ νλ½μκ° HTTP/1.1 λ©μμ§λ‘ λ³νν  λ λ©μμ§μ μλ―Έκ° λ³μ§λ  κ°λ₯μ± μμ
- HTTP/1.1κ³Όλ λ¬λ¦¬ HTTP/2.0μ ν€λ νλμ μ΄λ¦κ³Ό κ°μ λ°μ΄λλ¦¬λ‘ μΈμ½λ©
- λ€νν HTTP/1.1 λ©μμ§λ₯Ό λ²μ­νλ κ³Όμ μμ λ²μ­ λ¬Έμ κ° λ°μνμ§λ μμ

### 10.4.2 κΈ΄ μ»€λ₯μ μ μ§λ‘ μΈν κ°μΈμ λ³΄ λμΆ μ°λ €

- HTTP/2.0μ μ¬μ©μκ° μμ²­μ λ³΄λΌ λμ νμ  μ§μ°μ μ€μ΄κΈ° μν΄ ν΄λΌμ΄μΈνΈμ μλ² μ¬μ΄μ μ»€λ₯μμ μ€λ μ μ§νλ κ²μ μΌλ
- κ°μΈ μ λ³΄ μ μΆμ μμ©λ  κ°λ₯μ± μμ
- μ΄λ HTTPκ° νμ¬ κ°κ³  μλ λ¬Έμ μ΄κΈ°λ νμ§λ§, μ§§κ² μ μ§λλ μ»€λ₯μμμλ μνμ΄ μ μ

---
