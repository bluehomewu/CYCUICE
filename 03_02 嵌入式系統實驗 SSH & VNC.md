# 03/02 嵌入式系統實驗 SSH & VNC

###### 2022/03/02 嵌入式系統實驗
###### 本篇會著重在解釋 SSH

開啟 SSH 以及 VNC 功能可以參考這篇文章
[Day3 - 遠端控制樹莓派 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天](https://ithelp.ithome.com.tw/articles/10235452)

SSH
---
SSH 可以想像成連線到 Unix / Linux 系統的 Terminal（純文字模式）


SSH 分為兩種登入方式：
- 帳號密碼登入
帳號密碼登入相對簡單，但是不夠安全


- 帳號以及公私鑰登入
公私鑰登入相對複雜，但具有一定的安全性
私鑰可以分別想像成
私鑰即為鑰匙 ( id_rsa )
公鑰即為鎖頭 ( id_rsa.pub )
鑰匙與鎖頭是具有唯一性，一把鑰匙僅能開一個鎖
公鑰必須放置在被連線的主機上面
私鑰則必須放置在連線者的上面



首先，需要將 Raspberry Pi 透過有線網路或是 WiFi 連線到與電腦同一個區域網路

可以在 Raspberry Pi 內終端機輸入 `ifconfig` 或是 `hostname -I`

以 ifconfig 指令為例：
eth0 表示有線網路卡，wlan0 表示無線網路卡
並且可以在對應網卡後面找到 inet 項目，便可以得到 IPV4 的對應 IP

![](https://i.imgur.com/aZpRiDC.png)

接著便可以透過 SSH 指令連線到 Raspberry Pi

`ssh pi@<ip>`
For Example: `ssh pi@192.168.31.3`

接下來 Raspberry Pi 便會要求輸入 pi 帳號的密碼，以便登入

![](https://i.imgur.com/qu3ge88.png)


接著，我們必須回到 Windows / macOS 系統進行 ssh keys 建立

在命令提示字元 CMD 當中輸入 `ssh-keygen`
接下來，系統則會詢問 ssh keys 的存放路徑以及是否對 ssh keys 進行加密
預設將會保存在 `C:\Users\<username>\.ssh\` 資料夾內
預設不會對 ssh keys 進行加密
可以在這步驟連續按下 3 下的 Enter 鍵
便可以完成 SSH keys 的建立

![](https://i.imgur.com/bx1xEPM.png)

在 Windows / macOS 端完成 ssh keys 建立之後，我們需要把公鑰放到 Raspberry Pi 的 `authorized_keys` 檔案內

在 `C:\Users\<username>\.ssh\` 資料夾內以記事本編輯 `id_rsa.pub` 
把公鑰檔案內的所有文字複製起來

![](https://i.imgur.com/DswJJMR.png)

回到 Raspberry Pi 的 ssh 視窗
輸入 `nano .ssh/authorized_keys`

並且貼上公鑰檔案內的所有文字（直接按右鍵就好）

![](https://i.imgur.com/UmHoadN.png)

![](https://i.imgur.com/NaBfr37.png)

然後，依序按下 `Ctrl+O` > `Enter` > `Ctrl+X`

這樣便完成 ssh 公私鑰的環境設置了！

接著，則可以透過以下指令登入 Raspberry Pi
在原本的指令上，加入 -i 參數，可以進行挑選私鑰連線

`ssh -i C:\Users\<username>\.ssh\id_rsa pi@<IP>`
For Example:
`ssh -i C:\Users\EdwardWu\.ssh\id_rsa pi@192.168.31.3`

VNC
---
VNC 可以想像成連線到 Unix / Linux 系統的圖形化介面

推薦使用 [RealVNC](https://www.realvnc.com/en/connect/download/viewer/) 

可依自己的系統挑選對應版本下載

安裝過後，在畫面中輸入 Raspberry Pi 的 IP，並且依序輸入使用者帳號 pi 以及密碼進行登入。
這樣就可以成功透過 VNC 連線上 Raspberry Pi


同場加映
---
- [03/02 嵌入式系統實驗 SSH & VNC](https://hackmd.io/@EdwardWu/SSH_VNCTutorial)
- [03/09 嵌入式系統實驗 Git, GitHub, and GitPi (Part 1)](https://hackmd.io/@EdwardWu/GitTutorial)



###### tags: `嵌入式系統實驗` `Linux` `SSH` `VNC` `SCP` `中原資工`