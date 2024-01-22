
# **TERRAFORM (ıInfrastructure as Code ( IaC ) )**
- Cloudformation gibi 
- Birçok platformda kullanılabilir (Azure, Google cloud, Aws vb)
- Mimariyi kod yazarak oluşturmak
- Building, Changing and Managing infrastructure in a safe 

# *Why Insfrastructure As A Code?*
- Standartlaştırma
- otomatikleştirme
- ağır sistemlerde manuel yapmak hatanın farkedilebilirliğini azaltır, çözümü zorlaştırır

# *How terraform work?*
- Human readable language called HCL (HashiCorp Configuration Language)
- .tf uzantılı dosyada resourceleri yazar ve 'apply' ile mimari kaldırılır
- farklı platformlarda aynı anda resource'lar oluşturulabilir
- Two manin parts,
-     Terraform Code: Terraform binary that communicates, config file'ı okuma, bilgileri tutma (terraform'un kendisi) 
-     Terraform Plugins: Plugin based, terraform içinde oluşturmak i.in servis için gerekli platfoorm ile iletişim kurma pluginler ile yapılır. 
- Terraform supports one type of plugin called **PROVIDERS**

# *Workflows*
- Write; 
- Initialize; downloads the correct provider plug-ins for the project, init komutu
- Plan; terraform yazdıktan sonra hatayı azaltmak için plan görmek, plan komutu
- Apply; oluşturma

# *Components*
# STATE;
- oluşturduğumuz bütün makinaların bilgileri, verileri state'te tutar
- .state uzantılı dosyadır
-  siz oluşturduğunuz altyapıyı manuel olarak değiştirseniz bile Terraform state’ini kontrol edip onu eski haline getirecektir. Bu da bize **Immutable Infrastructure** sağlamaktadır

# PROVIDERS
- Aws, Gogle Cloud, Azure, Kuberbetes, open stack vb platformalara **provider** denir
- API interactions

# MODULES (ÖNEMLİ)
- oluşturduğumuz her terraform configuration file bir **module**
- şablon bir file gibi düşünülebilir (hazır moodul'ler de var)

# BACKENDS
- hiçbir şey belirtilmezse terraform nerde çalışıyorsa orda tutulur
- backend sensitive information'ları tutma

# RESOURCE BLOCKS
- most important element in the terraform 
- Adından da anlaşılacağı gibi altyapıdaki belirli bir kaynağı ifade eder (EC2 Instance, S3 Bucket vb.) Kaynak, kod içerisinde tanımlanmasına göre oluşturulur.
- resource type, local name..
-       resource "aws_instance" "foo" {
-          ami           = "ami-005e54dee72cc1d00" # us-west-2
-          instance_type = "t2.micro"
-       }

# HANDSON-1
- Ec2 ınstance oluşturulur (ınstance type t3a.small seçilir, t2.micro yetmeyebilir)
- Açılan ınstance'a ssh remote bağlanılır ve terraform indiriilir;
-     sudo dnf update -y
-     sudo dnf install -y yum-utils 
-     sudo dnf -y install terraform
- Her tool'da komutlar tool ismi ile başlar;
-     terraform init, terraform plan, terraform apply vb 
- terraform version; terraform versiyon kontrolü  
-     terraform'un versiyonu ve provider'ları hızlı değişir, kontrol edilmeli
- terraform --help; terraform komutları hakkında bilgi
- terraform -help apply; apply komutu ile kullanılabilecek komutlar 
- terraform üzerinden aws'de bir ec2 oluştururken resource block oluşturmalıyız (terraform registry; https://registry.terraform.io/browse/providers )    
- aws configure ayarları ile provider bloğunda aws access kkey, secret access key ve region yazılarak aws'de bir resoruve oluşturulabilir. Bu yöntemde credentials public durumda olur, güvenlik sorunu oluşturur
- IAM management console'dan ROLE oluşturularak resource oluşturulmalı
- IAM management'dan ec2, s3 yetkisi olan bir role oluşturulur ve EC2'ya attach edilir. Bu şekile çalışılan ec2 makinasının s3'e erişmesine yetki verilmiş olur
- mkdir terraform-aws && cd terraform-aws && touch main.tf 
-     çalışılacak dosya dizini oluşturma
- **Install the `HashiCorp Terraform` extension in VSCode**
- main.tf terraform uzantılı file; resource blockları, providerlar vs burada yazılır (main yerine başka isim de yazılabilir, .tf uzantılı olmalı)
- registry dokumantasyonundan resource bazlı kod bloğunu çekerek main.tf dosyasında resource oluşturulur
- terraform block;
-     terraform {
-       required_providers {
-         aws = {
-           source = "hashicorp/aws"
-           version = "5.30.0"                 
-         }
-       }
-     }
- provider block;
-     provider "aws" {
-       region = "us-east-1"
-     }

- eğer role beliremeseydik provider bloğuna access key, secret key credentials'larını yazmalıydık (GÜVENLİ DEĞİL!!)

- resource block; 
- Ec2 için mutlaka 2 data içermeli; resource type ve resource name 
-     resource "aws_instance" "tf-ec2" {
-       ami           = "ami-0230bd60aa48260c6"
-       instance_type = "t2.micro"
-       tags = {
-          "Name" = "created-by-tf"
-       }
-     }

- Hazır terraform'u çalıştırmak için; 
-     terraform init
-     terraform plan (optional)
-     terraform apply 
- terraform init komutu çalıştırdığımızda provider plugin'ları indirilir (.terraform dosyası)
- AWS offlcal bir platform olduğu için terraform block ve provider block yazılmadan da resourve block ile ec2 oluşturabilirdi. Diğer platformlarda bu geçerli değil 
- terraform init dedikten sonra önce backend başlatılır. main.tf dosyasında bir backend (s3 belirtilebilir mesela) belirtilmezse state dosylarını local'de tutar
- backend çalıştırılır, pluginler indirililr ve .terraform.lock.hcl dosyası oluşturulur
- lock dosyası; terraform bloğunda yazılan versiyonu kilitlemek.. bu durumda terraform block içinde versiyon değiştirilirse ;
#     **terraform plan yapmadan önce "terraform init -upgrade" komutunun çalıştırılmsı gerekir** 
- terraform apply komutundan sonra .tfstate dosyası oluşur
-     terraform state list komutu; state dosyasını getirir
- yenir bir resource eklerken yine .tf dosyasında resource block açılarak istenilen servis seçilir;
-     resource "aws_s3_bucket" "tf-s3" {
-       bucket = "oliver-tf-test-bucket-addwhateveryouwant"
-     }
- resource bilgilerinde değişiklik yapma ve yeni bir resource ekleme durumunda "terraform plan" komutu çalıştırılır
-     terraform destroy ; terminates resource

# TERRAFORM VALIDATES
- Dosyanın doğru olup olmadığını kontrol etme
- aslında değişken atama 

# TERRAFORM FORMAT 
- Dosyayı daha okunaklı hale getirme
- terraform dosyayı çalıştırır ama okunak değildir..format komutu ile düzenli hale getirir

# TERRAFORM CONSOLE
- Çalıışması için terraform kurmuş olamnız gerekir
- state dosyasından çalışır 
- resource'lar ile ilgli bilgiileri console'a gitmeden gösterir
- 'terraform console' komutu ile interaktif bir console açılır
-      aws_instance.tf.private_ip / aws_instance.tf.public_ip   **state dosyasından çeker**
-      min (1,2,3) fonksiyonu ile min değeri getirir
-      lower fonksiyonu >> lower('hello')
-      file("${path.module}/cloud)      **path dosya yolu veya dizini, cloud dosyasını okut demek aslında**

# TERRAFORM SHOW COMMAND
- terraform show; state'i daha okunaklı hale getirme
- json/yaml gibi bir formatta getirir
-      terraform state show (console açılır)
-      ec2,s3 istediğin resource ait bilgileri çekebilirsin


# TERRAFORM GRAPH COMMAND
- terraform graph
- Resource'ları şematik hale getirme
- arayüz için localde kurulabilir

# TERRAFORM OUTPUT COMMAND
- terraform output; main.tf dosyasına output eklenir
- output "tf_example_public_ip" {
  value = aws_instance.tf-ec2.public_ip 
}
- bir resource'a ait hangi bilgiyi istiyorsan state dosyasında output{} şeklinde kaydedilir
- Dokümantasyonda "Attribute Reference" kısmından bakılabilir
- terraform output -json ; çıktıyı json formatında verir
- main.tf dosyasında; sensitive istediğin bilgiyi çıktıda gizler 
-      output "tf_example_s3_meta" {
-        value = aws_s3_bucket.tf-s3.region 
-        sensitive = true  
-      }                         

