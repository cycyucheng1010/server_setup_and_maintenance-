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
