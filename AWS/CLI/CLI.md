## AWS CLI Session Notlar ##
* CLI amacı AWS servislerine ulaşmak için kullanırız
* AWS penceresinden CLI terminalin güçlendirilmiş hali bunu da acsess key ve secret access key ile sağlıyoruz
* CLI AWS konsoldan güçlü olduğuna örnek: konsoldan 160 gb veri yüklenebilirken CLI da sonsuz 
* Bazı komutların konsolda karsılıgı olmadıgın ıslemler clı uzerınde yapılır
* CLI AWS 2023 yuklu geldigi icin Ubuntu EC2 acıyoruz , 


## UBUNTU PROCESS STEPS ##
1. curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip" ## CLI zipli doyasını indiriyoruz
2. sudo apt install unzip # unzip programını yüklüyoruz
3. unzip awscliv2.zip # zipli dosyayı cıkarıyoruz
4. sudo ./aws/install # clı yukluyoruz
5. export PATH=$PATH:/usr/local/bin/aws # her klasörde calıstırmak icin PATH ekliyoruz
6. aws --version # verison kontrol
7. EC2- Terminate 

## ACSESS KEY AND SECRET ACCESS KEY CREATE  ##
Önelikle acsess key ve secret access key indirmemiz gerekli
   * IAM ile giriş --> Users --> Aktif olaraak kullanmış olduğumuz IAM user tıklayıp içerisini giriyoruz --> Security credentials tıkla --> access key (aynı anda en fazla 2 tane aktif olabiliyor) geliyoruz creat access key --> Command Line Interface(CLI) - Confirmation-next --> isim verelim --> Create acces key --> Secret acces key burda bır daha goruyoruz onun ıcın ındırmemız gerekli-dowland.csv.file yada show yapıp not almamız gerekli --> Done -->> Contine
## AWS CONFIGURATION SETTINGS ##
   * aws configure -->
   * AWS Access Key ID [None]: indirmis oldugumuz acsess key
   * AWS Secret Access Key [None]: indirmis oldugumuz secret access
   * Default region name [None]: hangi regions da acılmalı us-east-1 
   * Default output format [None]: yaml
   * cd .aws -ls --> config ve credentials dosyaları gör
   * cat .aws/config --> user bılgılerı gelir
   * cat .aws/credentials --> acsess key ve secret access key bilgilerini görürüz
   * aws iam list-users --> tüm userleri gösterir
   * aws configure list-profiles (list of the profiles ) --> tüm userleri gösterir
   * aws sts get-caller-identity (Who am I) --> default kım ona bakıyoruz
## USER CREATE ##
   * aws configure --profile user1 (Configure the terminal for additional user ) --> 2. bir user yaratmak icin
   * aws configure list-profiles --> oluturmus oldugumuz profilleri görebiliriz
   * cat .aws/config --> confing dosyasının içerisinde olusturmus oldugumuz profilleri görebiliriz 
   * aws iam list-users --> yazarsak tüm userlar gelir
   * aws iam list-users --profile user1 --> sadece profile user1 gelir
   * Ornek: aws s3 ls dedigimizde tüm s3 bucket leri gelir <aws s3 ls --profile user1> dersek user1 tanımlı s3 bucketleri gelir birden fazla userda islem yapmak istiyorsak <--profile user* eklememiz gerekli>
   * export AWS_PROFILE=user1 --> user1 aws profile yapıyoruz 
   * aws sts get-caller-identity (Who am I) --> aktif kullanıcı kım ona bakmak istersek
   * export AWS_PROFİLE=default --> default dönmek istersek
   * user1 profile silmek istersek --> vim .aws/confing ve vim .aws/credentials ayrı ayrı girip burdan silebiliriz
   ## ANATOMY OF CLI COMMANDS ##
    * aws s3 cp help -->
      * aws --> servis adı
      * s3  --> hangi servisi kullanacaksak
            --> kullanılacak servisler=   https://awscli.amazonaws.com/v2/documentation/api/latest/index.html
      * cp  --> servisle ne yapacaksınız
      * help -> 
   * aws s3 help --> s3 den sonra neler yazılabilir alternatif --> < https://awscli.amazonaws.com/v2/documentation/api/latest/index.html>
   ## LİST AND CREATE IAM USER ##
      * aws iam list-users --> userleri listeler
      * aws iam list-users --output table --> userleri tablo halinde getirir
      * aws iam create-user --user-name aws-cli-user --> user olusturma 
      * aws iam list-users | grep aws-cli-user --> user arama
      * aws iam delete-user --user-name aws-cli-user --> user silme 
      * Servilerin nasıl kullanılacagına ilişkin dökümantasyon:https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iam/index.html 
   ## S3 ##
      * aws s3 ls --> bucket var mı kontrol ediyoruz
      * aws s3 mb s3://osvaldo-cli-bucket-osvaldo233333(uniq olmalı) --> s3 oluşturuyoruz
      * touch in-class.yaml --> lokal de bulundugumuz klasorde yaml dosyası olusturuyoruz
      * aws s3 cp in-class.yaml s3://osvaldo-cli-bucket-osvaldo233333 --> cp ile in-class.yaml dosyasını s3://osvaldo-cli-bucket-osvaldo içerisini kopyalıyoruz konsoldan kontrol edebiliriz
      * aws s3 cp in-class.yaml s3://osvaldo-cli-bucket-osvaldo/new/in-class.yaml --> s3 icesinde bulunan in-class.yaml kopyalayıp /new/ yeni bir klasor icerisine atabiliriz --> konsoldan kontrol edebiliriz
      * Kopyaladıgımız dosyanın ismini degistirip de kopylayabiliriz örnek: aws s3 cp in-class.yaml s3://osvaldo-cli-bucket-osvaldo/new/in-CLAAAAAAAS.yaml
      * aws s3 rm s3://osvaldo-cli-bucket/new/in-class.yaml --> olusturmus oldugumuz bos bucket siliyoruz
      * aws s3 rb  s3://ykartal-cli-bucket --force --> olusturmus oldugumuz dolu bucket siliyoruz
   ## BASH #
      * #!/bin/bash
      * yum update -y --> sistemi güncelle
      * yum install -y httpd --> Apache HTTP sunucusunu yükle
      * cd /var/www/html --> html klasörüne git altına 
      * aws s3 cp s3://osvaldo-pipeline-production/index.html . --> at
      * aws s3 cp s3://osvaldo-pipeline-production/cat.jpg . --> at
      * systemctl enable httpd --> Apache HTTP sunucusu hizmetini bu * komutla otomatik başlatılabilir hale getirir.
      * systemctl start httpd --> Apache HTTP sunucusu hizmetini başlat
     ## LAUNCH AN EC2 #
      * aws ec2 run-instances \                 --> ec2 ayaga kaldır
           --image-id ami-053b0d53c279acc90 \   --> image id bu olsun --> güncel kodu asagıda var
           --count 1 \                          --> 1 tane olsun
           --instance-type t2.micro \           --> instance-type
           --key-name "firstkey"                --> .pem yazmıyoruz
         konsoldan kontrol edebiliriz  
      *  aws ec2 describe-instances --instance-ids <ec2 id yazılır> --> EC2 ile ilgili bilgileri görmek istersek
      * aws ec2 describe-instances \
           --filters "Name = key-name, Values = KEY_NAME_HERE"  --> burada Name ve Values sabit key-name ve KEY_NAME_HERE yerlerine ne yazmamız gerektigini dökümandan alıyoruz(dersde link verimedi)
      * aws ec2 describe-instances \
   --filters "Name = image-id, Values = ami-053b0d53c279acc90" --> olusturmus oldugumuz ec2 goruruz
      * aws ec2 describe-instances --query "Reservations[].Instances[].PublicIpAddress[]" --> Reservations altında Instances altında PublicIpAddress bigisini bana getir tüm calısan ec2 ların publıc ıpadresleri gelir
      *  aws ec2 describe-instances \
   --filters "Name = key-name, Values = KEY_NAME_HERE" --query "Reservations[].Instances[].PublicIpAddress[]"  --> KEY_NAME_HERE yerine kendi pemimizi yazıyoruz tek ıp adresi geliyor
      * aws ec2 stop-instances --instance-ids INSTANCE_ID_HERE #put your instance id --> ec2 stop etmek icin
      * aws ec2 terminate-instances --instance-ids INSTANCE_ID_HERE #put your instance id #ınstance olur --> terminate etmek icin
   ## CURRENT AMİ #
      * aws ssm get-parameters --names /aws/service/ami-amazon-linux-latest/al2023-ami-kernel-default-x86_64 --region us-east-1 --> güncel ami



## AWS CLI Session 1 : 

- This hands on explains how to install and configure AWS CLI. We'll also see how to create and manipulate the resources in AWS via AWS CLI 

### References
- https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-welcome.html
- https://awscli.amazonaws.com/v2/documentation/api/latest/index.html
- https://docs.aws.amazon.com/linux/al2023/ug/get-started.html
- https://docs.aws.amazon.com/systems-manager/latest/userguide/parameter-store-public-parameters-ami.html
- https://aws.amazon.com/blogs/compute/query-for-the-latest-amazon-linux-ami-ids-using-aws-systems-manager-parameter-store/


## Learning Outcomes

At the end of the this hands-on training, students will be able to;

- installing CLI on Windows, Linux or MAC O/S

- configuring CLI

- creating a resources with CLI

- working with Amazon Linux 2023 AMI

## Outline

- Part 1 - Installation

- Part 2 - Configuration

- Part 3 - Examples of the CLI commands

- Part 4 - Working with the latest Amazon Linux 2023 AMI


## Part 1 - Installation

### Step-1  Installation CLI on your "Local" 

- You can use the link below to install AWS CLI V2 according to your O/S.

- General page:
https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html


- Windows:
https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html


- Mac:
https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html
https://graspingtech.com/install-and-configure-aws-cli/


- Linux:
https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html

```
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
sudo apt install unzip  #install "unzip" if not installed
unzip awscliv2.zip
sudo ./aws/install
```

### Step-2 Installation CLI on Linux (Ubuntu EC2)

#### Section-1 Creating an Ubuntu EC2

- Since  "Amazon Linux 2023" image is installed with "CLI Version 2" by default we create an EC2 instance with  "Ubuntu EC2" image so that we can practice how to install CLI V2. 

```text
-AMI             : Ubuntu 22.04
-Instance Type   : t2.micro
-Security Group  : SSH-22
```

#### Section-2 Install CLI Version 2 

- Check and Install AWS CLI Version 2
```
aws --version 

curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip" ## CLI zipli doyasını indiriyoruz
```

- install "unzip" if not installed than unzip 
```
sudo apt install unzip # unzip programını yüklüyoruz
unzip awscliv2.zip 
sudo ./aws/install
```

- Update the path accordingly if needed (AWS CLI Version 1 uses>>>> /usr/bin/aws)
```
export PATH=$PATH:/usr/local/bin/aws
```
or you may type >>> "bash"


## Part-2  Configuration - Local

### Step-1 Creating Access Key ID and Secret Access Key

- Go to the IAM service

- From the left hand menu 
```
  Click Users ---->> Select user---->>Security Credential--->> Access keys --->>>Create Access key 
```
### Step-2 Configuring (!!!!!!We keep going with Local)

- Configure your terminal with AWS Access Key ID and Secret Access Key for CLI
```
aws configure #güncellemek ve yenıden oluşturmak ıcın 
```

- than fill the information and hit the ENTER 
```
AWS Access Key ID [None]: ****************
AWS Secret Access Key [None]: ****************
Default region name [None]: us-east-1
Default output format [None]: yaml
```

- Show the file inside the ./aws folder
```
cat .aws/config # user bılgılerı
cat .aws/credentials  #key ıd ve access ıd bılgılerı
```
- Check the existing profiles:
```
aws configure list-profiles (list of the profiles ) # tüm user ları getırır
aws sts get-caller-identity (Who am I) #default kım ona bakıyoruz
```

- Show how to use CLI on single terminal with multiple user:
```
aws configure --profile user1 (Configure the terminal for additional user ) yenı kullanıcı için terminali yapılandırın
aws iam list-users --profile user1 # kullanıcı secıyoruz#
aws s3 ls --profile user1 _user1 # dıye bırı var mı dıye nakıyor

 aws configure --profile vera # user olusturma
- Check the existing profiles again:
```
aws configure list-profiles # profıllerı lıstele
```

- Switch the current profile to "custom user" profiles
```

export AWS_PROFILE=user1 # kullanıcı degıstıme, aws sts get-caller-identity (Who am I) #default kım ona bakıyoruz
```

- Switch the current profile to "default" user profile again.
```
export AWS_PROFILE=default # kendime geri donuyoruz
```

- try to use CLI after deactivate KEYs from IAM. 


## Part 3 Examples of the CLI commands: 

- talk about the anatomy of CLI commands 
```
aws 
aws help # # yardım
aws s3 help # s3 hakkında bılgı
aws s3 cp help
```

### Step 1- IAM

- List and Create IAM user 
```
aws iam list-users # userlerı getırır

aws iam list-users --output table # tablo halınde getır

aws iam list-users | grep ykartal # ykartal ıam ıcınde arama

aws iam create-user --user-name aws-cli-user # user oluturma

aws iam list-users | grep aws-cli-user # user cli-user arama

aws iam delete-user --user-name aws-cli-user # user silme
```

### Step 2 - S3

- check the existing S3 buckets and create a new bucket named "osvaldo-cli-bucket"
```
aws s3 ls
aws s3 mb s3://osvaldo-cli#yeterlı oluyor#-bucket-osvaldo # s3 oluşturuyoruz
```

- create a file in the existing directory and copy this file to newly created bucket 
```
touch in-class.yaml # metin oluşturuyoruz
aws s3 cp in-class.yaml s3://osvaldo-cli-bucket-osvaldo # metin s3 kopyallıyoruz
```

- copy this file to new folder inside bucket 
```
aws s3 cp in-class.yaml s3://osvaldo-cli-bucket-osvaldo/new/in-class.yaml # metin s3 kopyallıyoruz
aws s3 ls s3://osvaldo-cli-bucket-osvaldo/new/ # ls yaptık
aws s3 rm s3://my-bucket/my-folder/my-file.txt    aws s3 rm s3://my-bucket/my-folder --recursive silem           
```

- show the example of the user data that we used in IAM session while covering ROLES. Since we don't have any AWS CLI configuration while launching an EC2, the CLI command inside the EC2 fetch the data via ROLES rather than CLI credentials. 
```
#!/bin/bash

yum update -y
yum install -y httpd
cd /var/www/html
aws s3 cp s3://osvaldo-pipeline-production/index.html .
aws s3 cp s3://osvaldo-pipeline-production/cat.jpg .
systemctl enable httpd
systemctl start httpd 
```

- Check inside of the newly created bucket.
```
aws s3 ls s3://osvaldo-cli-bucket
```

- delete the object inside the bucket 
```
aws s3 rm s3://osvaldo-cli-bucket/new/in-class.yaml # bacıtı bossa sılıyoruz
```
- remove the bucket
```
aws s3 rb  s3://ykartal-cli-bucket --force # bacıtı dolu sılıyoruz
```


### Step 3 - EC2
- check the available commands for ec2 
```
aws ec2 help
```

- list the EC2 instances
```
aws ec2 describe-instances
```

- list the EC2 instances in specific region
```
aws ec2 describe-instances --region us-east-2 --output table 
```

-  Launch an EC2 instance (Note that this command may not work in PowerShell because of the different meaning of "\" )
```
aws ec2 run-instances \
   --image-id ami-053b0d53c279acc90 \
   --count 1 \
   --instance-type t2.micro \
   --key-name "firstkey"
```
# instcance kuruyoruz
- List the instances filtering with key name and image id.
```
aws ec2 describe-instances \
   --filters "Name = key-name, Values = KEY_NAME_HERE" # put your key name

aws ec2 describe-instances \
   --filters "Name = image-id, Values = ami-053b0d53c279acc90" # image id of EC2
```

- List the instances only with specific attribute. This command list the Public IP addresses of all EC2
```
aws ec2 describe-instances --query "Reservations[].Instances[].PublicIpAddress[]" # publıc ıd gelır
```


- You can combine "filtering and query" 
```
aws ec2 describe-instances \
   --filters "Name = key-name, Values = KEY_NAME_HERE" --query "Reservations[].Instances[].PublicIpAddress[]" #put your key name without .pem
```
and 


```
aws ec2 describe-instances \
   --filters "Name = instance-type, Values = t2.micro" --query "Reservations[].Instances[].InstanceId[]"
```

- You may want to call multiple values under the single attribute. For example under the "Instances" attribute I need both "InstanceId and PublicIpAddress":
```
aws ec2 describe-instances  \
  --filters "Name = instance-type, Values = t2.micro" "Name = key-name, Values = KEY_NAME_HERE" \
  --query "Reservations[].Instances[].{Instance:InstanceId,PublicIp:PublicIpAddress}" #put your key name without pem
```


- Let's stop and terminate the instance
```
aws ec2 stop-instances --instance-ids INSTANCE_ID_HERE #put your instance id # stop etmek

aws ec2 terminate-instances --instance-ids INSTANCE_ID_HERE #put your instance id #ınstance olur # term etme
```

## Part-4  Working with the latest Amazon Linux 2023 AMI

- Call the latest version of AL2023
```
aws ssm get-parameters --names /aws/service/ami-amazon-linux-latest/al2023-ami-kernel-default-x86_64 --region us-east-1
```

- Filter the image ID of latest AL2023
```
aws ssm get-parameters --names /aws/service/ami-amazon-linux-latest/al2023-ami-kernel-default-x86_64 --query 'Parameters[0].[Value]' --output text # eon guncel ımajı gelır
```

- Launching EC2 instance with latest AL2023 AMI. 
```
aws ec2 run-instances \
   --image-id $(aws ssm get-parameters --names /aws/service/ami-amazon-linux-latest/al2023-ami-kernel-default-x86_64 --query \
               'Parameters[0].[Value]' --output text) \
   --count 1 \
   --instance-type t2.micro
```