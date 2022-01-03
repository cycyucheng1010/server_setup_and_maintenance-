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
