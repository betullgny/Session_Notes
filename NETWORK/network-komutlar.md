# HANGİ KOMUTLAR İLE ÇALIŞICAZ..
--TRACEROUTE
--PİNG
--MTR 
--IPCONFİG
--IPCONFİGTABLES
--ARP 
--NSLOOKUP 
--NMAP
--ROUTE
--NETSTAT
--TCPDUMP
--TELNET VE SSH

# **TRACEROUTE**: 
-->paketin uzak server'a giderken izlediği yolu gösterir..
-->internet üzerinden gönderdiğimiz paketler(ICMP) nereye gider?
-->Bu paketler hedefe nasıl ulaşır?
-->trace route lar sürekli başka hop'ları kullanır (trafik yoğunluğundan)
# linux-- *trace route (dns name) or (ıp adress)*
# Windows-- *tracert (dns name) or (ıp adress)*
time out- cevap gelmemesi, cevap gelene kadar devam ediyor

# *traceroute çalışma mantığını videodan izle*
pc bir paket hazırlıyor ve ilk router'a gidiyor ve karşıdakinin bilgisini pc alıyor 
pc ikinci kez paket hazırlıyor ve router'a gönderiyor..
ulaşması gereken yere ulaşınca server kendi bilgisini yollar..
30 router üzerinden ulaşamadıysa paket çöp olur 

# çıktılarda 1e100.net-----> FQDN HER EC2'NUN KENDİNE AİT FQDN'Sİ VARDIR.1E100.NET GOOGLE DEMEK;
# EC2----> tracert Public ıpv4


# **PİNG**: 
-->host yanıt veriyorsa ve host sahibine ulaşabiliyorsanız..
-->bir ağdan yanıt alma ve yanıt yollama süresinin kontrol edilmesi 
-->ufak veri paketleri göndeerek bağlantıyı test eder
-->daha çok internet bağlantısında bir sorun varsa çalıştırıyoruz
# linux-- *ping google.com* (biz durdurdmadığımız sürece gider)
          ping -c 4 google.com (ping sayısını biz belirtiyoruz, burda cevap daha hızlı geliyor)
          ping -c 4 *[216.239.38.120]*
# Windows-- *ping (host name) or (ıp adress)* (dns sorgulaması yapari default 4 paket gider)
         *ping -a (ıp adress) # ip deki diğer bilgisayara ufak veri paketleri gönderip, alarak bağlantıyı test eder.
          “-a” parametresi ile ipdeki bilgisayarın adresi görüntülenir.
-->linux'de çalışırken durdurmamız gerekir (ctrl-c) ve ttl değeri bizim için önemli değil


# **PATHPİNG**:
-->Windows için *ping* ve *tracert* komutlarının birleşimi
-->tracert komutu gibi çalışır aslında
-->bu komut sayesinde routerlar üzeriden geçen verinin kayba uğrayıp uğramadığı kontrol edilir
-->traceroute'a göre daha kısa cevaplar verir
# linux veya windows -- **pathping google.com**


# **MTR** : 
-->Traceroute ve ping komutlarını birleştirir ve statiktir
-->gidiş dönüş süresi ve paket kaybını da gösterir
-->durduruna kadar paket yollanır (waiting for replay)
-->linux'te çalışır, windows'da karsılığı **pathpingdir**
# Linux--- mtr DNS name or IP adress
# Windows--- pathping DNS name or IP adress 


# **IPCONFİG** : 
-->ipconfig komutunun çıktısı, makinenizdeki temel yönlendirilmiş protokol bilgilerini sağlar
-->bilgisayarın agdaki ip adresini gösterir.
-->bilgisayarınıza bağlı tüm ağ aygıtları hakkında detaylı bilgileri öğrenebilir
-->ipconfg/all komutu ile daha detaylı bilgiler 
# Windows-- *ipconfig* ve *ipconfig/all* 
eğer bir DHCP sunucuna bağlıysak ipconfig/release ve ipconfig/renew kullanırız
# ipconfig /release —> bilgisayarın ağdaki ipsini bırakır 
# ipconfig /renew —> bilgisayarın ağdaki ipsini yeniler.
# yenileme işlemi sırasında ilk önce “release” , sonra “renew” işlemi uygulanır


# **IFCONFİG** : 
 -->TCP/IP yapılandırmasını görüntülemek için kullanılır
 -->ifconfig, TCP/IP protokolünü hem görüntülemek hem de yapılandırmak için kullanılır
 --> ifconfig interface [address [parameters]      
 -->Tüm Network interfacelerinin bilgilerini görüntülemek için kullanabiliriz. Bunun için -a parametresini kullanacağız 
--> aws default subnet atıyor 
 # Linux-- *ifconfig* ve *ifconfig -a* 

 # **IPTABLES** : 
      **(KULLANMAK TEHLİKELİ)**
 -->trafiklere izin vermek veya izin vermemek için
 -->sunucuya gelen, sunucudan çıkan ve sunucunun yönlendirdiği paketler için birer zincir tanımlar
 -->bu zincirler **input(inbound),output(outbound) ve forward**
      --igress (inbound)
      --egress (outbound)

 -->Paket eşleşme olduğunda ya kullanıcı tarafından tanımlanan başka bir zincire atlar ya da **DROP veya ACCEPT** edilir.
      
 **indirmek için sudo yuml install iptables -y**

# Linux-- 
192.168.10.1 numaralı cihazdan gelen bağlantıyı engellemek için:
# iptables -A INPUT -s 192.168.10.1 -j DROP (-s source)
172.16.0.0/16 ağındaki tüm cihazlardan gelen tüm bağlantıları engellemek için:
# iptables -A INPUT -s 172.16.0.0/16 -j DROP (bu subnet içinden gelen tüm cihazları bloklama)
10.110.61.5'ten itibaren SSH bağlantılarını engelleyin:
# iptables -A INPUT -p tcp --dport ssh -s 10.110.61.5 -j DROP
● Herhangi bir IP adresinden gelen SSH bağlantılarını engelleyin:
# iptables -A INPUT -p tcp --dport ssh -j DROP
      ---DENEMEYİNİZ!!!

# **FIREWALL**: 
      linux't ıptables
--> Windowsta firewall


# **ARP** : 
       **(KULLANMAK TEHLİKELİ)**
--> Arp tablosu; IP'yi öğrenmeden önce bakılan tablo
--> TCP/IP adreslerini MAC adreslerine çevirmek için kullanılır
--> Bir TCP/IP aygıtının yerel alt ağdaki bir aygıta bir paket iletmesi gerektiğinde, öncelikle ARP önbelleği veya MAC adresi arama tablosu adı verilen kendi tablosuna bakar.
--> Hedef IP adresini içeren bir ilişki bulunamazsa cihaz bir ARP yayını gönderir
# Windows-- Arp -a yaparak arp tablosu gösterilir. gösterilen bilgilerde dynamic olanlar manuel değiştirilebilir, *kullanmak bu yüzden tehlikeli*
         --DHCP DYNMANİC ıp'leri belirler
-->arp tablosunu incelemek, arp tablosuna kayıt eklemek, kayıt silmek
# Linux-- Arp -a 
-->ARP komutu hakkında daha fazla bilgi için arp --help


# **NSLOOKUP** : namespace look up
--> bir servisin ıp adresini listeler (dns sunucundan bilgi almak)
--> aradığın servise ait ıp adresleri verir fakat çalışıp çalışmadığını söylemez
--> 
--> ping gibi çalışır (ping google.com)
# Windows-- nslookup 
         -- nslookup google.com (output google'ın olduğu ıp adresleri olur)
         -- nslookup microsoft.com
         --nslookup -type=MX google.com  
# Linux--  nslookup [option] [hosts]
        -- nslookup -type=mx google.com
        -- linux da dig komutu kullanılır 
        -- dig google.com (çok fazla detay bilgi)


# **NMAP** :       
-->Port tarama komutu ve portların açık/kapalı olup olmadığına bakıyoruz
--> linux işletim sistemi ile birlikte gelir
-->Daha detayda siberciler kullanıyor 
-->bir network üzerindeki canlı bilgisayarları tespi etmek için de kullanılır
-->bir cihazda bir bağlantı noktası açıksa
# Linux--  nmap -help
           namp 
           nmap www.geeksforgeeks.org
           nmap 172.217.27.174
          ** nmap -v (public ip) ec2-user ail bilgiler dökülür ama detay olmaz
           nmap 176.42.139.57 (wan)
           namp 192.168.1.24  (cihazımın ip)
           nmap -v -A scanme.map.org 


# **ROUTE** :   
        **(KULLANMAK TEHLİKELİ)**
--> Network'te route table'ı görüntüler ve yönetir
--> tabel'ı biz düzenleyebilyoruz o yüzden tehlikeli
--> mesela gateway silinirse bağlantıyı gider, wifi'ye bağlanamaz
--> route [-f] [-p] [Command] [Destination] [mask Netmask] [Gateway] [metric Metric] [if Interface] 
--> subnet mask'i 255.255.255.0 ve next hop adress'i 10.2.2.2 olan 10.1.1.0 hedefine bir rota eklemek için
        *route add 10.1.1.0 mask 255.255.255.0 10.2.2.2*
--> subnet mask'i 255.255.0.0 olan 10.100.0.0 hedefine giden rotayı silmek istiyorsanız  
         *route delete 10.100.0.0 mask 255.255.0.0*      
# Linux--  route
        

# Windows--  route print (burda çıkan route table dinamik ve değiştirilemez. yeni eklemek için farklı bir komut çalıştırıyoruz)
         -- on link bağlı demek 
         -- on link dışındakileri default gateway'den ulaşırız (0.0.0.0 internet)
         -- route print 10.*
         -- route -p add [opt]
         -- route -p change [opt]
         -- route -p delete [opt]


# **CURL** :  
        *ÖNEMLİ*
--> Windows da yok
--> remote bağlandığımız cihaza dosya indirmek
--> curl komutu ile aplikasyonumuz ayakta mı diye bakıyor 
--> cevap gelene kadar devam ediyor
--> **http içeriğini text olarak alıp kopyalamak**
# Linux--  curl [options] [URL...]
           curl (link) bu şekilde sayfadaki her şeyi getirir
           curl (raw link) 
           curl (raw link) -o app.py komutundan sonra cat app.py ile içindekini görüntüle
           wget de aynı işlevi yapar hatta klasör indirir
           curl google.com (cevap alıyorsa aplikasyon çalışıyordur)


# **SCP** :  
--> Local'den remote'a herhangi bir şey gönderme veya kopyalama 
--> remote'dan kullanılmaz, genellikle local'den kullanılır 
         *bilgisayarın kendi public ip'si yok*
-->ssh bağlantısı aracılığıyla ağ üzerinden güvenli bir şekilde sistemler arasında dosya ve dizin aktarmak için kullanılan bir komut satırı aracı
# Windows-- scp <options> <files_or_directories> user@target-host:/<folder> scp <options> user@target_host:/files <folder-local-system> 
     ----- scp -i C:\.ssh\firstkey.pem ec2-user@IP:/home/ec2-user/app.py deneme.sh
     dir (görüntüleme)
     ------  scp -i C:\.ssh\firstkey.pem deneme.sh ec2-user@IP:/home/ec2-user/app.sh deneme.sh (DOSYA OLUŞTURMAZ)



# **SSH** :  
        telnet-güvenli değil 
--> Telnet ile aynı seçeneklerin yanı sıra çok daha fazlasını sağlar ve verileri şifrelenmiş biçimde aktarır
--> Localden remote cihaza bağlanmak için kullandığımız komut
-->SSH kullanmak için sunucularınızın, yönlendiricilerinizin ve diğer cihazlarınızın SSH ile etkinleştirilmesi gerekir 
   ssh user-name@host(IP or Domain Name)
# Linux--  ssh user-name@host(IP or Domain Name)
       ssh -i C:\.ssh\firstkey.pem (public ip)----> localden bu şekilde bağlanıyoruz  
       

# **TCPDUMP** :
-->  bir ağdan canlı olarak yakalanan paketleri veya bir dosyaya kaydedilen paketleri okumak için kullanılır
--> Kullanıcı bilgisayarına bağlı bulunduğu bir ağ üzerinden iletilen veya alınan TCP/IP paketlerini veya diğer paketleri kaydetme, inceleme ve filtreleme imkânı sunmaktadır.
# Linux-- tcpdump -i any: Tüm arayüzlerdeki trafiği yakalar
          tcpdump -i [eth0]: Belirli bir arayüzdeki trafiği yakalar
          tcpdump host 192.168.5.5: Trafiği, ister kaynak ister hedef olsun, IP'ye göre filtreler
          windump, tcpdump'ın Windows sürümüdür




# **NETWORK CONFIGURATION FILES** :   
         --linux
-->  Global yapılandırma dosyası 
--> we want networking (NETWORKING=yes|no)
--> what the hostname should be (HOSTNAME=)
--> which gateway to use (GATEWAY=)
-->"/etc/hosts" dns sunuucusu olmayan küçük networklerde hostname'i çözümlemek amacı 
# linux
       cat /etc/sysconfig/network
       NETWORKING= yes (yes dışında herhangi bir şey yazarsan ec2'yu ve cihazı tüm networkten izole etmiş oluruz)
       NOZEROCONF= yes

       cat /etc/hosts (dns server)
       cat /etc/resolv.conf (ec2'nun kullandığı dns server'ın ip'si)







# **FTP**: 
-->  dosyaların aktarımı için kullanılır
# Windows-- ftp
            open ftp..... (sunucu adı)   şifre ve kullanıcı adı ile giriş yapılır
            open ftp.gnu.org (anonim bir file transfer server'ı)
            close deyip bağlandığımız server'dan çıkar 
