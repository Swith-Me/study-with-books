# π 10μ₯. ν΄λμ€

<br />

## ν΄λμ€ μ²΄κ³

1. μ μ ```static``` κ³΅κ°```public``` μμ
2. μ μ ```static``` λΉκ³΅κ°```private``` λ³μ
3. λΉκ³΅κ° μΈμ€ν΄μ€ λ³μ
4. κ³΅κ° ν¨μ
5. λΉκ³΅κ° ν¨μ   
μμ μ νΈμΆνλ κ³΅κ° ν¨μ μ§νμ λ£μ

### μΊ‘μν

λ³μμ μ νΈλ¦¬ν°λ κ°λ₯ν κ³΅κ°νμ§ μλ νΈμ΄ λ«μ§λ§, λ°λμ μ¨κ²¨μΌ νλ€λ λ²μΉλ μλ€   
νμ§λ§ μΊ‘μνλ₯Ό νμ΄μ£Όλ κ²°μ μ μΈμ λ μ΅νμ μλ¨μ΄λ€

<br />

## ν΄λμ€λ μμμΌ νλ€

ν΄λμ€λ λ§‘μ μ±μμ μ²λλ‘ μ¬μ©νλ€   
ν΄λμ€ μ΄λ¦μ Processor, Manager, Super λ±κ³Ό κ°μ΄ λͺ¨νΈν λ¨μ΄κ° μλ€λ©΄ ν΄λμ€μλ€ μ¬λ¬ μ±μμ λ μκ²Όλ€λ μ¦κ±°λ€

ν΄λμ€ μ€λͺμ λ§μΌ```if```, κ·Έλ¦¬κ³ ```and```, ~λ©°```or```, νμ§λ§```but```μ μ¬μ©νμ§ μκ³  25λ¨μ΄ λ΄μΈλ‘ κ°λ₯ν΄μΌ νλ€

### λ¨μΌ μ±μ μμΉ (Single Responsibility Principle, SRP)

ν΄λμ€λ λͺ¨λμ λ³κ²½ν  μ΄μ κ° λ¨ νλλΏμ΄μ΄μΌ νλ€λ μμΉμ΄λ€

λ³κ²½ν  μ΄μ λ₯Ό νμνλ € μ μ°λ€ λ³΄λ©΄ μ½λλ₯Ό μΆμννκΈ°λ μ¬μμ§λ€   
ν° ν΄λμ€ λͺ κ°κ° μλλΌ μμ ν΄λμ€ μ¬λΏμΌλ‘ μ΄λ€μ§ μμ€νμ΄ λ λ°λμ§νλ€

### μμ§λ

ν΄λμ€λ μΈμ€ν΄μ€ λ³μ μκ° μμμΌ νλ€

### μμ§λλ₯Ό μ μ§νλ©΄ μμ ν΄λμ€ μ¬λΏμ΄ λμ¨λ€

> ν° ν¨μ μΌλΆλ₯Ό μμ ν¨μλ‘ λΉΌλ΄κ³  μΆμλ°,   
> λΉΌλ΄λ €λ μ½λκ° ν° ν¨μμ μ μλ λ³μ λ·μ μ¬μ©νλ€

λ€ λ³μλ₯Ό ν΄λμ€ μΈμ€ν΄μ€ λ³μλ‘ μΉκ²©νλ€λ©΄ μ ν¨μλ μΈμκ° **νμμλ€**   
κ·Έλ§νΌ ν¨μλ₯Ό μͺΌκ°κΈ° **μ¬μμ§λ€**

ν΄λμ€κ° μμ§λ ₯μ μλλ€λ©΄ μͺΌκ°λΌ

**λ¦¬ν©ν°λ§ ν κΈΈμ΄κ° λμ΄λ μ΄μ **
1. μ’ λ κΈΈκ³  μμ μ μΈ λ³μ μ΄λ¦μ μ¬μ©νλ€
2. μ½λμ μ£Όμμ μΆκ°νλ μλ¨μΌλ‘ ν¨μ μ μΈκ³Ό ν΄λμ€ μ μΈμ νμ©νλ€
3. κ°λμ±μ λμ΄κ³ μ κ³΅λ°±μ μΆκ°νκ³  νμμ λ§μΆμλ€

<br />

## λ³κ²½νκΈ° μ¬μ΄ ν΄λμ€

κΉ¨λν μμ€νμ ν΄λμ€λ₯Ό μ²΄κ³μ μΌλ‘ μ λ¦¬ν΄ λ³κ²½μ μλ°νλ μνμ λ?μΆλ€

μ κΈ°λ₯μ μμ νκ±°λ κΈ°μ‘΄ κΈ°λ₯μ λ³κ²½ν  λ κ±΄λλ¦΄ μ½λκ° μ΅μμΈ μμ€ν κ΅¬μ‘°κ° λ°λμ§νλ€   
μ΄μμ μΈ μμ€νμ΄λΌλ©΄ μ κΈ°λ₯μ μΆκ°ν  λ μμ€νμ νμ₯ν  λΏ κΈ°μ‘΄ μ½λλ₯Ό λ³κ²½νμ§λ μλλ€

### λ³κ²½μΌλ‘λΆν° κ²©λ¦¬

μΈν°νμ΄μ€μ μΆμ ν΄λμ€λ₯Ό μ¬μ©ν΄ κ΅¬νμ΄ λ―ΈμΉλ μν₯μ κ²©λ¦¬νλ€

**μμ€νμ κ²°ν©λλ₯Ό λ?μΆλ©΄**
- μ μ°μ±κ³Ό μ¬μ¬μ©μ±μ΄ λμμ§λ€
- κ° μμλ₯Ό μ΄ν΄νκΈ° μ¬μμ§λ€
- DIPλ₯Ό λ°λ₯΄λ ν΄λμ€κ° λμ¨λ€

<br />

## λͺ°λλ κ²

**DIP** ν΄λμ€κ° μμΈν κ΅¬νμ΄ μλλΌ μΆμνμ μμ‘΄ν΄μΌ νλ€λ μμΉ

<br />

## κΈ°μ΅μ λ¨μ λΆλΆ

> _κ°κ²°ν μ΄λ¦μ΄ λ μ€λ₯΄μ§ μλλ€λ©΄ νκ²½ ν΄λμ€ ν¬κΈ°κ° λλ¬΄ μ»€μ κ·Έλ λ€_

> _λ¬Έμ λ μ°λ¦¬λ€ λλ€μκ° νλ‘κ·Έλ¨μ΄ λμκ°λ©΄ μΌμ΄ λλ¬λ€κ³  μ¬κΈ°λ λ° μλ€. 'κΉ¨λνκ³  μ²΄κ³μ μΈ μννΈμ¨μ΄'λΌλ **λ€μ** κ΄μ¬μ¬λ‘ μ ννμ§ μλλ€_
