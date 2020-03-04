# 在 Linux 中設定 VirtualBox 把玩 ssh

[Youtube Tutorial - 在 Linux 中設定 VirtualBox 把玩 ssh](https://youtu.be/Tq_iWo1IIkg)

## 教學

在開始介紹前, 請先確認有安裝 openssh-server

```cmd
sudo apt-get install openssh-server
```

安裝完後建議使用 `ssh localhsot` 測試, 如果可以連上代表正確安裝

![alt tag](https://i.imgur.com/nYo5NNn.png)

## 設定

先找到你要連的 VM -> Network 中的 Attached to 設定為 `Bridged Adapter`

![alt tag](https://i.imgur.com/FPLJtfS.png)

(如果主機是 win, 方法不一樣！！ 會比較麻煩, 要設定成 NAT 以及設定 Port Forwarding)

接著到 VM 中查看 ip

>> ip addr show

![alt tag](https://i.imgur.com/Xgu6SoD.png)

VM 中的 ip 是 `192.168.1.106`

回到本機輸入 `ssh twtrubiks@192.168.1.106`

![alt tag](https://i.imgur.com/NGJCIjo.png)

成功連接到 VM 中的機器了

![alt tag](https://i.imgur.com/E2OYjLq.png)
