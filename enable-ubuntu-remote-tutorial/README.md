# 如何在 ubuntu 啟用遠端桌面

[Youtube Tutorial - 如何在 ubuntu 啟用遠端桌面](https://youtu.be/-01unOIk9mI)

## 安裝教學

這邊使用 ubuntu 18.04 示範,

這邊使用 GNOME 桌面(大多數預設安裝都是這個), 如果不知道甚麼是 DE,

可參考 [Linux 桌面環境 Desktop Environment](https://github.com/twtrubiks/linux-note/tree/master/linux-de)

請先到以下的地方將它啟用

![alt tag](https://i.imgur.com/QNjAP8e.png)

然後將 Screen Sharing 打開, 設定你的密碼 8 碼,

注意, 如果你安裝 ubuntu minimal, 你會看不到這個選項,

直接執行以下指令安裝就會出現了,

```cmd
sudo apt-get install vino
```

![alt tag](https://i.imgur.com/OjN7Prq.png)

這時候你的 server 端其實已經設定完畢了.

接下來請到你的 client 端 (另一台電腦),

先來安裝 client 端遠端軟體, 我個人是使用 Remmina (ubuntu 預設會幫你裝這套)

![alt tag](https://i.imgur.com/qGYgKGu.png)

連線設定如下圖, 有幾個地方要注意,

Protocol 選 VNC

Server 選 你的 固定ip, 測試時可以使用 `127.0.0.1`

User name 這個名稱不重要, 隨便輸入即可

User password 輸入你剛剛的密碼

![alt tag](https://i.imgur.com/hekY4rz.png)

連線成功

![alt tag](https://i.imgur.com/ptfTAoh.png)

## 其他注意事項

linux 在遠端桌面的時候, server (被遠端的機器) 一定要接上螢幕,

不然你會發現雖然連過去了, 可是畫面動不了, 這個不知道和 grub 有沒有關係,

如果有解法我再教大家:sweat:

以上是使用 GNOME 桌面, 但我愈來愈喜歡 KDE 的桌面,

如果是 KDE 的桌面, 可以改用 krfb ( Server 端), krdc (Client端),

這是專門給 KDE 用戶使用的, 但是如果你 client 端比較喜歡 Remmina,

在 KDE 上也是可以正常使用的:smile:
