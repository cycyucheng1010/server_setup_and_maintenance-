# week2
## rpm
* RPM套件管理員（簡稱RPM，全稱為The RPM Package Manager）是在Linux下廣泛使用的軟體套件管理器
* 也可能是指.rpm的檔案格式的軟體包
 
![image](https://user-images.githubusercontent.com/62127656/134854003-a727e846-8c51-4b36-8064-741c609f4d5e.png)
> 包名、版本資訊、版本號、執行平台

![image](https://user-images.githubusercontent.com/62127656/134855810-e30f5c7d-3884-4027-aee9-11b77b9f8da8.png)
![image](https://user-images.githubusercontent.com/62127656/134857925-f6290964-c378-4d09-990a-62e0603acc7b.png)
![image](https://user-images.githubusercontent.com/62127656/134858133-a51dee45-2574-43d3-8211-6bbfa439f22a.png)

## linux系統下安裝軟體一般有3種方法：①rpm工具    ②yum工具    ③原始碼包安裝
* 在Linux下安裝軟體包，主要有3種辦法
  1. rpm工具（redhat package manager，手動安裝，難點在於包的依賴關係 rpm包類似於windows下的.exe檔案，安裝路徑和檔名基本都是固定的。 rpm -ivh [rpm完整包名] 

  2. yum工具（python開發出來的工具，操作物件rpm包，能自動解決軟體包的依賴關係，是最常用的方式） yum install -y 【包名簡稱】

  3. 原始碼包（需要通過編譯器把該原始碼包編譯成可執行的檔案）【安裝難度大】 ./configure---->make---->make install
## htop
![image](https://user-images.githubusercontent.com/62127656/134865765-521651a2-7518-4c29-9b91-81607ee8f029.png)
```
102  wget https://src.fedoraproject.org/lookaside/extras/htop/htop-2.2.0.tar.gz/sha512/ec1335bf0e3e0387e5e50acbc508d0effad19c4bc1ac312419dc97b82901f4819600d6f87a91668f39d429536d17304d4b14634426a06bec2ecd09df24adc62e/htop-2.2.0.tar.gz
  103  tar htop-2.2.0.tar.gz
  104  tar xvfz htop-2.2.0.tar.gz
  105  ./configure
  106  yum -y install gcc ncurses-devel
  107  sudo yum -y install gcc ncurses-devel
  108  ./configure
  109  ls
  110  cd htop-2.2.0 
  111  ls
  112  ./configure
  114  make
  115  make install
  116  sudo make install
  117  htop
```
## 參考資料
* [Linux】安裝軟體的三種方式--rpm、yum、原始碼包](https://www.itread01.com/content/1542075865.html)
* [rpm指令](https://mitblog.pixnet.net/blog/post/31457516)
