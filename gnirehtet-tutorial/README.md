# gnirehtet

透過電腦讓手機上網 [gnirehtet](https://github.com/Genymobile/gnirehtet)

## 教學

電腦需要 adb, 如果你是 Debian-based distros, 建議直接安裝 package android-tools-adb

```cmd
sudo apt-get install android-tools-adb
```

然後下載　Linux: gnirehtet-rust-xxx.zip

解壓縮, 切到目錄底下, 直接執行以下的指令

注意, 手機請打開 debug

![alt tag](https://i.imgur.com/gZxctGj.jpg)

可以先使用 `adb devices` 確認是否手機有連上.

確認連上之後, 使用以下指令啟動,

```cmd
./gnirehtet run
```

這時候你就會發現順利使用電腦的網路讓手機上網了

![alt tag](https://i.imgur.com/nrpmNo0.png)
