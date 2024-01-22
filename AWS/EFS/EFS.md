# **EFS**
- EFS 'serverless' ve 'set and forget' olarak çalışan bir elastik file system 
- AWS cloud service and On Premises resource'lar ile kullanılabilir 
- Amazon EFS, dosya ekledikçe veya kaldırdıkça depolama kapasitesini otomatik olarak artıracak ve azaltacak şekilde tasarlanmış
- Yani esnek kapasiteli bir depolama çözümüdür
- Depolama alanı sağlamaya gerek kalmadan otomatik olarak ölçeklendirilebilir (atomatically scale)
- Amazon EC2, Amazon ECS, Amazon Elastic Kubernetes Sercice, Aws Fargate ve Aws Lambda EFS kullanabilir
- Amazon EFS, bir dosya sistemine aynı anda bağlanan bir ila binlerce Amazon Elastic Compute Cloud (Amazon EC2) örneğini destekler.
- Her region'da max 100 file system olabilir
- Sadece depolama için ödeme yapılır
- EFS linux tabanlı ec2'lar için uygundur, windows için desteklenmez

# **EFS FEATURES**
- scalable, LOW COST 

# **ATTACHING**
- Birçok ec2'ya bağlıdır 


# **EFS STORAGE CLASSES**
- Aws file system oluşturabilmek için iki tip depolama tipi sunar; iki tip altında kullanım durumuna göre 4 tip depolama sınıfı vardır
-        STANDART
-        ---> EFS STANDARD; sık erişilen veriler için bölgesel bir depolama sınıfıdır. Bir region'da birden fazla AZ'DA verileri yedekleyerek yüksek ulaşılabilirlik ve dayanıklılık sağlar (mount target'a az ulaşılması durumlarında kullanılır)
-        ---> EFS STANDARD IA; az erişilen veriler için bölgesel bir depolama sınıfıdır. Bir region'da birden fazla AZ'DA verileri yedekleyerek yüksek ulaşılabilirlik ve dayanıklılık sağlar (mount target'a sık ualşılması durumlarında kullanılır)

-        ONE ZONE
-        ---> EFS ONE ZONE; Bir region'da bir AZ'da sık erişilen dosyalar için depolama
-        ---> ONE ZONE IA; az erişilen, düşük maliyetli depolama sınıfır. Bir region'da bir AZ'da verileri yedekleme


# **COMPARİSON OF STORAGE SYSTEMS**
- Cost optimize; S3 > EBS > EFS
- Speeed; EBS, EFS > S3 (S3 http zerinden olduğu için zaman gerekir, EFS tcp daha hızlı s3'e göre)
- EC2 MOUNT; S3-- no, EBS--Single EC2, EFS--Multiple EC2
- Storage Capacity; S3, EFS=00, EBS=64/16TİB
- 