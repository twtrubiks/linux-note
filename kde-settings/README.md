# KDE setting

KDE 客製化功能比較多, 所以這篇文章主要是紀錄一些功能.

### 重新啟動 KDE 桌面

KDE 版本 > 5.10

```cmd
kquitapp5 plasmashell
kstart5 plasmashell
```

### fcitx 新酷音輸入法

安裝方法

```cmd
sudo apt-get install fcitx fcitx-chewing
```

如果遇到打字太快整句會消失的 bug, 請嘗試以下方法

```cmd
vim ~/.xsessionrc

export GTK_IM_MODULE=fcitx
export XMODIFIERS=@im=fcitx
export QT_IM_MODULE=fcitx
```

請重開機.

但重開機後輸入法不會自動啟動, 所以要另外設定開機自動執行

```cmd
fcitx-autostart
```

這樣打字在怎麼快, 發生整句消失的 bug 就很少出現了.

### 無法啟動 VirtualBox

請參考 [How to sign a kernel module Ubuntu 18.04](https://superuser.com/questions/1438279/how-to-sign-a-kernel-module-ubuntu-18-04)

另外注意輸入密碼時, 請設定全部數字就好, 因為這樣在 bios 的介面上會比較方便.

(bios 介面上似乎對非數字無效)

### KDE 修改 DNS server

![alt tag](https://i.imgur.com/t4Mc8p6.png)

### KDE 設定開機自動啟動

![alt tag](https://i.imgur.com/O8CiLtD.png)

### KDE 設定快捷建移動視窗位置

![alt tag](https://i.imgur.com/GalKDFk.png)

### KDE 設定快捷建切換多桌面

![alt tag](https://i.imgur.com/N1u6SWv.png)

### KDE 設定快捷建將視窗切換到其他的桌面

![alt tag](https://i.imgur.com/Vnxvhdq.png)

### KDE 設定快捷建打開特定軟體

設定快捷建打開 shutter

![alt tag](https://i.imgur.com/bKXn2xX.png)

![alt tag](https://i.imgur.com/yc0en8v.png)

設定快捷建打開 konsole

![alt tag](https://i.imgur.com/Z5RRFNl.png)

![alt tag](https://i.imgur.com/575D0aA.png)
