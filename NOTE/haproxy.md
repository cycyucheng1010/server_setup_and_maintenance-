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
  server server_name1 192.168.56.102:80 check
  server server_name2 192.168.56.104:80 check

```
8. 查看ip 得到結果

![image](https://user-images.githubusercontent.com/62127656/147874344-4198a0cc-d0fe-4f1e-a443-cf7161b06d9f.png)
![image](https://user-images.githubusercontent.com/62127656/147874350-0bd11b43-fedd-4f67-8f04-5b11616cf073.png)
>第一得到centos7-1 再次刷新centos7-2
