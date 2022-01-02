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
