# server setup and maintenance final term 
![image](https://user-images.githubusercontent.com/62127656/147848323-079665a4-d060-4c3d-83bb-bcec88e969e9.png)
>網路問題重新連線

## Centos installation
* iso下載 (CentOS-7-x86_64-DVD-2009.iso)

![1](https://github.com/cycyucheng1010/NQU/blob/main/Centos7/week2-1.PNG)
![2](https://github.com/cycyucheng1010/NQU/blob/main/Centos7/week2-2.PNG)
## From NatworkManager to network
* ```ifconfig enp0s3```看enp0s3的ip netmask broadcast

![image](https://user-images.githubusercontent.com/62127656/147851039-48b5204f-a13b-46ea-9edd-e532a4f17c04.png)


* ```ip route show```看default route 

![image](https://user-images.githubusercontent.com/62127656/147851033-fc0e4cc9-9465-4108-b02a-6944cfa4f81b.png)
* NetworkManager&network一個啟動另外一個就要關閉
  1. ```systemctl stop NetworkManager ```
  2. ```systemctl disable NetworkManager```
  3. ```systemctl start network```
  4. ```systemctl status network```  
* 設定enp0s3的設定檔```gedit /etc/sysconfig/network-scripts/ifcfg-enp0s3```
```
TYPE="Ethernet"
BOOTPROTO=static
NAME="enp0s3"
DEVICE="enp0s3"
ONBOOT="yes"
IPADDR=10.0.2.20
NETMASK=255.255.255.0
GATEWAY=10.0.2.2

```
![image](https://user-images.githubusercontent.com/62127656/147851449-db8e8ff2-0e0a-4dec-86a4-b311529d33ee.png)

* 重啟網路 ```systemctl restart network```
* 查看網路 ```ifconfig``` ```ip route show```

![image](https://user-images.githubusercontent.com/62127656/147851495-ad2c4929-a05e-4212-b620-1d22f0ca5d80.png)
![image](https://user-images.githubusercontent.com/62127656/147851509-5848628f-9f2d-49c2-890e-43f46da3d92e.png)

* ping tw.yahoo.com and systemctl status  NetworkManager

![image](https://user-images.githubusercontent.com/62127656/147851535-51fc98f3-dcc9-4653-b760-030b5aabe56c.png)
![image](https://user-images.githubusercontent.com/62127656/147851551-a810da40-80e1-4f18-ab23-f10876e74756.png)

## ssh: scp&no password login
![6](https://github.com/cycyucheng1010/NQU/blob/main/Centos7/week2-6.PNG)

```sudo yum install openssh-server```

![image](https://user-images.githubusercontent.com/62127656/147726971-d46f197c-beec-49f1-b474-ce3bab1ebf0b.png)

```ifconfig```

![image](https://user-images.githubusercontent.com/62127656/147727008-10297aa6-b675-497b-85c7-879d39ead80f.png)

```ssh xxx@xxx.xxx.xxx.x```
```enter passord```

![image](https://user-images.githubusercontent.com/62127656/147729387-862f3772-0402-4615-81f2-656e11c915c9.png)


```ssh-keygen```
```scp /home/rick1010/.ssh/id_rsa.pub rick1010@192.168.239.140:/home/rick1010/.ssh/authorized_keys```
```ssh rick1010@192.168.239.140```顯示成功



## samba
## nfs
## vsftp
## haproxy
## www: php&mysql&virtualhost
* superuser
```su```
* 安裝apache
```yum install httpd```
* 關閉防火牆
```
systemctl status firewalld
systemctl stop firewalld
systemctl disable firewalld
```
* 設定完成重開
```reboot```
* 確認是否為Disabled
```getenforce```
* 改這行 SELINUX=Disabled
```
gedit/etc/selinux/config
```
* 查看apache是否成功啟動
```
systemctl start httpd
systemctl status httpd
```

![image](https://user-images.githubusercontent.com/62127656/147850131-afd19c6c-c5f9-466b-9818-c05a4bad15dd.png)

* install mariadb
```
yum install mariadb-server mariadb
```
* 啟動mariadb
```systemctl start mariadb.service```
* 查看mariadb的狀態
```systemctl status mariadb```
* database setting
  1. enter (keyboard)
  2. y 
  3. enter your password 
  4. neter your password again 
  5. n 
  6. n
  7. n
  8. y
* 進入mariadb
```mysql -u root -p```

![image](https://user-images.githubusercontent.com/62127656/147850372-5f6b188d-2dd7-410a-8c1a-f0ccf7ddc6ed.png)
![image](https://user-images.githubusercontent.com/62127656/147850503-d12d340e-93f8-4514-b30e-067c5309e6fc.png)

  1. ```show databases; ```
  2. ```create database testdb;```
  3. ```use testdb;```
  4. ```create table addrbook(name varchar(50) not null, phone varchar(10));```table名addrbook,varchar決定字元上限
  5. ```insert into addrbook(name, phone) values ("tom","1234567890");```輸入tom的值
  6. ```insert into addrbook(name, phone) values ("aa","1324567890");```輸入aa的值
  7. ```exit```
* ```yum install php php-mysql```下載php
* ```systemctl restart httpd``` 重啟apache
* ```gedit /var/www/html/test.php```建立php檔案內文如下:
```
<?php
$servername="localhost";
$username="root";
$password="rick1010";
$dbname="testdb";

$conn = new mysqli($servername, $username, $password, $dbname);

if ($conn->connect_error) {
    die("conneciton failed: ". $conn->connect_error);
}
//echo "connection ok"

$sql="select name,phone from addrbook";
$result=$conn->query($sql);

if($result->num_rows>0){
   while($row=$result->fetch_assoc()){
      echo "name:". $row["name"]." phone:". $row["phone"]."<br>";
   }
}
?>
```
## Bind
## tellnetd
