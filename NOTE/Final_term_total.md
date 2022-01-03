# server setup and maintenance final term 
![image](https://user-images.githubusercontent.com/62127656/147848323-079665a4-d060-4c3d-83bb-bcec88e969e9.png)
>網路問題重新連線

* Systemctl stop firewalld
* Systemctl disable firewalld
* Gedit /etc/selinux/config
* SELINUX=disabled
* reboot


## Centos installation
* iso下載 (CentOS-7-x86_64-DVD-2009.iso)

![1](https://github.com/cycyucheng1010/NQU/blob/main/Centos7/week2-1.PNG)
![2](https://github.com/cycyucheng1010/NQU/blob/main/Centos7/week2-2.PNG)
## From NatworkManager to network
1. ```ifconfig enp0s3```看enp0s3的ip netmask broadcast

![image](https://user-images.githubusercontent.com/62127656/147851039-48b5204f-a13b-46ea-9edd-e532a4f17c04.png)


2. ```ip route show```看default route 

![image](https://user-images.githubusercontent.com/62127656/147851033-fc0e4cc9-9465-4108-b02a-6944cfa4f81b.png)
3. NetworkManager&network一個啟動另外一個就要關閉
  1. ```systemctl stop NetworkManager ```
  2. ```systemctl disable NetworkManager```
  3. ```systemctl start network```
  4. ```systemctl status network```  
4. 設定enp0s3的設定檔```gedit /etc/sysconfig/network-scripts/ifcfg-enp0s3```
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

5. 重啟網路 ```systemctl restart network```
6. 查看網路 ```ifconfig``` ```ip route show```

![image](https://user-images.githubusercontent.com/62127656/147851495-ad2c4929-a05e-4212-b620-1d22f0ca5d80.png)
![image](https://user-images.githubusercontent.com/62127656/147851509-5848628f-9f2d-49c2-890e-43f46da3d92e.png)

7. ```ping tw.yahoo.com``` && ```systemctl status  NetworkManager```

![image](https://user-images.githubusercontent.com/62127656/147851535-51fc98f3-dcc9-4653-b760-030b5aabe56c.png)
![image](https://user-images.githubusercontent.com/62127656/147851551-a810da40-80e1-4f18-ab23-f10876e74756.png)

## ssh: scp&no password login
![6](https://github.com/cycyucheng1010/NQU/blob/main/Centos7/week2-6.PNG)

1. 安裝ssh ```sudo yum install openssh-server```

![image](https://user-images.githubusercontent.com/62127656/147726971-d46f197c-beec-49f1-b474-ce3bab1ebf0b.png)

2. 確認ip```ifconfig```

![image](https://user-images.githubusercontent.com/62127656/147727008-10297aa6-b675-497b-85c7-879d39ead80f.png)

3. 登入測試```ssh xxx@xxx.xxx.xxx.x``` ```enter passord```

![image](https://user-images.githubusercontent.com/62127656/147729387-862f3772-0402-4615-81f2-656e11c915c9.png)


4. ssh keys```ssh-keygen```
5. 將key傳到另一台電腦```scp /home/rick1010/.ssh/id_rsa rick1010@192.168.239.140:/home/rick1010/.ssh/authorized_keys```
6. 無密碼登入並成功```ssh rick1010@192.168.239.140```



## samba
1. ```yum install samba samba-client samba-common -y```
2. ```cd /``` ```ls```` ```mkdir mysamba``` ```mkdir mysamba2```
3. ```gedit /etc/samba/smb.conf```

![image](https://user-images.githubusercontent.com/62127656/147853007-3ced6243-886f-4581-a6b2-d0434302c829.png)
```
[mysamba]
	comment = Shared Directory 
	path = /mysamba 
	browseable = yes 
	writable = yes 

	create mode = 0660 
	directory mode = 2770 

	public = yes 
[mysamba2]
	comment = Shared Directory 
	path = /mysamba2 
	browseable = yes 
	writable = yes 

	create mode = 0660 
	directory mode = 2770 

	valid users = bb 
```
4. 重啟並查看狀態```systemctl restart smb``` ```systemctl status smb```
5. 可看可寫可讀```chmod 777 mysamba/``` ```chmod 777 mysamba2/```
6. 針對root設定密碼```smbpasswd -a root```
7. 利用windows連進去```\\192.168.56.101```
8. 然後新增a.txt到mysamba

![image](https://user-images.githubusercontent.com/62127656/147853254-506a7f56-9f1b-44ce-88db-f9bc63e8ce3c.png)
![image](https://user-images.githubusercontent.com/62127656/147853236-557aa955-dcc7-49b2-a8f9-ee3cb14b8a10.png)

9. ```net use * /delete```
10. 新增使用者 ```cd /home``` ```adduser bb``` ```passwd bb``` ```smbpasswd -a bb```
## nfs
1. 兩台都root```su``` ```mkdir mysamba```
2. 副機ifconfig確認ip然後本機ping副機```ping 192.168.56.108```
3. 副機建立資料夾 mynfs ```cd/``` ``` mkdir mynfs```
4. 主機建立資料夾mydata ```cd/``` ```mkdir -p /mydata
5. 主副機確定防火牆關閉 ```getenforce```理論上要是disabled ```gedit /etc/selinux/config```改成這個SELINUX=Disabled，然後重新開機
6. 主機副機安裝nfs ```yum install -y nfs-utils```
7. 副機: 決定規則(檔案內容 分享哪個資料夾 分享給誰 以下是所有節點 可讀可寫)```gedit /etc/exports``````/mynfs 192.168.56.101/24(rw,sync,no_root_squash,no_all_squash)```
8. 副機:啟動rpc bind跟nfs```systemctl start rpcbind``````systemctl start nfs-server```
9. 副機: ```rpcinfo -p``` ```exportfs -r``` ```exportfs```

![image](https://user-images.githubusercontent.com/62127656/147852237-ad533497-1acc-4413-ab13-d28d6ec6b5ec.png)
>副機成功共享

11. 主機: ```systemctl start rpcbind``` ```systemctl status rpcbind``` showmount -e 192.168.56.102```

![image](https://user-images.githubusercontent.com/62127656/147852364-9019c877-5755-4c54-b174-a6bc04931aba.png)
>主機成功看到

12. 主機:與副機掛載```mount -t nfs 192.168.56.102:/mynfs /mydata```
13. 副機: ```cd /mynfs``` ```chmod 777 /mynfs``` ```touch  {a..d}``` ```echo "hi" > hi.txt```
14. 主機: ```cd /mydata```

![image](https://user-images.githubusercontent.com/62127656/147852483-7ba42601-e7ab-4942-a479-9e29ca44fbbf.png)
>可看見檔案

15. 主機: 拿掉掛載先離開掛載資料夾 ```cd ..``` ```umount /mydata```
## vsftp
1. 安裝vsftp ```yum install vsftpd -y```
2. 開啟並查看狀態 ```systemctl start vsftpd``` ```systemctl status vsftpd```
3. 檢查阜號是否作用 ```netstat -tunlp | grep 21```
4.  利用windows cmd登入ftp，帳號:```anonymous``` 密碼: 任意email address

![image](https://user-images.githubusercontent.com/62127656/147874875-cd330701-d08d-49d5-9607-c8a806dee0c1.png)
>可利用cd ls pwd等指令，若要離開輸入bye即可

5. 利用winscp登入，選擇ftp帳號密碼一樣
6. 回到centos7:　```cd /var/ftp/``` ```chmod 777 pub``` ```ll```
7. ```mkdir upload``` ```mkdir dowload```
8. ```chmod 777 upload/``` ```chmod777 dowload/```
7. 修改設定檔```gedit /etc/vsftpd/vsftpd.conf```找到allow_ftpd_full_access底下的```#anon_upload_enable=YES```去掉```#```
8. 若要讓匿名者可新增資料夾則找到# new directories底下的```#anon_mkdir_write_enable=YES```去掉```#```
9. 重啟vsftp : ```systemctl restart vsftpd```
10. 副機下載ftp並連主機 ```yum install ftp``` ```ftp 192,168,56,101```name :user password : 可跳過
11. 將```anonymous_enable=YES``````anonymous_enable=NO```讓匿名者無法登入
12. 使用者登入限制在家目錄```#chroot_local_user=YES```改成```chroot_local_user=YES``` ```allow_writeable_chroot=YES```
## haproxy
* 主機、副機1、副機2
1. 副機1: ```systemctl start httpd``` ```systemctl status httpd``` ```gedit /var/www/html/index.html```內文輸入: centos7-1
2. 副機2: ```systemctl start httpd``` ```systemctl status httpd``` ```gedit /var/www/html/index.html```內文輸入: centos7-2
3. 檢查副機1是否成功 

![image](https://user-images.githubusercontent.com/62127656/147873796-e9252dad-2e4d-41d0-98e8-ca8e95cf457f.png)

4. 主機: ```getenforce``` ```systemctl status firewalld```
5. 安裝套件: ```yum install epel-release``` ```yum install haproxy -y``` 
6. 複製檔案: ```cp /etc/haproxy/haproxy.cfg /etc/haproxy/haproxy.cfg.bak```
7. 修改設定檔內容如下: ```gedit /etc/haproxy/haproxy.cfg```
```
global
  daemon
  chroot /var/lib/haproxy
  user haproxy
  group haproxy
  stats timeout 30s

defaults
  mode http
  log global
  option httplog
  option dontlognull
  timeout connect 5000
  timeout client 50000
  timeout server 50000

frontend http_front
  bind *:8080
  stats uri /haproxy?stats
  default_backend http_back

backend http_back
  balance roundrobin
  server server_name1 192.168.56.102:8080 check
  server server_name2 192.168.56.104:8080 check

```
8. 查看ip 得到結果

![image](https://user-images.githubusercontent.com/62127656/147874344-4198a0cc-d0fe-4f1e-a443-cf7161b06d9f.png)
![image](https://user-images.githubusercontent.com/62127656/147874350-0bd11b43-fedd-4f67-8f04-5b11616cf073.png)
>第一得到centos7-1 再次刷新centos7-2

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
1. 確定防火牆關閉 是disabled ```selinux```
2. ```systemctl status firewalld```
3. ```yum install bind bind-chroot bind-utils -y```
4. ```systemctl start named``` ```systemctl status named```
5. 挖取pchome的網路資料```dig @127.0.0.1 www.pchome.com.tw```

![image](https://user-images.githubusercontent.com/62127656/147878355-18e502b3-8edc-4908-bf7f-3cf327b3aa5c.png)

6. 挖取nqu的網路資料```dig @127.0.0.1 www.nqu.edu.tw``` 

![image](https://user-images.githubusercontent.com/62127656/147878386-032a82c5-a6dd-457b-8c0b-90f3053ef56a.png)

7. 查詢nqu的host```host -t A www.nqu.edu.tw 127.0.0.1```

![image](https://user-images.githubusercontent.com/62127656/147878419-c71b44e9-b765-4302-9ff5-9dcb5fd9281a.png)

8. nslookup www.pchome.com.tw 192.168.56.101

![image](https://user-images.githubusercontent.com/62127656/147878727-a99f5812-d50a-4d1b-b19b-f52c4b1b2382.png)

9. 將```listen-on port 53{ 127.0.0.1; };```改成```listen-on port 53{ any; };```，將``` allow-query { localhost; };```改成```allow-query { any; };```
10. 重啟```systemctl restart named```
11. 管理網域```gedit /etc/named.rfc1912.zones```內容如下
```
zone "a.com" IN {
	type master;
	file "a.com.zone";
	allow-update { none; };
};
```
12. ```cd /var/named``` ```gedit a.com.zone```
```
$TTL 600 ;10 minutes

@ IN SOA       @ user.gmail.com (
               2021031803 ;serial
               10800      ;refresh
               900        ;retry
               604800     ;epxire
               86400      ;minimux
               )
@              NS    dns1.a.com.
dns.com        A     192.168.56.101
dns1           A     192.168.56.101
www            A     192.168.56.150
eshop          CNAME www
ftp            A     192.168.56.150
abc            A     192.168.56.120
```
13. 重啟```systemctl restart named```
14. ```host -t ns a.com 127.0.0.1```

![image](https://user-images.githubusercontent.com/62127656/147878854-b416a4cd-b73a-4ae6-af7d-360fe43d36e8.png)

15. 修改檔案```gedit /etc/named.rfc1912.zones```在最底下加入以下
```
zone "56.168.192.in-addr.arpa" IN {
	type master;
	file "56.168.192.in-addr.arpa.zone";
	allow-update { none; };
};
```
16. 新增檔案```cd /var/named``` ```gedit 56.168.192.in-addr.arpa.zone```內容如下
```
$TTL 600 ;10 minutes

@ IN SOA       @ user.gmail.com (
               2021031803 ;serial
               10800      ;refresh
               900        ;retry
               604800     ;epxire
               86400      ;minimux
               )
56.168.192.in-addr.arpa.   IN  NS dns1.a.com.
200.56.168.192.in-addr.arpa. IN PTR www.a.com.
150.56.168.192.in-addr.arpa. IN PTR ftp.a.com.
```
17.重啟```systemctl restart named```
18.查看```nslookup 192.168.56.150 127.0.0.1```

![image](https://user-images.githubusercontent.com/62127656/147879030-4ac89bfc-2ec5-45a8-b5d1-433d36caca8c.png)


## telnetd
1. 安裝telnet``` yum install telnet``` ```yum install telnet-server```
2. 檢查是否安裝xineted```rpm -qa | grep xinetd``` ```yum install -y xinetd```
3. 建立telnet設定檔```gedit /etc/xinetd.d/telnet```
```
service telnet

{
 flags = REUSE
 socket_type = stream
 wait = no
 user = root
 server = /usr/sbin/in.telnetd
 log_on_failure += USERID
 disable = no
}
```
4. 查看設定檔```cat echo-dgram```
5. 開啟&查看```systemctl start xinetd``` ```systemctl status xinetd``` ```netstat -tunlp | grep 23```
6. 利用pytty的telnet去登入

![image](https://user-images.githubusercontent.com/62127656/147878022-e60c1c41-e9ef-40ec-bfb7-3d282a0e4d05.png)
![image](https://user-images.githubusercontent.com/62127656/147878009-6618f0b4-673b-4c06-9b18-dc2068de322a.png)


## DHCP & NAT
* 主機 副機(需新增名為a的網路卡，選擇內部網路)
1. 副機: 關閉其他網路，只留名為a的網路卡
2. 主機: ```ifconfig``` ```ip addr add 192.168.10.1/24 brd + dev enp0s9``` 
3. 主機: ```ifconfig enp0s9 ``` 可得192.168.10.1的網路ip
4. 副機: 關閉網路管理員並清除ip```systemctl stop NetworkManager``` ```ifconfig enp0s3 0```
5. 主機: ```yum install dhcp``` ```gedit /etc/dhcp/dhcpd.conf```
```
subnet 192.168.10.0 netmask 255.255.255.0{
   range 192.168.10.100 192.168.10.200;
   option routers 192.167.10.1;
   option domain-name-servers 8.8.8.8;
   default-lease-time 600;
   max-lease-time 7200;
}
```
6. 重啟```systemctl restart dhcpd.service``` 
7. 出錯的話做這步，不然就跳過```ip addr add 192.168.10.1/24 brd + dev enp0s9``` ```systemctl start dhcpd.service```
8. 
