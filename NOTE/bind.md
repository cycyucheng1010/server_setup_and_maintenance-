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

8. ```nslookup www.pchome.com.tw 192.168.56.101```

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
