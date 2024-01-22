# *ELASTIC IP*
- Elastic IP static ıpv4 adresi'dir
- Private subnet'ten direkt internet bağlantısı yoktur. Bu sorunu çözmek içn NAT GATEWAY veya NAT INSTANCE oluşturulur
# *NAT INSTANCE AND NAT GATEWAY*
- NAT GATEWAY aws tarafından yönetilir, NAT INSTANCE customer tarafından yönetilir 
- Bazı aws servisleri için elastic ıp gerekir (route53 veya NAT gateway(elastic ıp kullanılmak zorunda) gibi)
- Elastic ıp'leri nat gateway ile attach ederek nat gateway ile (private subnetteki servisler için) internet bağlantısı sağlanır
# *BASTION HOST* 
- BOSTION HOST(jump box); private subnet içindeki ınstance'a inbound traffic sağlar. NAT GATEWAY ise farklı olarak outbound traffic (çıkış) sağlar
- Public subnet ve private subnet aynı VPC içinde oldukları için localde Bostion Host ile iletişim kurabilir, bunun için internet çıkışına gerek yok 
- NAT INSTANCE da hem inbound hem de outbound traffic için kullanılır, public subnet içine kurulur. Bu şekilde private ve public subnet içindeki servisler için local'de iletişim sağlar ve private subnette yer alan resource için internete çıkış kapısını oluşturur
- Nat ınstance, nat gateway ve bastion host public subnet'te olmak zorunda 


**Nat gateway ile Bastion Host arasındaki fark;**
- Nat gateway private subnette bulunan ec2'nun internet çıkış(outbound connectivity) için, bastion host ise kullanıcı için private subnetteki ec2 (inbound connectivity) ile iletişim kurabilmesi
- Nat gateway'i dışardan private subnetteki resource ulaşmak için kullanamayız, aynı şekilde bostion host'u outbound connectivity olarak kullanmayız

# *HANDS ON*
- Private adında, betul vpc'de, subneti private 1a olan bir EC2 kaldırdık
-      Auto assign public IP enable olsa da İWG olmadığı için public ıp üretmez
- Sec grup olarak > create sec grup > ssh ve ıcmp bağlantısı olan bir name 
-      Sec grup inbound ssh > anywhere
-               inbound all ıcmp > anywhere (ping atacağımız için ICMP seçtik)
- Private EC2'ya SSH ile nasıl bağlanırız;
-      ssh -i /c/.ssh/firstkey.pem ec2-user@private-ip      
#     **BAĞLANMAZ; ssh ile bağlanabilmesi için public ip gerekir ama private subnette public ip üretilmez. Bağlanabilmek için BASTION HOST OLMALI**

- Bastion host adında, betul vpc'de, subneti public 1a, 1b veya 1c olan bir EC2 kaldırdık
-      **subnet 1a olmak zorunda değil, aynı vpc içinde olduğu için farklı az olabilir**
- Sec grup olarak > create sec grup > ssh ve ıcmp bağlantısı olan bir name 
-      Sec grup inbound ssh > anywhere
-               inbound all ıcmp > anywhere (ping atacağımız için ICMP seçtik) 
- Bastion host EC2'ya bağlanmak için;
-      ssh -i /c/.ssh/firstkey.pem ec2-user@public-ip
-      **sudo hostnamectl set-hostname bastion-host && bash (EC2 **hostname ismini değiştirme)**
-      ping (private EC2 private-ip)

- Private EC2 security group 22 port ve source 0.0.0.0/0 (22 port ile her yerden bağlanılabilir durumda)
-      bu best practice değil yani en güvenli hali değil
-      public'ten yani bastion host üzerinden gelsin trafik..manuel olarak; (edit inbound > ssh > source bostion host public ıcmp)

- Private EC2'ya ssh üzerinden bağlanmak için keypair lazım;
-      local'de bulunan keypair'ı bastion host'a alıp basiton host ile private EC2'ya bağlanılmalı
-      BİRİNCİ YÖNTEM; scp (localden ec2ya dosya vs atma) komutu ile yapılabilir ama bu ec2'dan local'e yapılmaz, publib ip'si yok
-      İKİNCİ YÖNTEM; LOCAL BASH;  eval "$(ssh-agent)" > ssh'ın ajanı, output olarak Agent pid numarası verir
-      ssh-add ./[your pem file name] > ./ yerine keypair'ın olduğu dosya yolu yazılmalı
-      For me; ssh-add C:\Users\pc\.ssh\firstkey.pem
-      ssh -A ec2-user@bastion-host-public-ip
-      **BASTİON HOST'A BAĞLANDI**
- Bastion host'a bağlandıktan sonra SSH ile private ec2'ya bağlanma;
-      ssh ec2-user@private ec2-private ıp (keypem'i ssh içine gömülü olarak almış olduk)
-      PRIVATE EC2'YA BAĞLANDI
-      sudo hostnamectl set-hostname private-instance && bash
-      EXIT yaparak private ec2'dan bastion host ec2'ya geçebiliriz


# *HANDS ON 2*
- Önce bastion host sonra da private ec2'ya bağlanma;
-     eval "$(ssh-agent)" > ssh-add ./[your pem file name] > ssh -A ec2-user@bastion-host-public-ip > ssh ec2-user@private ec2-private ıp
- Private EC2;
-     sudo yum update -y  ; private ec2'nun internete çıkışı yok, NAT GATEWAY LAZIM (***PARALI***)
-     Nat Gateway için ELASTIC IP olmak zorunda
-     Allocate Elastic IP Adress > Tag ekle > Allocate
-     Nat Gateway > create > subnet; betul-public > connectivity type; public > Elastic IP > create nat gateway
-     **NAT GATEWAY'İ PRİVATE ROUTE TABLE'A BAĞLAMALIYIZ**
-     private route table > routes > edit route table > add route > dest 0.0.0.0/0 ; target nat-gateway (oluşturduğumuz NGW) > save changes
-     PRIVATE EC2'DAN İNTERNET'E ÇIKIŞ VERMİŞ OLDUK 
-     test etmek için ping <bastion public ip>
-     NAT GATEWAY silindikten sonra route table'dan da kaldırılmalı


# *HANDS ON 3*
- NAT INSTANCE kurarak private ec2'ya hem giriş hem çıkış yapabilme;
-     Nat ınstance için EC2 kaldırırken farklı AMI kullanılır 
-     browse more AMIs > search nat > communıty AMIs > ami-0aa210fd2121a98b7 (nat) 
-     network settings > betul-vpc > subnet; 1c public > select sec grp; public sec grp > launch ınstance
-     **private ec2'dan ping atamazsın, İGW'yi route table'dan kopardık**
-     **NAT INSTANCE BASTION HOST OLARAK DA KULLANILABİLİR**
-     Private subnetteki resource'ların internete çıkışını NAT INSTANCE ile yaparız ama nat ınstance route table'a eklenmeli
-     route table > private route table > edit routes > dest; 0.0.0.0/0 target; nat-ınstance > save changes
-     Test etmek için; ping <nat ınstance public ip>  PİNG ATILMAZ!!
-     Nat ınstance > actions > networking > change source/dest > source/dest checking stop ACTIVE > save 
-     Test etmek için; ping <nat ınstance public ip>  PİNG ATILDI!!
-     Test etmek için; ping <bastion host public ip>  PİNG ATILDI!!
-     **Artık private ec2'nun internete çıkışı var ama sudo yum install git -y yapmaz  sebebi; nat ınstance 22 ve all port açık. HTTP ve HTTPS de açılması lazım** 


