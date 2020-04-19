# linux tlp tutorial

TLP - Linux Advanced Power Management

[Youtube Tutorial - tlp tutorial](https://youtu.be/rQmbnXheSVw)

TLP 是一款進階電源管理的工具, 如果沒特別需求, 你甚至可以在安裝完它之後,

直接忘記他的存在, 然後你會發現你的電池續航力提昇了不少:satisfied:

但有幾個地方要注意:exclamation::exclamation:

首先是沒有 GUI 界面可以調整, 再來是筆電比較適合使用:smile:

安裝文件可參考 [tlp-installation](https://linrunner.de/en/tlp/docs/tlp-linux-advanced-power-management.html#installation),

Ubuntu 安裝方法(其他 distro 請參考上方連結),

先加入 Package Repository (建議執行此步驟, 確保安裝版本是最新的)

```cmd
sudo add-apt-repository ppa:linrunner/tlp
sudo apt update
```

安裝 packages

tlp (PPA or universe) – Power saving

tlp-rdw (PPA or universe) – optional – Radio Device Wizard

```cmd
sudo apt install tlp tlp-rdw
```

ThinkPads only

acpi-call-dkms (universe) – optional

external kernel module providing battery charge thresholds and recalibration for newer ThinkPads (X220/T420 and later)

```cmd
sudo apt install acpi-call-dkms
```

tp-smapi-dkms (PPA or universe) – optional

external kernel module providing battery charge thresholds, recalibration and specific status output in tlp-stat for older ThinkPads

```cmd
sudo apt install tp-smapi-dkms
```

如果需要查看硬碟健康檢查資料, 請安裝 smartmontools

```cmd
sudo apt-get install smartmontools
```

TLP 1.3 and higher 設定檔在以下的路徑, 官方文件 [tlp-configuration](https://linrunner.de/en/tlp/docs/tlp-configuration.html) (包含參數的說明)

```cmd
sudo cat /etc/tlp.conf
```

附上我自己的設定檔 [/etc/tlp.conf](https://github.com/twtrubiks/linux-note/blob/master/linux-tlp-tutorial/tlp.conf):smile:

查看全部狀態指令

```cmd
sudo tlp-stat
```

查看 `tlp-stat` 有哪些參數可以使用

```cmd
sudo tlp-stat --help
```

![alt tag](https://i.imgur.com/k7KK9TM.png)

查看電池資訊

```cmd
sudo tlp-stat -b
```

![alt tag](https://i.imgur.com/49v0PAJ.png)

查看硬碟資訊

```cmd
sudo sudo tlp-stat -d
```

![alt tag](https://i.imgur.com/elwKj0w.png)

注意, 如果你這邊沒有資訊,代表硬碟的 id 錯了,

請使用以下指令查詢正確的硬碟 id

```cmd
sudo tlp diskid
```

![alt tag](https://i.imgur.com/spyZxVE.png)

以這邊舉例, 所以要把 [/etc/tlp.conf](https://github.com/twtrubiks/linux-note/blob/master/linux-tlp-tutorial/tlp.conf) 改成 `DISK_DEVICES="nvme0n1"`


查看系統資訊

```cmd
sudo tlp-stat -s
```

![alt tag](https://i.imgur.com/8JHq2Ok.png)

查看溫度和風扇資訊

```cmd
sudo tlp-stat -t
```

![alt tag](https://i.imgur.com/2e6vMni.png)

追蹤模式, 查看 tlp 執行 log

```cmd
sudo tlp-stat -T
```

![alt tag](https://i.imgur.com/zEC0uwY.png)

查看設定檔

```cmd
sudo tlp-stat -c
```

![alt tag](https://i.imgur.com/edmbItx.png)

查看 PCI 裝置

```cmd
sudo tlp-stat -e
```

查看顯卡資訊

```cmd
sudo tlp-stat -g
```

查看 USB 資料
```cmd
sudo tlp-stat -u
```

查看警告資訊

```cmd
sudo tlp-stat -w
```