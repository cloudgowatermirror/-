# -
Windows 10利用SSH安全加密連線遠端登錄Ubuntu(Linux)，以下用Ubuntu18.04做說明

先到Ubuntu的系統

如果是使用Ubuntu桌面版，需開啟終端機(terminal)以輸入指令
如果是使用Ubuntu伺服器版則直接輸指令即可

透過以下指令先確認ubuntu是否安裝了SSH，可以看到系統預設裝了openssh-client
dpkg -l | grep ssh

再來，為了其他電腦連線到你的Ubuntu，必須透過以下指令安裝openssh-server
sudo apt-get install openssh-server

如果是為了讓Ubuntu連線至其他電腦，則透過以下指令安裝openssh-client(通常系統都預設安裝)
sudo apt-get install openssh-client

都裝好了之後在用指令確認一下，會發現多了一個openssh-server
dpkg -l | grep ssh

再來透過以下指令確認openssh-server是否有啟動
ps -e | grep ssh

如果發現沒有啟動則輸入以下指令啟動
sudo /etc/init.d/ssh start
或
sudo service ssh start

然後輸入ifconfig確認Ubuntu的IP

再來到Windows 10並開啟命令提示字元(cmd.exe)，然後輸入ssh Ubuntu使用者名稱@Ubuntu的IP

如果發現無法練線，則先用ping嘗試看看

未完待續....
