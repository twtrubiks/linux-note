# 在 Linux 中設定 VirtualBox - 常見問題

在 Linux 中玩 VirtualBox 常常會遇到一些問題, 這邊文章紀錄一下

## 常見問題

### 在 Linux 中設定 VirtualBox 把玩 ssh

[linux-virtualbox-ssh-tutorial](https://github.com/twtrubiks/linux-note/tree/master/linux-virtualbox-ssh-tutorial) - 在 Linux 中設定 VirtualBox 把玩 ssh

### 為甚麼在使用 VirtualBox 會很慢:question:

除了硬碟請使用 SSD 之外, 想請將 Enable 3D Acceleration 打勾

![alt tag](https://i.imgur.com/xuQ0q8E.png)

為甚麼在 VirtualBox 內會找不到外接 USB 裝置:question:

這問題通常是 groups 的問題, 因為 groups 中沒有 vboxusers 這個 group,

所以導致無法取得 USB 裝置,

在 terminal 上輸入以下的指令

`sudo usermod -a -G vboxusers username`

建議登出再登入或重新開機,

然後在 terminal 上輸入 groups, 你就可以看到 vboxusers 了:smile:

![alt tag](https://i.imgur.com/KIteuAV.png)

### 為甚麼在 VirtualBox 無法 share 資料夾:question:

可能會出現類似的錯誤訊息

```text
You do not have the permissions necessary to view the contents of ....
```

只需要將 shared folders 加入即可

```cmd
sudo adduser [username] vboxsf
```

建議登出再登入或重新開機.

### 建議 VirtualBox 安裝 VBoxGuestAdditions

主要安裝它能帶來一些額外的功能, 調整螢幕, 全屏螢幕, 剪貼簿和主機共享之類的,

如果進入到 VirtualBox 中無法直接安裝,

建議可以直接將 VBoxGuestAdditions.iso 檔案掛上去

![alt tag](https://i.imgur.com/t13LRgu.png)
