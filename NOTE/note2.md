# week2
## rpm
* RPM套件管理員（簡稱RPM，全稱為The RPM Package Manager）是在Linux下廣泛使用的軟體套件管理器
* 也可能是指.rpm的檔案格式的軟體包
 
![image](https://user-images.githubusercontent.com/62127656/134854003-a727e846-8c51-4b36-8064-741c609f4d5e.png)
> 包名、版本資訊、版本號、執行平台

## linux系統下安裝軟體一般有3種方法：①rpm工具    ②yum工具    ③原始碼包安裝
* 在Linux下安裝軟體包，主要有3種辦法
  1. rpm工具（redhat package manager，手動安裝，難點在於包的依賴關係 rpm包類似於windows下的.exe檔案，安裝路徑和檔名基本都是固定的。 rpm -ivh [rpm完整包名] 

  2. yum工具（python開發出來的工具，操作物件rpm包，能自動解決軟體包的依賴關係，是最常用的方式） yum install -y 【包名簡稱】

  3. 原始碼包（需要通過編譯器把該原始碼包編譯成可執行的檔案）【安裝難度大】 ./configure---->make---->make install
## 參考資料
* [Linux】安裝軟體的三種方式--rpm、yum、原始碼包](https://www.itread01.com/content/1542075865.html)
