[English Version](README_en.md)

# 修正 ubuntu 18.04 網路連線設定遺失問題

一般來說, 下圖紅色的地方是可以設定網路線連接的狀態,

![alt tag](https://i.imgur.com/3wLYDUA.png)

不過有一次紅色框起來這塊自己消失了, 找了很久, 似乎是 bug,

這邊紀錄一下, 只需要在底下的路徑建立一個檔名為

`10-globally-managed-devices.conf` 的空檔案即可修正.

```cmd
sudo touch /etc/NetworkManager/conf.d/10-globally-managed-devices.conf
```

建議再重開 NetworkManager, 更好的方法式重開機(可做可不做 :relaxed:)

```cmd
sudo systemctl restart NetworkManager
```

相關討論以及說明可參考

[https://bugs.launchpad.net/ubuntu/+source/network-manager/+bug/1638842](https://bugs.launchpad.net/ubuntu/+source/network-manager/+bug/1638842)
