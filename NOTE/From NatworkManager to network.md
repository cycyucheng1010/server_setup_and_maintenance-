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
