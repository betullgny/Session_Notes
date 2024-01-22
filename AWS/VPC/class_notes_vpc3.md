# VPC tekrar bilgiler:

    -   Private subnet için ===> Internet Gateway + Route Tables işlenmesi lazım!!
    -   aynı Vpc içerisinde Jump box/Bastion Host ile SSH agent ile private subnet e ulaşaibliriz.
    -   Natgateway veya nat instance ile subnetten internete çıkış yapabiliriz
    -   Natgateway >> AWS tarafından yönetilir, bakım, güvenlik vs onda

# 01.VPC Endpoint >> (00:10:20)

    -   AWS içerisinde (S3,DynoDB) VPC miz dışarısında AWS içerisinde (Non-VPC components) olan servise amazon backbone (AWS altyapısı) ile  (bir nevi tam olarak internete çıkmadan) ulaşmak için kullanılır.

![Alt text](VPC_endpoint-1.jpg)

# 02.VPC Peering >> (00:18:30)

    -   VPC ler arasında internete çıkış yapmadan/AWS içerisinde (internet gateway kullanmadan) bağlantı yapmak için kullanılır.
    -   VPC1-VPC2 arasında, VPC1-VPC3 arasında peering olsa bile VPC2-VPC3 arasaında bağlantı için peering GEREKİR

![Alt text](vpc-peering-1.jpg)

# 03.VPN & Direct Connect >> (00:26:00)

- AWS Site to Sİte VPN >> On-premise DB ile AWS arasına kurulan VPN (2 tünel kurulur, biri yedek)
- AWS Client VPN >> Remote Userın GÜVENLİ bir şekilde Client VPN endpoint aracılığıyla bağlanması

![Alt text](vpn-connections-1.jpg)

- Direct Connect >> On premise olan tesis ile AWS'yi AWS partner (AWS Connect Partner) üzerinden fiziksel kablo ile bağlama
  - Direct connect ile VPN hybrid olarak kullanılabilir.

# HANDS-ON >>> (01:00:00) (FARKLI OLARAK NatGateway ile değil NAT instance ile yapılacak)

    -   WINDOWS instance oluşturuyoruz:
        AMI             : Microsoft Windows Server 2022 Base
        Instance Type   : t2.micro
        Network         : **Default VPC
        Subnet          : Default Public Subnet a
        Security Group  :
            Sec.Group Name : WindowsSecGrb
            Rules          : RDP --- > 3389 ---> Anywhere

    -   Nat instance oluştur:
        AMI             : ami-0aa210fd2121a98b7 (Nat Instance)
        Instance Type   : t2.micro
        Network         : clarus-vpc-a
        Subnet          : clarus-az1a-public-subnet
        Security Group  :
            Sec.Group Name : Public Sec.group
            Rules          : SSH - ICMP yeterli
        +  NAT instance ayarını yap, NAT instance seç > Actions > Networking > Change Source / destination check >Stop işaretle
        +   VPC'ye git > Route Tables > Private RT > Routes > Edit Route > Add route > Destination :0.0.0.0., TArget > İnstance (NAT instance seç) > SAVE

    -   Bastion instance oluştur (dersin 2. bölümü için)
        AMI             : Amazon Linux 2023
        Instance Type   : t2.micro
        Network         : clarus-vpc-a
        Subnet          : clarus-az1b-public-subnet
        Security Group  : ssh-icmp yeterli

    -   private instance oluştur
        AMI             : Amazon Linux 2023
        Instance Type   : t2.micro
        Network         : clarus-vpc-a
        Subnet          : clarus-az1a-private-subnet
        Security Group  : SSH - HTTP açık olacak (garanti olması için HTTPs aç)

        user data       :

        #!/bin/bash

        yum update -y
        yum install nginx -y
        yum install -y wget
        systemctl enable nginx
        cd /usr/share/nginx/html
        chmod o+w /usr/share/nginx/html
        rm index.html
        wget https://raw.githubusercontent.com/awsdevopsteam/route-53/master/index.html
        wget https://raw.githubusercontent.com/awsdevopsteam/route-53/master/ken.jpg
        systemctl start nginx

    -    WINDOWS instance a remote bağlanma (01:15:00)
        + Windows instance a gir, connect kımını tıkla > RDP client a gir.
        +   Download remote desktop file > dosyasını indir ve yükle
        +   Get password ' e tıkla > key pairini upload et, şifreyi al/kopyala
        +   Yüklediğin remote desktop file a gir, şifre kısmına şifreyi kopyala ve bağlan (uzun sürüyor)
        +   VPC> Virtual private cloud > Peering connections e git. > Create peering connection a gir.
            +   isim ver
            +   request >  windows instance yani default VPC
            +   accepter > kabul eden kendi oluşturduğumuz VPC
            +   create VPC connection > oluştur, oluşan peering e gir ve REQUEST i onayla
            +   VPC> Virtual private cloud > Route Tablea git (01:28:20)
                -   Default RT e gir > Edit Route >  Destination : PRIVATE subnet IP, Target > Peering  (oluşturduğumuz peering  seç) > SAVE
                -   PRIVATE RT e gir > Edit Route >  Destination : DEFAULT subnet IP, Target > Peering  (oluşturduğumuz peering  seç) > SAVE
        +   UZAKTAN windows a gir, EDGE aç, private IPyi yapıştır ve KENi gör :)

# S3 BUCKETa ENDPOINT ile bağlanma ===> (01:56:00)

    -S3 bucket a git, S3 bucket oluştur.
        Object Ownership            : ACLs disabled
        Block all public access     : Checked
        Versioning                  : Disabled
        Server access logging       : Disabled
        Tags                        : 0 Tags
        Default encryption          : Disabled
        Object lock                 : Disabled
    -   Handson a yer alan 2 fotoyu UPLOAD et

    INTERNET ile BAĞLANMA:
    -   eval "$(ssh-agent)"  / ssh-add ./[your pem file name] / ssh -A ec2-user@ec2-3-88-199-43.compute-1.amazonaws.com    ===> komutları ile BASTION instance a bağlan
    -   ssh ec2-user@[Your private EC2 private IP]  ==> private instance a bağlan
    -   IAM > Roles > create Role > AWS SErvice > EC2 > Next > Permission policies > S3fullaccess> next > isim ver > Create Role
    -   PRIVATE instance tıkla > Actions > Security > Modify IAM Role > Az önce oluşturduğumuz rolü kaydet.
    -   aws s3 ls  >> ile S3 e bağlanabildiğini kontrol et. (internet ile bağlandık)

    ENDPOINT ile bağlanma
        + VPC > Virtual private cloud > Endpoint > create endpoint             (02:22:10)
            - isim ver
            -   services > s3 arat > Type: Gateway olanı SEÇ!!!! (ınterface on-premise den bağlantı, gateway AWS backbone ile içerden bağlantı)
            -   VPC > kendi VPC miz seç > Route table private seç(otomatik RT atar) > CREATE
        +    aws s3 ls  >> ile S3 e bağlanabildiğini kontrol et. (endpoint ile bağlandık)
