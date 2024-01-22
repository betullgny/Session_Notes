# **EC2 Block-Based Virtual Disks**
-cloud'a eklediğimiz depolama aracı (sanal disk)
-kaldırdığımız makinanın verilerini sakladığı yer veya uygulamalarını yüklediği yer
-Aws Block Based Virtual Disk kategorisinde 2 volume seçeneği sunar
# ---INSTANCE STORE ve ELASTIC BLOCK STORE 



# **Virtualization in EC2**
-sanallaştırmada bi fiziksek sunucu (hypervisor) ve üstünde bir sanallaştırma yazılımı (virtualization software ) bulunur
-bu sunucu ve yazılım katmanına hypervisor diyoruz
-Hypervisor, bünyesinde bulunan tüm bilgilere bağlı tüm makineler veya depolama cihazları tarafından erişilebilir olmasını sağlayan bir sistemdir.
-hypervisor'a doğrudan bağlanan ve hypervisor ile ilişkili her mankinanın erişebildiği sistemlere AWS'de EC2, ELASTIC BLOK STORE denir
# AVANTAJI; eğer physıcal server'da bir sorun oluşursa üzerinde çalışan sanal makinaların konfigürasyonları diğer makinalara geçer ve kesintisiz çalışmaya devam eder. Bu durumda Instance Block Storage hypervısor yerine hangi sanal makina çalışıyorsa ona bağlanır.

# **Instance Block Storage (Ephemeral)**
-sanal makianın üzerinde çalıştığı sunucuuya doğrudan bağlı olan diskleri kullanan depolama yöntemi
-SSD veya HDD Hard Disk'i olabilir
# AVANTAJI; yüksek erişim hızı ve düşük gecikme süresi
# DEZAVANTAJI: sanal makina (virtual machine) kapanırsa burada bulunan datalar kaybolur. fiziksel makinaya bir şey olursa veya vitual machine kapatılırsa disklerde bulunan dataya erişilemez

-çok hızlı veri alışverişi için ınstance store(fiziksel bağlı olduğu için..ec2 kapanınca bilgiler gider)
-snapshot için uygun değil
-ınstance store bir ec2'ya bağlı ve sadece onunla çalışır ama ebs'yi farklı ec2'ya bağlayarak da çalıştırabiliriz



# **EBS (Elastic Block Storage)** daha yaygın kullanıcaz 
-cloud'da bulunan bir sanal disk
-EBS, sanal makineye bağlanabilen ve bir işletim sistemi/uygulama içerisine kurulabilen depolama çözümüdür.
-EBS %99 erişilebilirlik oranı sağlar ve verileri aynı AZ içindeki farklı, birden fazla fiziksel cihaza kopyalar 
-windows veya linux da ec2 oluşturduğumuz zaman EBS gömülü olarak eklenir
# Instance Storage'dan farklı olarak EBS snapshot özelliğine sahiptir. bu şekilde EBS ile oluşturduğumuz dosyalarınkopyasını kaydedebilir ve bu doyadan yeni makina oluşturabiliriz
-(SSD): Baskın performans özelliğinin IOPS olduğu, küçük G/Ç boyutuyla sık okuma/yazma işlemlerini içeren işlemsel iş yükleri için optimize edilmiştir.
-(HDD): Baskın performans özelliğinin aktarım hızı olduğu büyük akışlı iş yükleri için optimize edilmiştir.
-hypervisor ile bağlı ec2'ya
-ec2 kapanınca ebs çalışmaya devam ediyor, **remote** çünkü ve çalışaya devam ettiği için para ödüyoruz
# **USE CASES(SINAVLARDA ÇIKAR)**
-fiziksel olarak ec2'ya network ile bağlı demek


# **block base**
-büyük dataları bloklara bölerek depolama 
-bu dataya sadece bağlı olan ec2 oluşuyor 
-segmentlere ayırma
-bir ec2ya birden fazla volume eklenir ama bir volume birden fazla ec2'ya bağlanmaz


 **IOPS and Throughput**
-IOPS saniyedeki input/output işlemleri demektir. IOPS, bir diske saniyede kaç okuma ve yazma yapılabileceğini belirten bir değerdir.(sürat)
-Throughput bir depolama sistemine saniyede kaç MB veri aktarımı yapıldığını gösterir (debi)
-IOPS diskin işlevsel hızı; THROUGHPUT işlem kapasitesi ile ilgili
-Datayı ne kadar hızlı işleme, hızlı işlem için IOPS, büyük hacimli datalarla uğraşırken THROUGHPUT
