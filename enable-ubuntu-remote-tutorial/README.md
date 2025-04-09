# 如何在 ubuntu 啟用遠端桌面

* [Youtube Tutorial - 如何在 ubuntu 啟用遠端桌面](https://youtu.be/-01unOIk9mI)

* [Youtube Tutorial - 修正 Ubuntu 遠端桌面黑頻 (沒接螢幕)](https://youtu.be/3j4wUMX95zA)

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

* [Youtube Tutorial - 修正 Ubuntu 遠端桌面黑頻 (沒接螢幕)](https://youtu.be/3j4wUMX95zA)

linux 在遠端桌面的時候, server (被遠端的機器) 一定要接上螢幕,

不然你會發現雖然連過去了, 可是畫面動不了(或是黑屏) :sweat:

這邊提供解決方法,

安裝 `xserver-xorg-video-dummy`

```cmd
sudo apt-get install xserver-xorg-video-dummy
```

Ubuntu 18.04 已測試過(請安裝這個)

```cmd
sudo apt-get install xserver-xorg-video-dummy-hwe-18.04
```

也可以透過以下指令去尋找對應的版本

```cmd
sudo apt-cache search video-dummy
```

安裝後請到路徑下建立 `xorg.conf`

```cmd
sudo vim /usr/share/X11/xorg.conf.d/xorg.conf
```

如果你找不到這個路徑, 也可能在 `/etc/X11/xorg.conf`,

[xorg.conf](https://github.com/twtrubiks/linux-note/blob/master/enable-ubuntu-remote-tutorial/xorg.conf) 內容如下,

```conf
Section "Device"
    Identifier  "Configured Video Device"
    Driver      "dummy"
EndSection
Section "Monitor"
    Identifier  "Configured Monitor"
    HorizSync 31.5-48.5
    VertRefresh 50-70
EndSection
Section "Screen"
    Identifier  "Default Screen"
    Monitor     "Configured Monitor"
    Device      "Configured Video Device"
    DefaultDepth 24
    SubSection "Display"
    Depth 24
    Modes "1920x1080"
    EndSubSection
EndSection
```

接著重開機, 之後 server (被遠端的機器) 沒接螢幕遠端進去也不會再黑屏了 :satisfied:

但如果這時候 server (被遠端的機器) 接螢幕開機, 你會發現他螢幕不會顯示

(這是正常的, 因為現在是 dummy 的螢幕 :smile:)

你只要刪除 `xorg.conf`, 再重開機就會正常了.

(需要這個功能的時候, 再把 `xorg.conf` 加回去即可 :smile:)

如果想透過 command 查看 vino 的設定,

```cmd
gsettings list-recursively org.gnome.Vino
```

如果想透過 command 改設定中的參數,

```cmd
gsettings set org.gnome.Vino require-encryption false
```

如果想透過 command 啟動指令

```cmd
export DISPLAY=:0 && /usr/lib/vino/vino-server
```

## 結論

以上是使用 GNOME 桌面, 但我愈來愈喜歡 KDE 的桌面,

如果是 KDE 的桌面, 可以改用 krfb ( Server 端), krdc (Client端),

這是專門給 KDE 用戶使用的, 但是如果你 client 端比較喜歡 Remmina,

在 KDE 上也是可以正常使用的 :smile:
