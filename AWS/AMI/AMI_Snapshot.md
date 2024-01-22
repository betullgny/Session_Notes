# AMI AND SNAPSHOT DERS NOTLARI #
* AMI: Bilgisayarın  işletim sistemi ve ona bağlı programlar
* AMI 3 ayrılır: 
  * Public: Görünürlük olarak herkese acık anacak kendi icerisinde ücretli ve ücretsiz olarak ayrılır
  * Paid : Devolapırlar tarafından üretilen ve berlirli bir ücret ödeyerek alırız örnek olarak sec grp, database ücret ödeyerek alabılırız
  * Private : Sadece bize gözükür ve görecek kisileri biz belirleriz aynı zamanda ücretsizdir

* Snapshot : Volüm EC2 ve EBS anlık olarak kopyasını olusturdugumuz zip dosyası olarak düsünebiliriz. Mantık datayı yedeklemek 
* Snapshot AMI den farklı AMI, Snapshot bir üst modeli 
* Snapshot kullanıma hazır degil zip olarak bekliyor diyebiliriz
* Snapshot nerden alınır biri Volume digeri Instance bunların beraberde snapshot alabiliriz ayrı ayrı da
* Snapshotlar S3 de tutulur
* Lifecycle of Snapshot:


* Volume ---->              --> Volume --> Encrypted
               --> Snapshot --> Backup
* Instance -->              --> AMI    --> EC2

* Volume ve Instanceden bir Snapshot alıyoruz
* Almıs oldugumuz shapshottan --> Volume --> Encrypted(Sınavlarda cıkar)
                              --> Backup --> Alabiliriz
                              --> AMI olusturabilir daha sonra bununla bir EC2 kaldırabiliriz
* Incremental: 3 cesit vardır, full backup göre biraz daha yavastır
  * Full Backup : hergün alınan backup, cok uzun sürer 
  * Incremental Backup : Bir önceki günü dikkate alır
                        Pazartesi --> 10 gb bir veri yedekledik  Pazartesi  SNAP--> 10 GB
                         Salı      --> Pazartesi alnınan 10 gblık verinin 4 gb degısıklık yaptıgımız varsayalım bu durumda 4 gb lık veririn backup alıyoruz  Salı SNAP-->  6+4 GB
                         Carsamba  --> Salı günü almıs oldugumuz toplam backup 10 gb üzerine 2 gb ekleme yaptıgımızı var sayalım bu durumda sadece 2 gb backup alıyoruz Carsamba SNAP--> 6+4+2 GB
                         "C:\Users\yasin\OneDrive\Masaüstü\software\Read Me\AMI_Snapshot\1.png"
  * Differential Backup : Full Backup takip eder her zaman aradaki farkın backupını alır 
  * Document Linki: https://aws.amazon.com/compare/the-difference-betwen-incremental-differential-and-other-backups/
  * Retention Period : Örnek olarak biz retention periodu 5 verdiğimizi var sayarsak son gün backup alır elinde her zaman 5 backup tutar sistemde bir hata cıkması durumunda geriye dönüs de saglıyor örnek: windows sistem geri yükleme
  * AWS Backup: data lifecycle management ec2 ya özelken, aws backup data lifecycle management yaptıgını da yapar ve tüm  propjeyı kapsayacak sekilde backup alabilir. Örnek olarak bir tag eklersek aws backup bizim belirledgimiz policylerle tüm proje içesinde belirtilen tag ait backupları alır.<sınavlarda cıkar>
  * Encryption of Root Device via Snapshot: Şifrelenmemiş bir volumü şifrelemek istememiz durumunda snapshot altıktan sonra kopyalarsak o esnada Encryption cıkar. Örnek olarak bastan bir ec2 yaratırken eger onun volumunu Encryption olarak yaratmadıysak onu Encryption etmenin yolu onun shapshot alıp aynı yere yada baska bir yere kopyalarken Encryption seceneğini tıklayarak onu Encryption hale getirebiliyoruz. Daha sonra ondan bir volume uretip  onu ait oldugu ec2 baglarız. Örnek bir harddiskimiz var bunun shapshot alıyoruz, shapshot alırken kopyalıyoruz, kopyalarken Encryp ediyoruz ve daha sonra tekrar bilgisayarımıza takıyoruz böylelikle harddiskimiz kriptolu oluyor.<sınavlarda cıkar>




 
  # Instance(Nginx)--> Snapshot --> Create Image(AMI) --> EC2 ##
*  "C:\Users\yasin\OneDrive\Masaüstü\software\Read Me\AMI_Snapshot\2.jpg"
## Instance(Nginx) ##
* Instance --> Launch an instance --> Name --> Application and OS Images --> Key pair (login)--> Key pair --> Advanced details --> User data 
  ```text
  #! /bin/bash
  yum update -y
  yum install nginx -y
  systemctl start nginx
  cd /usr/share/nginx/html
  chmod -R 777 /usr/share/nginx/html
  rm index.html
  wget https://raw.githubusercontent.com/awsdevopsteam/route-53/master/index.html
  wget https://raw.githubusercontent.com/awsdevopsteam/route-53/master/ken.jpg
  systemctl restart nginx
  systemctl enable nginx

## Snapshot ##
 --> Launch İnstance
* Sol menüde Elastic Block Store --> Snapshots --> Create snapshot --> Instance --> Instance ID --> Select an instance<yaratmıs oldgumuz ec2 secıyoruz> --> Description<random> --> Volumes - optiona --> <Exclude root volumesecmiyoruz:istersen volumleri harıc tutabılırsın,Exclude specific data volumes:belirli veri hacimlerini haric tutabilirsin,  >
* Tags --> Add tag --> Key:Name --> Value - optional:Description<random> ekleyebiliriz --> Create Snapshot


* Suana kadar Instance olusturk ve onun snapshot aldık 

## Create Image(AMI) ##
* Sol Menü --> Snapshots_-> on son olusturdugumuz shapshotu secıyoruz --> Actions --> Create İmage from snapshot --> 
* Image name<random> --> Description <random> --> create image nerden takip edebiliz--> sol menude Images--> AMIs den kontrol

* Almıs oldugumuz snapshottan amı urettik ve daha sonra da ec2 olusturucaz

## EC2 ##
* Instances --> Launch an instance --> Name and tags --> My AMIs --> Owned by me --> olusturmus oldugumuz amı -->geri kalan standart (Advanced details dokunmuyoruz) --> Launch İnstance

## Instance(Nginx)--> Create Image(AMI) --> EC2 ##
* Instance --> ilk olusturgumuz instance seciyoruz --> actions --> image and templates --> Create image --> Image name <random> --add tag --> Key:Name, Value :random --> Create İmage <snapshot almadan yapmıs olsak da arka planda yapıyor>
* Instances --> Launch an instance --> Name and tags --> My AMIs --> Owned by me --> olusturmus oldugumuz amı -->geri kalan standart (Advanced details dokunmuyoruz) --> Launch İnstance

# Root Volume of Instance(Nginx)--> Snapshot --> Create Image(AMI) --> EC2 ##

* Sol menünden --> Elastic Block Store --> Snapshots --> Create snapshot --> Snapshot settings --> Volume<daha once olsuturmus olduygumuz ec2 volumu> --> Description<random> --> add tag Key:Name, Value : random --> Create Snapshot
* Uretmıs oldugumuz volume snapshot secıyoruz actions --> Create image from snapshot --> Image name<random>, Description<random> --> create image --> amıs den kontrol edelim
* Instances --> Launch an instance --> Name and tags --> My AMIs --> Owned by me --> olusturmus oldugumuz amı -->geri kalan standart (Advanced details dokunmuyoruz) --> Launch İnstance

* NOT: Temelde bir ec2 ya baktıgımız zaman snapshot dayanır. AWS nin bir snapshot havuzu vardır bu havuzdan ımageler ve volume uretılır 

* Olusturmus oldugumuz amıler private bunu nasıl public yaaprız: amıs --> private yapma ıstedıgımız amılerın ıcerısıne gırıyoruz --> Permissions --> Edit AMI permissions --> public --> save chances 
* BU yapmıs oldugumuz amı nerden bulunur --> İnstance --> Launc isntance --> Browe more AMIs --> Community AMIs den buluruz

## WORDPRESS ##
* Wordpress : WordPress, kişisel bloglardan kurumsal web sitelerine, e-ticaret mağazalarından haber sitelerine kadar çeşitli türde web siteleri oluşturmak için kullanılabilir. WordPress'in açık kaynak kodlu olması, geliştiricilerin yazılımı özelleştirmesine ve ihtiyaçlarına göre uyarlamasına olanak tanır. Bu nedenle, dünya çapında birçok web sitesi WordPress'i tercih etmektedir.
* AWS de ise bunun ücretsiz kurulmusu var, İnstance --> Launc isntance --> Browe more AMIs --> AWS Marketplace AMIs --> sol menüden Free trial seciyoruz--> WordPress Certified by Bitnami and Automattic select --> Continue --> Instance type t2.micro olacak --> digerleri standart --> Launch İnstance

## DELETİON ORDER ##

* AMI --> silmek istegimizi secıyoruz --> Actions --> Deregister AMI
* Snapshot --> silmek istegimizi secıyoruz --> Actions --> Delete snapshots<ec2 calısırken silebiliyoruz ec2 da yüklü>
* Instances










# Hands-on EC2-05 : Working with EC2 Snapshots

Purpose of the this hands-on training is to teach students how to take a snapshot of EC2 instance, create an image from EC2 instance and using Data Lifecycle Manager. 

## Learning Outcomes

- At the end of the this hands-on training, students will be able to;

- take snapshots of EC2 instances on AWS console.

- create images from EC2 instances on AWS console.

- understand of difference between the image and the snapshot.

- create different types of AMI.

- using the Data Lifecycle Manager 

- coping and sharing AMI

## Outline

Part 1 - Creating an Image from the Snapshot of the Nginx Server and Launching a new Instance

Part 2 - Creating an Image and Launching an Instance from the same Image

Part 3 - Creating an Image from the Snapshot of the Root Volume and Launching a new Instance

Part 4 - Make AMI Public

Part 5 - Create Wordpress with AMI (Bitnami)

Part 6 - Using Data Lifecycle Manager 

## Part 1 - Creating an Image from the Snapshot of the Nginx Server and Launching a new Instance

- Launch an instance with following configurations.

  a. Security Group: Allow SSH and HTTP ports from anywhere with named "SSH and HTTP"

  b. User data (paste user data seen below for Nginx)

  ```text
  #! /bin/bash
  yum update -y
  yum install nginx -y
  systemctl start nginx
  cd /usr/share/nginx/html
  chmod -R 777 /usr/share/nginx/html
  rm index.html
  wget https://raw.githubusercontent.com/awsdevopsteam/route-53/master/index.html
  wget https://raw.githubusercontent.com/awsdevopsteam/route-53/master/ken.jpg
  systemctl restart nginx
  systemctl enable nginx
  ```

  c. Tag: Since "Data Lifecycle Manager" work based on tags, we use tag to customize Instance!!!!!!!! 

  ```text
  Key: Name 
  Value: SampleInstance  
  ```
- First copy the Instance ID and then go to EC2 dashboard and click "Snapshot" section from left-hand menu.


- Click `create snapshot` button.

Select resource type : Instance
Instance ID          : Select the instance ID of Nginx
Name(manually)       : Instance-Snapshot_First

- Click create snapshot.

- Click snapshot `Action` menu and select `create image`

```text
Name        : ClaruswayAMI_1
Description : ClaruswayAMI_1
```

- Click the `launch instance` tab.

- Click `myAMI` from left-hand menu.

- Select `ClaruswayAMI_1' AS AMI

- Launch instance named "Instance_1_from_Sample_Instance"

- Copy the public IP of the newly created instance and paste it to the browser.

- Show that the Nginx Web Server is working.

## Part 2 - Creating an Image and Launching an Instance from the same Image with "Action Menu"

- Go to `SampleInstance`

- Click the actions menu.

- Select image >> create image.

```text
Name        : ClaruswayAMI_2
Description : ClaruswayAMI_2
```

- Click AMI section from left hand menu and show `ClaruswayAMI_2`

- After ClaruswayAMI creation process is completed, click snapshot section from left-hand menu.

-  Show that AWS has created a new snapshot for newly created `ClaruswayAMI_2` image.

-  Click the `launch instance` tab.

- Click `myAMI` from left-hand menu.

- Select `ClaruswayAMI_2`.


- Launch instance as named "Instance_2_from_Sample_Instance_"

- Copy the public IP of the newly created instance and paste it to the browser.

- Show that the Nginx Web Server is working.

- Check the "Snapshot Menu" ıf there is an extra snapshot or not. If yes, then  name it. Explain the reason.

```text
Name : Snapshot_Second_Auto 
```
## Part 3 - Creating an Image from the Snapshot of the Root Volume and Launching a new Instance

- Go to EC2 menu and click snapshot section from left-hand menu.

- Click `create snapshot` button.
```text
Select resource type : ****Volume
Instance ID : select the root volume of the SampleInstance
Name(manually)       : Snapshot_Third 
```

- Go to the AMI menu and Click create AMI.

```text
Name        : ClaruswayAMI_3
Description : ClaruswayAMI_3
```
- Click the `launch instance` tab.

- Click `myAMI` from left-hand menu.

- Select `ClaruswayAMI_3`.

- Launch instance as named "Instance_3_from_Sample_Instance_"

- Copy the public IP of the newly created instance and paste it to the browser.

- Show that the Nginx Web Server is working.

## Part 4 - Make AMI public.

- First go to the AMI section  from left-hand menu and say students to not to do together.

- Select the ClaruswayAMI_4

- Click on permission and Click on  "Make public "

- After a while it will be public. Send the AMI ID from slack

- Tell the student to  go Launch Instance-----> Choose AMI------> Community AMI, and paste the "ID of AMI" to the search bar 

- Delete all AMIs and Snapshots. 

## Part 5 - Hosting WordPress on EC2 with AMI

- Create EC2 instnace
  - AMI: AWS Marketplace  : "WordPress Certified by Bitnami and Automattic" (Free tier)
  - Instance type         : t2.micro
  - Volume                : 10Gb is default.***
  - Tag                   : Key=Name, Value= WordPress
  - Sec.Grp               : HTTP & HTTPS & SSH are default ***
  - 
- Go to the browser and paste WP IP .

- to customize the page >>>>> after the IP add "/wp-admin"
- In the opening page you need a user name and password for customizing WP
- Select the wordpress Instance from console
   - Action  >>>  Monitor and Troubleshoot >>>>>> Get System logs >>>> "user name and password" 
- Go to browser and log in with credentials. 
- Delete the WP instance. 

## Part 6 - Using Data Lifecycle Manager :

- In the Amazon EC2 Console, under Elastic Block Store——>Lifecycle Manager——>Create Snapshot Lifecycle Policy. 

- You can select the policy type depending on your target component to snapshot. 

```text
Policy type: EBS snapshot policy
```

- You can select the resource type as volume or instance depending on whether you want to schedule snapshots. We will be selecting "Instance" as the resource type.

Select resource type: Instance

- Enter a description:

```text
Description: "Test policy"
```
- Now select the tags associated with the EBS volumes or EC2 instance, it will depend on the option chosen above. We are  including all the instances with tag name : 


```text
  Key: Name 
  Value: SampleInstance  
```
- 55.This policy must be associated with an IAM role that has the appropriate permissions. If you choose to create a new role, you must grant relevant role permissions and setup trust relationships correctly. If you are unsure of what role to use, choose Default role. 
```text
  Default Role 
```

- Schedules define how often the policy is triggered, and the specific actions that are to be performed. The policy must have at least one schedule. This schedule is mandatory, while schedules 2, 3, and 4 are optional.

```text
Policy Schedule 1
Schedule name : My_schedule
Frequency     : Daily
Every         : 24H
Starting at   : 03:00 UTC
Retention Type: Count 
Retain        : 5
```
- Copy the "Copy tags from source" option. Thanks to this option, tags from the source volume are automatically assigned to snapshots created by the schedule.

- Cross region copy

```text
Uncheck "Enable cross region copy"
```
- Cross-account sharing

```text
Check "Enable cross-account sharing"
```
- Confirm the Policy Summary to be sure that the rules are specified correctly, as per your requirement. Choose Enable policy to start the policy, runs at the next scheduled  time.

```text
Enable Policy
```
- Finally, choose Create Policy. 

- That's it, you're DLM policy is created. Check ıt from console. 

- Delete the policy. 