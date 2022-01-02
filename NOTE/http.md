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
