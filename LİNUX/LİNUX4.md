# Table Of Contents
--Stadin, Stdout, Stderr
--Filters
--Commands
    cat, tee, grep, cut, tr, wc, sort, uniq, comm
--Control Operators 


# Stadin (Standart İnput)
 stdin---> program ----> stdout
                   ----> stderr

#                                                                    **FILTERS**
PİPE; ile bir komutun çıktısı diğer komutun girdisi olur
 cats colors.txt | grep "blue"

# CAT
Görüntüleme
 cat days.txt 
 cat days.txt | cat | defalarca cat yapsak da text'i bir kere gösterir

# TEE
Aslında cat ile aynı görevde ekstra kaydetme işlemi de yapar
 ls -l | tee file.txt | less                                   #more/less komutu da aynı işlevde
less verinin tamamından ziyade line line gösteririr. line'ı sen belirlersin

# GREP 
Metin satırını filtrelemek 
 cat tennis.txt | grep Williams

# CUT 
Dosyalardan kolon seçebilmek 
 ls -l | cut -d' ' -f3                      # d; delimiter, f3 kolon numarası 
dosya isimlerine göre txt, html vs seçim yapılabilir 
 ls -l | cut -d' ' -f3 | grep txt

# TR 
Translate (değiştirme)
 cat clarusway.txt | tr 'aer' 'iou'
Küçük harften büyük harfe ve tam tersi gibi veya yeni satırları boşluklara çevirmek için kullanılır
 cat clarusway.txt | tr '\n' ' '   # görünmeyen seperate yerine boşluk koyar ve çıktıyı yan yana verir

# WC 
Satırları veya karakterleri saymak 
Log dosyalarında vs kullanılabilir
 wc count.txt                   # klime, satır ve karakter sayılarını birlikte çıktı verir
 wc -l count.txt                # sadece satırları
 wc -w count.txt                # sadece kelimeleri 
 wc -c count.txt                # sadece karakterleri

# **FİLTRELEME KOMUTLARININ ÇOĞUNDA ANA DOSYA DEĞİŞMEZ, GÖRÜNTÜLENEN DEĞİŞİR**
Mesela; 
 cat clarusway.txt | tr 'aer' 'iou' > tee result.txt   komutu ile çıktıyı result.txt dosyasına yazdırabiliriz. Bu durumda da clarusway.txt dosyası değişmez, değişiklikler yapılır ve result.txt dosyasına kaydedilebilir.

# SORT 
Alfabetik sıralama 
 sort (A-Z sıralama)
 sort -r (reverse; Z-A sıralama )
 sort -f (non case sensitive)
**ASCII'ye göre sıralama**

# UNIQ
Burda önce sort komutu ile sıralama yapılır, sonra unıq komutu kullanılır
 cat marks.txt | sort | uniq 

# COMM
Kümeler ama iki dosyayı karşılaştırma olarak da düşünülebilir
Defaul olarak 3 colomns(kolon) görüntüler
 birinci kolon birinci dosyanın eşleşmeyen ögelerini                     # Sadece A kümesinde olanlar 
 ikinci kolon ikinci dosyanın eşleşmeyen ögelerini                       # Sadece B kümesinde olanlar 
 üçüncü kolon ise her iki dosyanın ortak ögelerini gösterir              # A ve B kümesi kesişim
Comm komutunun çalışabilmesi için iki dosya da sıralanmış (sort) olmalı 


# TAC 
ters çevirme 
 tac count.txt        # sıralı bir dosyayı tersten yazma, sondan başlayarak gibi
 tac count.txt | grep temp.txt | tac 



#                                                             **CONTROL OPERATORS** 

# SEMICOLON(;)
Bir veya birden fazla komut ; ile arka arkaya çalıştırılabilir
 cat days.txt ; cat count.txt 
Hangisi doğruysa çalışır, herhangi bir koşul yok


# AMPERSAND (&)
Bir satır & işaretiyle sona erdiğinde, kabuk komutun bitmesini beklemez. Kabuk isteminizi geri alacaksınız ve komut arka planda yürütülecektir. Bu komutun arka planda yürütülmesi tamamlandığında bir mesaj alacaksınız.
 sleep 20 &
sleep 20 yazdığında sistem arka planda çalışıyor, komutun bitmesini beklemek gerekir.
sleep 2 & yazdığında bir tane process id verir ve işleme devam etmeni sağlar 


# DOLLAR QUESTION ($?)
son yazılan komutun çalışıp çalışmadığını kontrol etme 
output 0 ise komut doğru ve çalışır, output 0'dan farklı ise komut yanlış veya çalışmaz
Bash script'te conditions'larda kullanılabilir **
 echo $?

# DOUBLE AMPERSAND (&&) 
Logical AND
Birinci komut çalışırsa ikinci komut da çalışır 
 cat days.txt && cat count.txt 
Ön şart; ikinci komutun çalışabilmesi için birinci komut mutlaka çalışmalı 
**(PYTHON & OPERATÖRÜ GİBİ ÇALIŞIR)**
 sudo dnf update -y && sudo dnf upgrade -y


# DPUBLE VERTICAL BAR (||)
Logical OR
Birinci komut çalışırsa ikinciyi çalıştırmaz tekrar
 sudo dnf update -y || sudo dnf upgrade -y 
Hangi komut çalışırsa koşulsuz onu çalıştırır
**(PYTHON | OPERATÖRÜ GİBİ ÇALIŞIR)**
 cat days.txt || echo 'clarusway' ; echo one    # Birinci komut çalışır OR kpısı ikinci komuta gitmez fakat ; üçüncü komutu çalıştırır
 zecho days.txt || echo "clarusway" ; echo one 

# COMBINING && AND ||
Programlama dilindeki if/then/else structure'ı 
AND ve OR operatörü birlikte kullanılır 
 ls && ll || echo "succesfull"
 ls && lsll || echo "succesful"

# POUND SIGN (#)

#                         **SED, AWK AND CRONTAB COOMAND**

# SED COMMAND
Bul ve değiştir
Bir file içinde değiştirmek istediğin kelime ve yerine yazacağın kelime vs
 sed 's/linux/ubuntu/' sed.txt
 s; bul ve değiştir 
 linux; değiştirilen
 ubuntu; yeni yazılan
 
**Case Sensitive** 
 sed 's/linux/ubuntu/i' sed.txt
 i; case sensitive ortadan kaldırma 

her satırın 2.değiştirilenini değiştirme 
 sed 's/linux/ubuntu/2i' sed.txt

Globalde yani tüm emtin içerisinde değiştirme
 sed 's/linux/ubuntu/g' sed.txt

Global ve case sensitive çin 
 sed 's/linux/ubuntu/ig' sed.txt
 sed 's/linux/ubuntu/3i' sed.txt  # her satırda bulduğu 3.linux değişir

İstediğin satırda değişim yapma 
 sed '2 s/linux/ubuntu/ig' sed.txt # 2 satır sayısını ifade eder

# AWK COMMAND
Bir dosya içinde istenilen değeri alma
awk.txt dosyasının tamamını gösterir
 awk '{print}' awk.txt

/This ile başlayanları grüntüleme
 awk '/This/ {print}' awk.txt

İstediğin sütunu görüntüleme 
 awk '{print $2}' awk.txt
 awk '{print $2,$4}' awk.txt

-F delimiter olarak kullanılabilir
 awk -F: '{print $2}' awk.txt

9.sütunu görüntüleme
 ls -l | awk '{print $9}'

awk içinde if komutu çalıştırma
 awk '{ if($7 == "3") print $0;}' awk.txt
 $7 7.sütun demek


# CRONTAB 
**UBUNTU'DA ÇALIŞIR**
Belirlediğiniz bir zaman ya da zaman diliminde belirlediğiniz komut, script ya da uygulamanın çalışmasını sağlamak 
 crontab -e                 #  crontab file editleme
 crontab -l                 # crontab task listeleme
 crontab -u username -e     # diğer username editleme

```bash
* * * * * <shell command>   # her dakika
0 1 * * * <shell command>   # her gün saat 1'de
* * * 1 * <shell command>   # ocak ayında her dakika 
* * * * 6 <shell command>   # her cumartesi her dakika
0 1/15 * jan,jun mon,fri <command> # execute at every 1 a.m. and 3
                                     p.m. every monday and friday on
                                     january and june
```

