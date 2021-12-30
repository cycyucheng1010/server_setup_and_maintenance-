# server setup and maintenance final term 
## Centos installation
* iso下載 (CentOS-7-x86_64-DVD-2009.iso)

![1](https://github.com/cycyucheng1010/NQU/blob/main/Centos7/week2-1.PNG)
![2](https://github.com/cycyucheng1010/NQU/blob/main/Centos7/week2-2.PNG)
## From NatworkManager to network
## ssh: scp&no password login
### scp
![6](https://github.com/cycyucheng1010/NQU/blob/main/Centos7/week2-6.PNG)
>sudo yum install openssh-server
---
![7](https://github.com/cycyucheng1010/NQU/blob/main/Centos7/week2-7.PNG)
>利用clone後的linux系統進行登入，在此步驟前筆者原無設定RichChen的password會有不知密碼無法登入的問題，設定完後便可解決!
---
![8](https://github.com/cycyucheng1010/NQU/blob/main/Centos7/week2-8.PNG)
>利用本機進行登入
---
![9](https://github.com/cycyucheng1010/NQU/blob/main/Centos7/week2-9.PNG)
>安裝Winscp並登入
---
![10](https://github.com/cycyucheng1010/NQU/blob/main/Centos7/week2-10.PNG)
>將helloworld.txt這個檔案傳到RickChen中
---
![11](https://github.com/cycyucheng1010/NQU/blob/main/Centos7/week2-11.PNG)
>helloworld中的內容
---
![12](https://github.com/cycyucheng1010/NQU/blob/main/Centos7/week2-12.PNG)
>傳送後多了helloworld.txt之檔案，利用cat開啟後可發現內文符合!說明傳送成功且正確

### no pass word login 
## ssh 免密碼登入
![week3-1](https://user-images.githubusercontent.com/62127656/113002220-2668d280-91a4-11eb-9722-6622aec1f1a5.PNG)
![week3-2](https://user-images.githubusercontent.com/62127656/113002238-2b2d8680-91a4-11eb-8a7b-50691312f146.PNG)
![week3-3](https://user-images.githubusercontent.com/62127656/113002252-2d8fe080-91a4-11eb-9335-3df5466b2d05.PNG)
![week3-4](https://user-images.githubusercontent.com/62127656/113002267-308ad100-91a4-11eb-9798-ec2e4f6a3976.PNG)
![week3-5](https://user-images.githubusercontent.com/62127656/113002279-3385c180-91a4-11eb-892c-218c6c5f8fd4.PNG)

## samba
## nfs
## vsftp
## haproxy
## www: php&mysql&virtualhost
## Bind
## tellnetd
