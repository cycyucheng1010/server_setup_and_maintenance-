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
