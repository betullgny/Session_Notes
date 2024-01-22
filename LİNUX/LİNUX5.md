# Table Of Contents
Review--Shell
      --Bash 
Bash Prompt
Shell Scripts

# Kernel shell ile donanım arasında bağlantısı sağlar 
kullanıcılarının komut satırı arayüzlerini kontrol etmelerini sağlayan bir bilgisayar program türüdür
Shell user ınterface'dir
Bash, csh, zsh, tcsh türleri mevcut 

# Bash Prompt 
Bash renk değiştirme 
 export komutu ile renk değiştirilir 
 export $PS1

# SHELL SCRİPT
# Shebang (#!)
Shell Script dosyaları .sh uzantılı olmalı 
oluştururken vim, nano operatörleri kullanılabilir
.sh uzantılı dosyanın çalıştırılabilmesi için executable olması gerekir 
chmod +x ile dosya çalıştırılabilir hale gelir
dosyayı 2 şekilde çalıştırabiliriz;
 ./class.sh 
 bash class.sh 


# VARIABLE 
Değişken atama 
$VARIABLE=value 
DEğişken user belirleyebilir veya değişkeni user'dan da isteyebilir

 #!/bin/bash
 NAME=paul 
 AGE=35
 echo $NAME
 echo $AGE 

**KURALLAR**
= önce/sonra boşluk olmamalı
a-z veya A-Z kullanılabilir
0-9 arasında sayılar kullanılabilir
_ underscore ile başlayabilir
!, *, - shell için kullanılmaz 

# Console İnput
User'dan isteyebiliriz

 #!/bin/bash
 echo "enter your name : "
 read NAME 
 echo "Hello $NAME" 

Kullanıcıdan input almayı tek satırda gerçekleştirebiliriz
 #!/bin/bash
 read -p "enter your name" USERNAME
 echo "welcome $USERNAME"

 read -s -P "enter your password" PASSWD
 echo -e "\nyour password is $PASSWD"

Birden fazla ifade yazdırmak için
 #!/bin/bash
 echo "what cars do you lıke"
 read CAR1 CAR2 CAR3

 echo "your first car is $CAR1"
 echo "your second car is $CAR2"
 echo "your thırd car is $CAR3"                          #yerleri de değişebilir. output değişmez

# **YUKARIDAKİ KODU TEK SATIRDA NASIL YAZABİLİRİZ**
 read -p "what cars do you like?" CAR1 CAR2 CAR3
 echo "your first car is $CAR1"
 echo "your second car is $CAR2"
 echo "your thırd car is $CAR3"  

# COOMMAND LINE ARGUMENTS
$0 - bash script'in adı      ./script.sh 
$1-$9                         ARG1, ARG2, ARG3....ARG10

 echo "my script is $0"          #script ismini verir
 echo "first arg is $1"          #birinci argüman verir
 echo "second arg is $2"         #ikinci argüman 
 echo "third arg is $3"          #üçüncü argüman
 echo "number of all arg is $#"  #tüm argümanların sayısını verir
 echo "all arg is $@"            #tüm argümanların ne olduğunu tek tek verir
 echo "$RANDOM"                   #random sayı üretme (0-32.767 arasında bir sayı üretir)
 echo "$(($RANDOM%10))"          #

 test.sh çalıştır                # Burda argümanları girmemiş olduk bu yüzden output eksik olur 
 test.sh BIR IKI UC DORT

echo "File Name is $0"
echo "First Parameter is $1"
echo "Second Parameter is $2"
echo "Third Parameter is $3"
echo "All the Parameters are $@"
echo "Total Number of Parameters : $#"
echo "$RANDOM is a random number"
echo "The current line number is $LINENO"  #hangi satırdaysa onu verir

# ARRAYS 
 #!/bin/bash
 DISTROS[0]="ubuntu"
 DISTROS[1]="fedora"
 DISTROS[2]="debian"
 DISTROS[3]="centos"
 DISTROS[4]="alpine"

 devops_tools=("docker" "kubernetes" "ansible" "terraform" "jenkins")

 echo ${DISTRO[0]}
 echo ${DISTRO[1]}

# SIMPLE ARITHMETIC
3 farklı kullanım yöntemi var
expr item1 operator item2    
# STANDART OUTPUT
expr değerleri aarsında mutlaka boşluk bırakılmalı

 #!/bin/bash
 expr 5 + 2
 expr 7 - 1

 read -p "enter first number:" NUM1 #7
 read -p "enter second number:" NUM2 #9

 let TOTAL=NUM1+NUM2
 echo "total is $TOTAL"
 echo "NEW TOTAL is $((++TOTAL))
 
let <arithmetic expression>  
# DEĞİŞKENİN SONUCUNU BAŞKA DEĞİŞKENE ATAMA
 let "sum = 3 + 5"
 echo $sum

 let "sum = 3 + 5"  #çift tırnaksız da sonucu verir
 echo $sum
# kod
 x=5
 let x++
 echo $x

 y=3
 let y--
 echo $y 
# kod
 read -p "Input first number: " first_number
 read -p "Input second number: " second_number

 let "sum = $first_number + $second_number"
 let "sub = $first_number - $second_number"
 let "mul = $first_number * $second_number"
 let "div = $first_number / $second_number"
 echo "SUM=$sum"
 echo "SUB=$sub"
 echo "MUL=$mul"
 echo "DIV=$div"

 let first_number++
 let second_number--
 echo "The increment of first number is $first_number"
 echo "The decrement of second number is $second_number"
# kod
# **burda önce new_number'a number ataması yapıldı. sonra number 1 arttırılır**
 number=10
 let new_number=number++   # This firstly assigns the number then increases.
 echo "Number = $number"            
 echo "New number = $new_number"

# kod
# **burda önce işlem yapılır sonra atama yapılır**
 number=10
 let new_number=--number   # This firstly decreases the number then assigns.
 echo "Number = $number"
 echo "New number = $new_number"
 
 sum=$((3 + 5))
 echo $sum 

 sum1=$((12 + 9))
 echo $sum1