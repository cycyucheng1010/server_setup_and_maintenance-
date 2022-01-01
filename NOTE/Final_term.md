# server setup and maintenance final term 
![image](https://user-images.githubusercontent.com/62127656/147848323-079665a4-d060-4c3d-83bb-bcec88e969e9.png)
>網路問題重新連線

## Centos installation
* iso下載 (CentOS-7-x86_64-DVD-2009.iso)

![1](https://github.com/cycyucheng1010/NQU/blob/main/Centos7/week2-1.PNG)
![2](https://github.com/cycyucheng1010/NQU/blob/main/Centos7/week2-2.PNG)
## From NatworkManager to network
```
systemctl stop NetworkManager
systemctl disable NetworkManager
systemctl start network
systemctl status network
```
```ifconfig```
ip 192.168.239.139 netmask 255.255.255.0  broadcast 192.168.239.255
```ip route show```
default via 192.168.239.2 


```ifconfig```記ip netmask

![image](https://user-images.githubusercontent.com/62127656/147729727-4ba7fa26-6874-4856-84a1-eeac1a390cc7.png)

``` ip route show```記內定路由ip

![image](https://user-images.githubusercontent.com/62127656/147729749-bbcb49d0-8d9f-41a9-8c7a-8d019a82bce6.png)

![image](https://user-images.githubusercontent.com/62127656/147735783-da9278d0-ac20-45e2-9ea4-79ea33cfa8d3.png)
* 修改dhcp增加ipaddr,netmask,gatway,:wq!,systemctl restart network

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
## Bind
## tellnetd
