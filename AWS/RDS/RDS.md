
#  **WHAT IS DATABASE** 
"A system in which we can access the structured data is called a database."
Database; yapılandırılmış verilere erişebildiğimiz sistem 
S3, ec2 ve RDS farklı türde depolama yöntemleri aslında 

# S3                                             # EC2/EBS (volume)                                      # AMAZON RDS
*Object base*                                    *blok base*                                        *Query base* 
ınventory list                                   web server                                          customer info 
ınventory pictures                               operating system ile ilgili dataları                email
ınventory info                                   web server software                                 phone
system logs                                      system and app logs                                 login date/shopping data
fotoğraz tarzı;                                  nginx, appache                                      applikasyonu kullanırken girilen bilgi
                                                                                                     **asıl olayı hızlı şekilde cevap vermesi**

# OLTP                                               # OLAP
OLTP sistemlerinin temel amacı                       Bu sistemlerdeki asıl amaç ise veriyi işlemek değil, analiz etmektir.
veriyi işlemektir, analiz etmek değildir.            OLTP sistemlerine göre analiz ve raporlamaları çok daha performanslı  
                                                     yapmasından dolayı tercih edilmektedir.  
Daha hızlı sistem                                    Daha yavaş, saatler süren bir sistem 
Burada yedekleme önemli                              yedekleme yok, kaynaktan tekrar veri çekilir
Aplikasyon, site üzerinden kimin ne                  OLTP'de kaydedilen veri üzerinden analitik sonuç çıkarma
aldığını kaydetme 

# **TYPES OF DATABASE**
Amazon bize 2 database seçeneği sunar;
Relational Database (SQL) ------excel
Non-Relational Database (NoSQL) ------word 

# Relational                                                                     # Non-Relational
Düzenli veri tutulması                                                            Düzensiz yapı
veriler satır/sütun şeklinde depolanır                                            Spesifik seçim yapamazsın 
yaygın kullanılan                                                                 Tabloların aksine farklı yollar ile veriler modellenir
önceden belirlenmiş şemalar ve  kullanıcı bunlara uyrak veri depolar              Relational'a göre daha hızlı
Non_Relational'dan farkı **JOİN** olması                                          Relational'a ait sorunları çözmek için tasarlanmış ve 
Join fonksiyonu ile farklı tablolardan yeni                                       Dinamik şemalar ile çalışır
bir tablo üretebiliriz                                                            JOİN fonksiyonu yok
veritabanı geliştiricleri ile koordinasyon olmalı                                 veritabanı geliştiricisi ile koordinasyona gerek yok
Büyk ve Dinamik çalışan veritabanları için uygun değil                            Basit veritabanları için 
daha kompleks veritabanları için 

# AMAZON RDS                                                             # EC2                                               # AMAZON DYNAMODB 
3 Bölümden oluşur;                                                      
DATABASE ENGİNNES                                                       RDS'İN AMI
--- postgreSQL, MariaDB vs versiyon seçiyoruz aslında  
DB INSTANCE                                                             INSTANCE TYPE 
--- db.t2.micro                                                       
STORAGE DİSK                                                            VOLUME 
--- volume 

amazon yönetir (run, backup, versioning..)                             Manuel 
kontrol aws'de                                                         kontrol kullanıcıda
hata payı düşük                                                        hata payı yüksek          

# RDS (DATABASE INSTANCE)
ec2'ta olduğu gibi 2'ye ayırıyoruz; pricing and purpose 
# PRİCİNG                                                                                              # PURPOSE 
--On Demand Instance                                                                                -- Memoory Optimized 
En pahalı                                                                                              Çok fazla ram'a ihtiyaç varsa
   
--Reserved Instance                                                                                 -- General Purpose 
kullanım süresi belirterek alınan                                                                     Fiyat/performans ürünü ajajaj  
                                                                                                     
#                                                                                                  -- Burstable
# INSTANCE STORAGE
3 bölümden oluşur
*General Purpose Storage*                                 *Provisioned IOPS Storage*                         *Magnetic Storage*
cost effective                                            depolanan bilginin verilme hızının                 not recommend 
(fiyat/perfomans)                                         önemli olduğu durumlarda kullanılır

# MYSQL WORKBENCH 
Database'leri gösteren grafiksel ara yüzü olan bir aplikasyon 
RDS'w ssh üzerinden direkt bağlanamayız. Önce ssh ile bir ec2'ya bağlan, ec2 içine mysql client yükle ve 2206 portu ile amazon rds'e bağlan 
Bunun yerine workbench ve 3306 portu ile direkt amazon rds'e bağlanabiliriz.

# DB INSTANCE BACKUP 
AMAZON biir manulel biri otomatik olmak üzere 2 backup sistemi sunar
*AUTOMATED*                                 *MANUALLY*
belirtilen zamanlarda backup alır           Manuel olarak o anda alınan backup
S3'de tutulur                               S3 tutulur 
RDS kapandığı an silinir                    RDS kapandığında durur

**manuel daha avantajlı gibi görünse de;**
automated backup'da belirtilen saatlerde backup alır, sorun olan saate transaction logs'lar ile gidilebilir 
transaction log'lar her 5 dakika da tutulur 
her zaman yeni database açılır eskisi üzerinden çalışmaz
backup'lar 35 gün tutulur 

# DB INSTANCE SNAPSHOT 
Anlık snapsot oluşturma 

# RDS MULTİ-AZ DEPLOYMENT 
Farklı availability zone'da database yaratır 
sadece ihtiyaç dahilinde kullanılır yani primary hasar gçrdüğünde vs 
hem read hem write özelliği var 
2 database aynı anda çalışmıyor, diğer database ana oyuncu olur 
senkron

# RDS REPLİKAS
Farklı availability zone'da database yaratır 
Database'i okur, write yetkisi yok
senkron değil
Hızlandırma faydası var (SORU OLABİLİR!!)

# AURORA MULTİ-MASTER CLUSTERS
Hem multi-az hem replika
Daha maliyetli

# HANDSON 
--KONSOL'dan RDS servisi
--önce RDS için security group oluşturulur
  inbound'da mysql/yani 3306 portu açılır
  source myıp seçilir (database gizlidir)
create sec grp

