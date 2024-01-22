# **YAML(Yaml aint markup Language)**
-Bir veri tutma formatı veya veri serileştirme 
-objelerin verilerini yaml ve json formatları ile saklanır 
-veri trasnfeeri olarak json kullanıcaz 
-biz daha çok configürasyon dosyaları için kullanıcaz 
-keyler string olmalı, value boolean, integer vs 
-

# **JSON ()**
- Veri transferi için daha çok json kullanılır
-policy (yetkilendirme) yaparken aslında json kullanılır 
-keyler "" şeklinde; listeler [], farklı elemanları , ile ayrıyoruz
-.json uzatıntılı 

# **KARŞILAŞTIRMA (YAML-JSON)**
-YAML CONFİGÜRASYON DOSYALARINDA,json API'lar için yani veri trasnferi içn
-yaml daha avantajlı, referans gösterme durumu var 
-yaml'da komplex veri tipleri kullanılanılır json da kullnılmaz
-birbirilerine döndürülebilddikleri için kullanışlı 
-VERİ KAYBI OLABİLİR
-YAML commet kullandırıyor, json kullandırmaz
-Yaml indentation dikkat edilmeli

# first yaml comment

---      
 # burası ayrı bir yaml, başlangıç ve bitişine koyduk
key: value veya key: "value"
instructur: guile 
---



# **AWS CLOUD FORMATION**
-infrastructure as a cod yani kod ile mimari (IAC)
-AWS Servisi (oluşurma, silme, modify one for few aws resources)
-templates: hazırlanmamış, düzenlenmemiş hali, biz yazıyouz
-stack: cloud formation 'ın templates'in dönüşmüş hali yani 
-aws cloud formation dont pay 
- you pay for use 


# **TEMPLATES**
-yaml veya json formatında yazılmış dosya
-json, yaml, txt, templates uzantılı dosyalar da okunur
-description (iş hayatında kullanılmalı). templates'a ait açıklamaları buraya yazarız
-template version, resouces yazmak zorunlu (output?)

# **STACK**
-template ile ayağa kaldırılmış yapı 
-karmaşık da olur ten bir ec2 da olur 
-Yani ec2 da olabilir sadece, ec2, subnet, load balancer vs gbi komplike servisler de olabilir

#  Arkadaşlar servisleri yazarken herhangi bir sıralama belirtmemize gerek yok. Eğer bir resource da alacağımız değer, diğer bir resource un parametresi olacaksa bunu belirtmemiz gerekiyor sadece. Mesela oluşturacağımız security group ları kullanacağımız Instances da belirtmek yeterlidir. Gerisini Cloudformation ayarlar.. 


AWSTemplateFormatVersion: 2010-09-09
Description: |
  
Parameters:
  
Resources:
  
Outputs:   

# bunu aws den alırken 2010-09-09 silersen hata verir
# logıcal ıd aslında ec2'ya verdiğimiz isim..daha kolay iletişim kurmak için 
# default vpc kullanıyorsak securıtygruops yeterli ama kendi belirlediğimi vpc olursa securıtygroupıd de olmalı 

yeni bir resources yani sec group belirlerken resources gibi başlık açıyoruz



# **LOAD BALANCER**

