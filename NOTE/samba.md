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
