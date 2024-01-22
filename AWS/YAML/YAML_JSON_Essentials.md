# YAML JSON ESSENTİALS DERS NOTLARI #
  # YAML #
* YAML: İnsanların ve bilisayaların okuması ve yazması için tasarlanmıs veri serileştirme formatıdır. YAMl okuması ve yazması daha kolaydır.
* YAML VE JSON bir veri tutma veri serileştirme formatıdır. Veri serileştirme: micro servisler arasında iletişim sağlama
* YAML uzantısı .yaml yada .yml 
* Pyhon-Java-C++ bu üç servisin birbirleriyle anlasşması için ortak bir dil kullanmaları gerekli bunu da ymal ve json ile sağlıyoruz.
* Veri transferi için ise daha hızlı oldugu için json tercih ediyoruz.
* Yaml key ve value degerlerini tutar key degerlerimiz her zaman string olmalı 
  # JSON #
* JSON: Micro servisler arasında veri transferi için kullanılır.
* JSON uzantısı .json
  # YAML VE JSON ARASINDA Kİ FARKLAR#
* Yaml daha cok konfrigasyon dosyalarında kullanılır
* Json veri transferi için kullanılır.
* Yaml ve Json birbilerine cok kolay cevrilebilir

## EXAMPLE-YAML ## 

# First yaml comment           --> istediğimiz yere yorum satırı ekleyebiliriz

---                            --> 3 -başladık ve  3 - ile bitirdik ayrı bir yaml dosyası demek      
key: value 
instructor: guile
---

# List of strings             --> Liste gösterim sekli     
- guile
- osvaldo
- james

---

[guile, osvaldo, james]     --> Liste gösterim sekli 

---
instructors:                --> Liste gösterim sekli 
 - guile
 - osvaldo                                                            --> genel de bu sekilde kullanılır 
 - james

---
# An employee record

instructor: 
  name: guile
  job: solutions-arct                                                 --> örnek olarak bir ögretmenin özelliklerini tanımlıyoruz
  department: aws
  age: 37

---
# Employee list

instructors: 
 - name: guile
   job: solutions-arct
   department: aws
   age: 37

 - name: callahan
   job: solutions-arct
   department: aws
   age: 40

---

-  instructor1: 
      name: guile
      job: solutions-arct
      department: aws
      age: 37
-  instructor2: 
      name: james
      job: devops-eng
      department: 
        - devops
        - aws
      age: 36
---
# Another object format

instructor:
  {name: callahan, job: devops-eng, department: [aws, devops], age: 40}  --> json formatına örnek

...

#Multiple lines values

example1: |  # Keeps the block                      --> | işareti koyduktan sonra altında bulunanları satır satır yazarız genelde user data icin kullanırız
  Guile M.
  Solutions Architect
  Clarusway LLC.
example2: > # This text will be a sentence         --> > uzun olan komutları yazmak için kullanılır <This text will be a sentence >
  This
  text
  will
  be
  a
  sentence


## AWS CLOUDFORMATİON ##
* Cloudformation: Kod ile mimari, dosya üzerinde yazılmış olan bilgilere göre aws servislerini calıstırıyoruz
* Bir template dosyası hazırlıyoruz ve hep o dosya üzerinden aws servislerini calıstırıyoruz
* Dogru hazırlanmıs template dosyalarını cloudformation yükleriz ve bir mimari ayaga kalkar
* Template ile kaldırılmış olan bir mimari cok kolay bir şekilde silinebilir



- Template <json/Yaml --> Cloudformation <dogru yazılmı bir template dosyasını kaldııryor>  --> Stack

* Cloudformation herhnagi bir üceti yok ayaga kaldırdığı servisler ücretli olabilir
* Template : json,yaml,yml,olamalı ayrıca template veya txt olabilir ama biz yaml tercih ediyoruz
* Templatin bir yapısı var template version ve resources olmak zorunda

- Stack
* Template ile ayaga kaldırılmış bir yapı bu yapı bir ec2 da olabilir bierden fazla da olabilir
* Aws template içerisine yamış oldugumuz tüm servileri bir bütün olarak görür ve içlerinden birini silmek istersek hepsini silme zorunda kalırız

* Template dosyaları her zaman s3 de tutulur
* Daha önce olusturmus oldugumuz bir template dosyası s3 de olabilir ordan da calıstırabiliriz
* 
