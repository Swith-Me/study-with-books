# πΏ [14μ₯] λ³΄μ HTTP

---

# 14.1 HTTPλ₯Ό μμ νκ² λ§λ€κΈ°

* μΉμ μμ ν λ°©μμ HTTPλ₯Ό νμλ‘ ν¨
* HTTPμ λ³΄μ λ²μ μ ν¨μ¨μ , μ΄μμ±, κ΄λ¦¬ μ©μ΄, μ μλ ₯μ΄ μκ΅¬λ¨

>
* **μλ² μΈμ¦**
μμ‘°λ μλ²κ° μλμ μ μ μμ΄μΌ ν¨
* **ν΄λΌμ΄μΈνΈ μΈμ¦**
μ§μ§ μ¬μ©μμμ μΈμ¦ν  μ μμ΄μΌ ν¨
* **λ¬΄κ²°μ±**
μμ‘°λ λ°μ΄ν°λ‘λΆν° μμ ν΄μΌ ν¨
* **μνΈν**
μλ²μ ν΄λΌμ΄μΈνΈλ λμ²­μ λν κ±±μ  μμ΄ λν κ°λ₯ν΄μΌ ν¨
* **ν¨μ¨**
μ λ ΄ν ν΄λΌμ΄μΈνΈλ μλ²λ μ΄μ©ν  μ μλλ‘ λΉ λ₯Έ μκ³ λ¦¬μ¦μ κ°μ ΈμΌ ν¨
* **νΈμ¬μ±**
νλ‘ν μ½μ λͺ¨λ  ν΄λΌμ΄μΈνΈ μλ²μμ μ§μλμ΄μΌ ν¨
* **κ΄λ¦¬μ νμ₯μ±**
λκ΅¬λ , μ΄λμλ  μ¦κ°μ μΈ λ³΄μ ν΅μ  κ°λ₯ν΄μΌ ν¨
* **μ μμ±**
νμ¬ μλ €μ§ μ΅μ μ λ³΄μ λ°©λ²μ μ§μν΄μΌ ν¨
* **μ¬νμ  μμ‘΄μ±**
μ¬νμ λ¬Ένμ , μ μΉμ  μκ΅¬λ₯Ό λ§μ‘±ν΄μΌ ν¨

### 14.1.1 HTTPS
* HTTPλ₯Ό μμ νκ² λ§λλ λ°©μ μ€ κ°μ₯ μΈκΈ°μλ κ²
* ```https://```
* λͺ¨λ  HTTP μμ²­κ³Ό μλ΅ λ°μ΄ν°λ λ€νΈμν¬λ‘ λ³΄λ΄μ§κΈ° μ μ μνΈνλ¨
* SSL (μμ  μμΌ κ³μΈ΅, Secure Sockets Layer)μ μ΄μ©νμ¬ κ΅¬νλ¨

<br/>

# 14.2 λμ§νΈ μνΈν

>
**λμ§νΈ μνΈνμμ λ€λ£¨λ λ΄μ©**
* **μνΈ**
νμ€νΈλ₯Ό μλ¬΄λ μ½μ§ λͺ»νλλ‘ μΈμ½λ©νλ μκ³ λ¦¬μ¦
* **ν€**
μνΈμ λμμ λ³κ²½νλ μ«μλ‘ λ λ§€κ°λ³μ
* **λμΉ­ν€ μνΈ μ²΄κ³**
μΈμ½λ©κ³Ό λμ½λ©μ κ°μ ν€λ₯Ό μ κ³΅νλ μκ³ λ¦¬μ¦
* **λΉλμΉ­ν€ μνΈ μ²΄κ³**
μΈμ½λ©κ³Ό λμ½λ©μ λ€λ₯Έ ν€λ₯Ό μ¬μ©νλ μκ³ λ¦¬μ¦
* **κ³΅κ°ν€ μνΈλ²**
λΉλ° λ©μμ§λ₯Ό μ λ¬νλ μλ°±λ§ λμ μ»΄ν¨ν°λ₯Ό μ½κ² λ§λ€ μ μλ μμ€ν
* **λμ§νΈ μλͺ**
λ©μμ§κ° μμ‘° νΉμ λ³μ‘°λμ§ μμμμ μμ¦νλ μ²΄ν¬μ¬
* **λμ§νΈ μΈμ¦μ**
μ λ’°ν  λ§ν μ‘°μ§μ μν΄ μλͺλκ³  κ²μ¦λ μ μ νμΈ μ λ³΄

### 14.2.1 λΉλ° μ½λμ κΈ°μ κ³Ό κ³Όν
* μνΈλ²μ λ©μμ§ μΈμ½λ©κ³Ό λμ½λ©μ λν κ³Όνμ΄μ κΈ°μ 
* λ©μμ§μ λμ²­, λ³μ‘°λ₯Ό λ°©μ§νκΈ° μν΄ μ¬μ©
* λκ΅°κ°κ° λ©μμ§, νΈλμ­μμ μ μμμ μ¦λͺνλ λ°λ μ¬μ©

### 14.2.2 μνΈ(cipher)
* μνΈλ²μ μνΈλΌ λΆλ¦¬λ λΉλ° μ½λμ κΈ°λ°
* μνΈλ λ©μμ§λ₯Ό μΈμ½λ©νλ μ΄λ€ νΉμ ν λ°©λ²κ³Ό λμ€μ κ·Έ λΉλ° λ©μμ§λ₯Ό λμ½λ©νλ λ°©λ²
* νλ¬Έ β μνΈ μ μ© β μνΈλ¬Έ

### 14.2.3 μνΈ κΈ°κ³
* κΈ°μ μ΄ μ§λ³΄νλ©° λ³΅μ‘ν μνΈλ‘ λ©μμ§λ₯Ό μ½λ©νκ³  λμ½λ©νλ κΈ°κ³, μνΈ κΈ°κ³λ₯Ό λ§λ€κΈ° μμ
* λ¨μν νμ μ νλ λμ  κΈμλ€μ λμ²΄νκ³ , κ·Έ μμλ₯Ό λ°κΎΈμμΌλ©°, κ°λ₯΄κ³  ν λ§λ

### 14.2.4 ν€κ° μλ μνΈ
* λκ΅°κ° κΈ°κ³λ₯Ό νμΉλλΌλ μ¬λ°λ₯Έ λ€μ΄μΌ μ€μ (ν€ κ°) μμ΄λ λμ½λ λμ λΆκ°
* μνΈ λ§€κ°λ³μλ₯Ό ν€λΌκ³  λΆλ¦
* μ΄ κ°μ μνΈ κΈ°κ³λ€μ μλ‘ λ€λ₯Έ ν€ κ°μ κ°μ§κ³  μκΈ° λλ¬Έμ μ κ°κ° λμ

### 14.2.5 λμ§νΈ μνΈ
* λ κ°μ§μ μ£Όμν λ°μ 
  * μλ λ° κΈ°λ₯μ λν κΈ°κ³ μ₯μΉμ νκ³μμ λ²μ΄λ, λ³΅μ‘ν μΈμ½λ©/λμ½λ© μκ³ λ¦¬μ¦ κ°λ₯
  * λ§€μ° ν° ν€λ₯Ό μ§μνλ κ²μ΄ κ°λ₯ν΄μ Έ, λ¬΄μμ μΆμΈ‘ ν€μ μν ν¬λνΉ μ΄λ €μμ§

<br/>

# 14.3 λμΉ­ν€ μνΈλ²

* λμΉ­ν€ μνΈ μκ³ λ¦¬μ¦μ μΈμ½λ©κ³Ό λμ½λ©μ κ°μ ν€λ₯Ό μ¬μ©

### 14.3.1 ν€ κΈΈμ΄μ μ΄κ±° κ³΅κ²©(Enumeration Attack)
* λλΆλΆμ κ²½μ°, μΈμ½λ© λ° λμ½λ© μκ³ λ¦¬μ¦μ κ³΅κ°μ μΌλ‘ μλ €μ Έ μμΌλ―λ‘ ν€λ§μ΄ μ μΌν λΉλ°
* λ¬΄μ°¨λ³λ‘ λͺ¨λ  ν€ κ°μ λμν΄ λ³΄λ κ³΅κ²©μ μ΄κ±° κ³΅κ²©μ΄λΌκ³  ν¨
* κ°λ₯ν ν€ κ°μ΄ λ§μ μλ‘ μ λ¦¬

### 14.3.2 κ³΅μ ν€ λ°κΈνκΈ°
* λμΉ­ν€ μνΈμ λ¨μ  μ€ νλλ λ°μ‘μ, μμ μκ° μλ‘ λννλ €λ©΄ λ λ€ κ³΅μ ν€λ₯Ό κ°μ ΈμΌνλ€λ κ²
* κΈ°μ΅ν΄μΌ ν  λΉλ° ν€κ° λμ΄λκ² λλ€λ λ¨μ 

<br/>

# 14.4 κ³΅κ°ν€ μνΈλ²

* λ κ°μ λΉλμΉ­ ν€λ₯Ό μ¬μ©
* μΈμ½λ©μ μν νλ, λμ½λ©μ μν νλ
* ν€μ λΆλ¦¬λ λ©μμ§μ μΈμ½λ©μ λκ΅¬λ ν  μ μλλ‘ ν΄μ£Όλ λμμ, λ©μμ§λ₯Ό λμ½λ©νλ λ₯λ ₯μ μμ μμκ²λ§ λΆμ¬

### 14.4.1 RSA
>
**κ³΅κ°ν€ λΉλμΉ­ μνΈμ κ³Όμ **
* κ³΅κ°ν€(λκ΅¬λ μ»μ μ μμ)
* κ°λ‘μ±μ μ»μ μνΈλ¬Έμ μΌλΆ(λ€νΈμν¬λ₯Ό μ€λνν΄μ νλ)
* λ©μμ§μ κ·Έκ²μ μνΈνν μνΈλ¬Έ(μΈμ½λμ μμμ νμ€νΈλ₯Ό λ£κ³  μ€νν΄μ νλ)

* μ΄ λͺ¨λ  μκ΅¬λ₯Ό λ§μ‘±νλ κ³΅κ°ν€ μνΈ μ²΄κ³ μ€ μ λͺν νλλ MTμμ λ°λͺλκ³  μμ
* RSAμ μμ€ μ½λκΉμ§ μ£Όμ΄μ‘λ€κ³  νλλΌλ μνΈλ₯Ό ν¬λνΉνμ¬ ν΄λΉνλ κ°μΈ ν€λ₯Ό μ°Ύμλ΄λ κ²μ λ§€μ° μ΄λ €μ

### 14.4.2 νΌμ± μνΈ μ²΄κ³μ μΈμ ν€
* κ³΅κ°ν€μ μνΈ λ°©μμ μκ³ λ¦¬μ¦μ λ¨μ μ κ³μ°μ΄ λλ¦° κ²½ν₯μ΄ μλ€λ κ²
* λλ¬Έμ λμΉ­ν€μ λΉλμΉ­ λ°©μμ μμ΄ μ¬μ©

<br/>

# 14.5 λμ§νΈ μλͺ

* λκ° λ©μμ§λ₯Ό μΌλμ§ μλ €μ£Όκ³  κ·Έ λ©μμ§κ° μμ‘°λμ§ μμμμ μΈμ¦

### 14.5.1 μλͺμ μνΈ μ²΄ν¬μ¬μ΄λ€
* μλͺμ λ©μμ§λ₯Ό μμ±ν μ μκ° λκ΅°μ§ μλ €μ€
* μλͺμ μμ‘°λ₯Ό λ°©μ§ν¨
* λμ§νΈ μλͺμ λ³΄ν΅ λΉλμΉ­ κ³΅κ°ν€μ μν΄ μμ±

<br/>

# 14.6 λμ§νΈ μΈμ¦μ

* νν certsλΌκ³  λΆλ¦¬λ λμ§νΈ μΈμ¦μ
* μ λ’°ν  μ μλ κΈ°κ΄μΌλ‘ λΆν° λ³΄μ¦λ°μ μ¬μ©μλ νμ¬μ λν μ λ³΄λ₯Ό λ΄κ³  μμ

### 14.6.1 μΈμ¦μμ λ΄λΆ
* λμμ μ΄λ¦, μ ν¨κΈ°κ°, μΈμ¦μ λ°κΈμ, μΈμ¦μ λ°κΈμμ λμ§νΈ μλͺ

### 14.6.2 X.509 v3 μΈμ¦μ
* λΆνν, λμ§νΈ μΈμ¦μμ λν μ  μΈκ³μ μΈ λ¨μΌ νμ€μ μμ
* λ―Έλ¬νκ² λ€λ₯Έ μ¬λ¬κ°μ§ μ€νμΌμ λμ§νΈ μΈμ¦μλ€μ΄ μ‘΄μ¬

### 14.6.3 μλ² μΈμ¦μ μν μΈμ¦μ μ¬μ©νκΈ°
* μ¬μ©μκ° HTTPSλ₯Ό ν΅ν μμ ν μΉ νΈλμ­μμ μμν  λ, μ΅μ  λΈλΌμ°μ λ μλμΌλ‘ μ μν μλ²μμ λμ§νΈ μΈμ¦μ κ°μ Έμ΄
* μΉ μ¬μ΄νΈμ μ΄λ¦κ³Ό νΈμ€νΈλͺ, μΉ μ¬μ΄νΈμ κ³΅κ°ν€, μλͺ κΈ°κ΄μ μ΄λ¦, μλͺ κΈ°κ΄μ μλͺ

<br/>

# 14.7 HTTPSμ μΈλΆμ¬ν­

* HTTPSλ HTTPμ κ°μ₯ μ λͺν λ³΄μ λ²μ 
* HTTPSλ HTTP νλ‘ν μ½μ λμΉ­, λΉλμΉ­ μΈμ¦μ κΈ°λ° μνΈ κΈ°λ²μ κ°λ ₯ν μ§ν©μ κ²°ν©ν κ²
* HTTPSλ μΈν°λ· μ νλ¦¬μΌμ΄μμ μ±μ₯ + μΉ κΈ°λ° μ μμκ±°λμ μ±μ₯μ κ³ μν

### 14.7.1 HTTPS κ°μ
* HTTPSλ κ·Έλ₯ λ³΄μ μ μ‘ κ³μΈ΅μ ν΅ν΄ μ μ‘λλ HTTP
* μνΈνλμ§ μμ HTTP λ©μμ§λ₯Ό TCPλ₯Ό λ³΄λ΄κΈ° μ  λ³΄μ κ³μΈ΅μΌλ‘ λ³΄λ

### 14.7.2 HTTPS μ€ν΄
* μ€λλ  λ³΄μ HTTPλ μ ν, μΉ μλ²μκ² λͺμ μμμ΄ νμ β URLμ μ€ν΄
* HTTPS νλ‘ν μ½μμ URLμ μ€ν΄ μ λμ¬λ ```https```

### 14.7.3 λ³΄μ μ μ‘ μμ
* μνΈνλμ§ μμ HTTPμμ, ν΄λΌμ΄μΈνΈλ μΉ μλ²μ 80λ² ν¬νΈλ‘ TCP μ»€λ₯μμ μ΄κ³ , μμ²­ λ©μμ§λ₯Ό λ³΄λ΄κ³ , μλ΅ λ©μμ§λ₯Ό λ°κ³ , μ»€λ₯μμ λ«μ
* HTTPSμμμ μ μ°¨λ SSL λ³΄μ κ³μΈ΅ λλ¬Έμ μ½κ° λ λ³΅μ‘
* 443 ν¬νΈλ‘ μ°κ²°, μνΈλ² λ§€κ°λ³μμ κ΅ν ν€ νμμΌλ‘ SSL κ³μΈ΅ μ΄κΈ°ν, νΈλμ°μ΄ν¬ μλ£λλ©΄ SSL μ΄κΈ°ν μλ£, ν΄λΌμ΄μΈνΈκ° μμ²­ λ©μμ§λ₯Ό λ³΄μ κ³μΈ΅μ λ³΄λ

### 14.7.4 SSL νΈλμ°μ΄ν¬
>
**νΈλμ°μ΄ν¬μμ μΌμ΄λλ μΌλ€**
* νλ‘ν μ½ λ²μ  λ²νΈ κ΅ν
* μμͺ½μ΄ μκ³  μλ μνΈ μ ν
* μμͺ½μ μ μ μΈμ¦
* μ±λμ μνΈννκΈ° μν μμ μΈμ ν€ μμ±

* μνΈνλ HTTP λ°μ΄ν°κ° λ€νΈμν¬λ₯Ό μ€κ°κΈ°λ μ μ, SSLμ ν΅μ μ μμνκΈ° μν΄ μλΉν μμ νΈλμ°μ΄ν¬ λ°μ΄ν°λ₯Ό μ£Όκ³  λ°μ

### 14.7.5 μλ² μΈμ¦μ
* SSLμ μλ² μΈμ¦μλ₯Ό ν΄λΌμ΄μΈνΈλ‘ λλ₯΄κ³  λ€μ ν΄λΌμ΄μΈνΈ μΈμ¦μλ₯Ό μλ²λ‘ λ λΌμ£Όλ μνΈ μΈμ¦ μ§μ
* λ³΄μ HTTPS νΈλμ­μμ ν­μ μλ² μΈμ¦μλ₯Ό μκ΅¬
* μλ² μΈμ¦μλ μ‘°μ§μ μ΄λ¦, μ£Όμ, μλ² DNS λλ©μΈ μ΄λ¦, κ·Έ μΈμ μ λ³΄λ₯Ό λ³΄μ¬μ£Όλ X.509 v3μμ νμλ μΈμ¦μ

### 14.7.6 μ¬μ΄νΈ μΈμ¦μ κ²μ¬
* μ΅μ  μΉ λΈλΌμ°μ λ€ λλΆλΆμ μΈμ¦μμ λν΄ κ°λ¨νκ² κΈ°λ³Έμ μΈ κ²μ¬λ₯Ό νκ³  κ·Έ κ²°κ³Όλ₯Ό λ μ² μ ν κ²μ¬λ₯Ό ν  μ μλ λ°©λ²κ³Ό ν¨κ» μ¬μ©μλ€μκ² μλ €μ€
* λ μ§ κ²μ¬ β μλͺμ μ λ’°λ κ²μ¬ β μλͺ κ²μ¬ β μ¬μ΄νΈ μ μ κ²μ¬

### 14.7.7 κ°μ νΈμ€νκ³Ό μΈμ¦μ
* κ°μ νΈμ€νΈ(νλμ μλ²μ μ¬λ¬ νΈμ€νΈλͺ)μΌλ‘ μ΄μλλ μ¬μ΄νΈ λ³΄μ νΈλν½μ λ€λ£¨λ κ²μ κΉλ€λ‘μ΄ κ²½μ°λ λ§μ

<br/>

# 14.8 μ§μ§ HTTPS ν΄λΌμ΄μΈνΈ
* SSLμ λ³΅μ‘ν λ°μ΄λλ¦¬ νλ‘ν μ½

### 14.8.1 OpenSSL
* OpenSSLμ SSLκ³Ό TLSμ κ°μ₯ μΈκΈ° μλ μ€ν μμ€ κ΅¬ν
* OpenSSL νλ‘μ νΈλ, κ°λ ¬ν λ€λͺ©μ  μνΈλ² λΌμ΄λΈλ¬λ¦¬μΈ λμμ SSLκ³Ό TLS νλ‘ν μ½μ κ΅¬νν κ°κ±΄νκ³  μμ ν κΈ°λ₯μ κ°μΆ μμ© μμ€μ ν΄ν·μ κ°λ°νκ³ μ ν κ²°κ³Όλ¬Ό

### 14.8.2 κ°λ¨ν HTTPS ν΄λΌμ΄μΈνΈ
* (μλ΅)

### 14.8.3 μ°λ¦¬μ λ¨μν OpenSSL ν΄λΌμ΄μΈνΈ μ€ννκΈ°
* (μλ΅)

<br/>

# 14.9 νλ½μλ₯Ό ν΅ν λ³΄μ νΈλν½ ν°λλ§

* ν΄λΌμ΄μΈνΈλ μ’μ’ κ·Έλ€μ λμ νμ¬ μΉ μλ²μ μ κ·Όν΄μ£Όλ μΉ νλ½μ μλ²λ₯Ό μ΄μ©
* HTTPSκ° νλ½μμλ μ λμν  μ μλλ‘ νκΈ° μν΄, ν΄λΌμ΄μΈνΈκ° νλ½μμκ² μ΄λμ μ μνλ €κ³  νλμ§ λ§ν΄μ£Όλ λ°©λ²μ μ½κ° μμ ν΄μΌ ν¨ β HTTPS SSL ν°λλ§ νλ‘ν μ½
