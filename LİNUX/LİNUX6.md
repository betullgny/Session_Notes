# TABLE OF CONTENTS
--If Statements
--If Else Statements
--If Elif Statements
--Nested If Statements

# IF STATEMENTS 
If çalışma metodu;
# **if'de belirtilen koşul doğru ise then ve fi arasındaki command çalışır, doğru değil ise then ve fi arasında girmez**
 if [[ some test ]] 
 then 
  <commands>
 fi           

 mkdir test && cd test && touch ifstate.sh && chmod +x ifstate.sh 
 #!/bin/bash
 read -p "ınput a number" NUMBER 
 if [[ $NUMBER -gt 50 ]]
 then 
 echo "the number is big."
 fi

### Relational Operators
      **Operator | Description** 
| -------- | ----------- |
| -eq   | equal                  |  eşit            |
| -ne   | not equal              |  eşit değil      |
| -gt   | greater than           |  büyük           |
| -lt   | less than              |  küçük           |   
| -ge   | greater than or equal  |  büyük veya eşit |
| -le   | less than or equal     |  küçük veya eşit |

### String Operators
| Operator | Description |
| -------- | ----------- |
| =    | equal            | eşit
| !=   | not equal        | eşit değil
| -z   | Empty string     | boş değer               # BOŞLUK DA BİR KARAKTERDİR
| -n   | Not empty string | boş değer değil

```bash
#!/bin/bash

if [[ "a" = "a" ]]
then
  echo "They are same"
fi                                # eşittir ve if true çalıştı

if [[ "a" != "b" ]]
then
  echo "They are not same"
fi                                # eşit değildir ve true çalıştı

if [[ -z "" ]]         
then
  echo "It is empty"
fi                                # eşittir ve true çalıştırır
 
if [[ -n "text" ]]
then
  echo "It is not empty"
fi
```


###  File Test Operators
      **Operator | Description** | 
| -------- | ----------- |
| -d file   | directory  |           dosya directory mu 
| -e file   | exists     |           dosya var mı
| -f file   | ordinary file     |    sıradan bir dosya mı
| -r file   | readable          |    readable dosya mı
| -s file   | size is > 0 bytes |    dosyanın boyutu
| -w file   | writable          |    writeable dosya mı
| -x FILE   | executable        |    executabel dosya mı

 size açısından karşılaştırma
 400- SADECE READ YETKİSİ
 $0 (DOSYANIN KENDİSİ) ÇALIŞTIRILABİLİR Mİ SORGUSU



# IF ELSE STATEMENTS 

If else çalışma metodu;
# **if'de belirtilen koşul doğru ise then ve fi arasındaki command çalışır, doğru değil ise then ve fi arasında girmez ama else çalıştırır**
 if [[ some test ]] 
 then 
  <commands>
  else 
  <other commands>
 fi     


# IF ELIF ELSE STATEMENTS 
If elif else çalışma metodu;
# **if'de belirtilen koşul doğru ise then ve fi arasındaki command çalışır, doğru değil ise then ve fi arasında girmez ama elif çalıştırır. Elif koşulu da doğru değil ise else çalıştırır**

 if [[ some test ]] 
 then 
  <commands>
 elif [[ some test ]]
 then
  <different commands>
  else 
  <other commands>
 fi     


# NESTED IF STATEMENTS 
İf döngüsü içinde yeni bir döngü oluşturma 

 if [[ some test ]] 
 then 
  <commands>
  if [[ some other test ]]
   then
   <commands>
   else 
   <commands>
  fi
 else 
  <other commands>
 fi    

**İKİNCİ İF BLOĞUNDA KÖŞELİ PARANTEZ OLMAZ. MATEMATİKSEL İŞLEM OLDUĞUNDA () KULLANILIR**