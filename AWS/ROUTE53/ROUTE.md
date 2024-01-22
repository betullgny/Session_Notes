- Routing- policy'ler ile sağlanır 

# **ROUTING POLİCİES**
- Farklı senaryolarda trafiği sonfigüre etme 
- Trafği hangi policy'lere göre yönlendiririz
- Simple Routing Policy; no health check..aws burda load balancer gibi ip servis sağlıyor
-     Load balancer REGION BASED olması, simple routing policy global yani tüm regıon'larda load balancer gibi olabilr
-     **Br policy seçmek zorunda**
# **FAILOVER ROUTING POLICY**
- Health check ile sorun tespit ediliyor
- Site patladığı zaman faılover routıng devreye girer

# **WEIGHTED ROUTING POLICY**
- Bazı ec2'lara daha az yük vermek
- Beta version olarak da adlandırılır
- Random version 
- **Oranı** user belirler (SINAV SORUSU)

# **LATENCY ROUTING POLICY**
- Gecikme bazlı bakar olaya 
- Hızlı cevap beklediğinde bunu kullanırsın
- Oyun, video vs gibi durumlarda serverlar arasında zaman tutma
- Yakınlık bir kriter değil, ping atıp hangi server'ın daha hızlı olduğunu test eder

# **GEOLOCATION ROUTING POLICY**
- User önemli kriter, user'ın talebine göre content belirler
- Burda da hız önemli ama asıl önemli olan talep
- lokasyonlara göre farklı site içerikleri var
-     **Country and Continent**

# **GEO PROXIMITY ROUTING POLICY**
- sanal sınırlar, üretici belirler 
- talep gelen lokasyona göre bir katsayısı durumu belli olabilir..sanal sınır  olduğu için üretici belirler

# **MULTIVALUE ROUTING POLCY**
- Birden fazla health check yerine 8 farklı ınstance ve health chechk
- farklı lokasyonlarda health check yapar

# **IP BASED ROUTING POLCY**
- CIDR locations içinde olanlar bağlanır diğerleri yasak 
- Giriş/çıkış kontrol edilebilir veya sınırlandırılabilir


# **DNS HEALTH CHECKING**
- health chech ücretli
- önce health check oluşturulur sonra referans gösterilir

# **HANDS ON**
- EC2 kaldırdık ve user data ekledik (default vpc)
- Yeni EC2 kaldırdık ve user data 2 ekledik (default vpc)
- Yeni EC2 kaldırdık ve user data 3 ekledik (default vpc)
- Yeni EC2 kaldırdık ve user data 4 ekledik (betul vpc)
- Yeni EC2 (windows) kaldırdık ve user data 5 ekledik (betul vpc)
-      windows ec2 ile 4.ec2 aynı vpc'de
- 