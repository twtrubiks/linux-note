# exFAT-usb 教學 - 讓usb可以存放大於4GB檔案

因為 FAT32 格式化會有單檔不能超過4GB的限制,

為了解決這個問題, 可以使用 exFAT 格式化, 這樣就可以放單檔案超過 4GB 了,

請先安裝 [exfatprogs](https://github.com/exfatprogs/exfatprogs)

```cmd
sudo apt install exfatprogs
```

先找到自己的 usb

```cmd
sudo fdisk -l
```

![alt tag](https://i.imgur.com/OLQZyuN.png)

假設這邊 usb 是 `/dev/sda1`

卸載 usb 確保沒有被系統佔用

```cmd
sudo umount /dev/sda1
```

格式化

```cmd
sudo mkfs.exfat /dev/sda1
```

透過 `fsck.exfat` 檢查是否成功

```cmd
sudo fsck.exfat /dev/sda1
```