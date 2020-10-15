# -
如何讓Windows 10遠端Linux
利用SSH安全加密連線遠端登錄Linux，以下用Ubuntu 18.04 伺服器版做說明

先到Ubuntu的系統

如果是使用Ubuntu桌面版，需開啟終端機(terminal)以輸入指令
如果是使用Ubuntu伺服器版則直接輸指令即可

透過以下指令先確認ubuntu是否安裝了SSH，可以看到系統預設裝了openssh-client

dpkg -l | grep ssh

![image](https://github.com/cloudgowatermirror/-/blob/master/01-1.jpg)


再來，為了其他電腦連線到你的Ubuntu，必須透過以下指令安裝openssh-server

sudo apt-get install openssh-server

如果是為了讓Ubuntu連線至其他電腦，則透過以下指令安裝openssh-client(通常系統都預設安裝)

sudo apt-get install openssh-client

都裝好了之後在用指令確認一下，會發現多了一個openssh-server

dpkg -l | grep ssh

![image](https://github.com/cloudgowatermirror/-/blob/master/03.jpg)


再來透過以下指令確認openssh-server是否有啟動，如果看到sshd表示已經啟動

ps -e | grep ssh

![image](https://github.com/cloudgowatermirror/-/blob/master/04.jpg)

如果發現沒有啟動則輸入以下指令啟動

sudo /etc/init.d/ssh start
或
sudo service ssh start

然後輸入ifconfig確認Ubuntu的IP

再來到Windows 10並開啟命令提示字元(cmd.exe)，然後輸入ssh Ubuntu使用者名稱@Ubuntu的IP

如果發現無法練線，則先用ping嘗試看看

如果ping顯示以下訊息，可能是IP分配錯誤或是防火牆出問題
ssh: connect to host master port 22: Connection timed out

如果ping顯示以下訊息，可能路由沒設定或設定跑掉
connect: Network is unreachable

可透過以下指令應急
route add default gw 192.168.1.xxx

或是到interfaces此設定檔修改，先用以下指令到根目錄 
cd /

再用以下指令用vim編輯器開啟interfaces修改設定
sudo vim /etc/network/interfaces

若找不到檔案則用下列方式一層層進入並用ls找看看檔案，確認有檔案在用sudo vim的指令
cd etc
ls
cd network
ls
sudo vim interfaces

進入設定檔後，裡面可能都是空白的也可能是有東西的，但不論如何就改成以下設定，怕誤刪就在該行前面加#註解即可

先按i進入編輯模式，然後輸入下面的東西
auto eth0 #開機自動連線
#iface eth0 inet dhcp
iface eth0 inet static
address 192.168.1.14
netmask 255.255.255.0
gateway 192.168.1.1
設定完按 shift+: 然後輸入wq就會存檔並退出設定檔
iface eth0 inet static #static表示使用固定ip，dhcp表示使用動態ip

再來到resolv.conf此設定檔修改，先用以下指令到根目錄 
cd /

再用以下指令用vim編輯器開啟resolv.conf修改設定
sudo vim /etc/resolv.conf

若找不到檔案則用下列方式一層層進入並用ls找看看檔案，確認有檔案在用sudo vim的指令
cd /
ls
cd etc
ls
sudo vim resolv.conf

進入後改成下面設定
nameserver 8.8.8.8
設定完按 shift+: 然後輸入wq就會存檔並退出設定檔

然後輸入以下指令
resolvconf -u

如果出現下面訊息
Command 'resolvconf' not found, but can be installed with:
sudo apt install openresolv
sudo apt install resolvconf

就輸入下面指令把東西裝一裝

IP分配錯誤可能是/etc/hosts檔案中的IP位址對應有問題，要檢查名稱和IP地址的對應關係

如果檢查完發現IP分配沒問題，就用以下指令看Port是不是被防火牆鎖住了
sudo service iptables status

如果顯示已下錯誤訊息
Unit iptables.service could not be found.


未完待續....
