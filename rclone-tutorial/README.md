[English Version](README_en.md)

# Rclone tutorial

[Youtube Tutorial - Rclone tutorial 超棒雲端同步工具](https://youtu.be/0ChhvaHIQ9Y)

rclone 是一套很棒的文件同步管理工具, 可以和非常多的雲同步, 像是 Dropbox,
Google Drive......

請直接參考官網的說明 [rclone](https://rclone.org/).

## 教學

安裝方法官方文件 [rclone.org-install](https://rclone.org/install/)

因為可能需要一些依賴, 所以建議可以先安裝

```cmd
sudo apt-get update
sudo apt-get install curl unzip
```

安裝 rclone

```cmd
curl https://rclone.org/install.sh | sudo bash
```

確認版本是否成功安裝

```cmd
sudo rclone --version
```

開始設定

```cmd
sudo rclone config
```

![alt tag](https://i.imgur.com/98Cq8UN.png)

我們使用 dropbox 來示範, 編號是 9

![alt tag](https://i.imgur.com/PWMJ5SH.png)

client_id 和 client_secret 這邊直接 enter ( 不填 ),

Edit advanced config 選 n

![alt tag](https://i.imgur.com/WWMPRWu.png)

Auto config 選擇 y,

先著如果沒有自動打開網址, 請自行把他點開

![alt tag](https://i.imgur.com/aBkg1Gx.png)

輸入自己的 dropbox 帳號

![alt tag](https://i.imgur.com/yFGW3Vf.png)

![alt tag](https://i.imgur.com/ekaCLHc.png)

![alt tag](https://i.imgur.com/F3s2Hft.png)

如果資訊都正確, 請選擇 y

![alt tag](https://i.imgur.com/vSpd7B4.png)

可以使用以下指令確認 remotes

```cmd
sudo rclone listremotes
```

注意, 是 `dropbox:` 哦, 有一個 `:`

![alt tag](https://i.imgur.com/V9G8Izm.png)

使用 [rclone_sync](https://rclone.org/commands/rclone_sync/) commands,

本機同步到遠端

```cmd
sudo rclone sync -v ~/linux-note dropbox:temp
```

補充：-v, -vv, --verbose

![alt tag](https://i.imgur.com/CLpsAbv.png)

遠端同步回本機

```cmd
sudo rclone sync -v dropbox:temp ~/test-data
```
![alt tag](https://i.imgur.com/t0fhAF8.png)

顯示 remote 的資料夾

[rclone_lsd](https://rclone.org/commands/rclone_lsd/)

```cmd
sudo rclone lsd dropbox:
```

## 其他的 commands

可參考 [rclone-docs](https://rclone.org/docs/),

顯示 remote 的全部資料

[rclone_ls](https://rclone.org/commands/rclone_ls/)

```cmd
sudo rclone ls dropbox:
```

顯示 remote 的資料夾

[rclone_lsd](https://rclone.org/commands/rclone_lsd/)

```cmd
sudo rclone lsd dropbox:
```

複製檔案, 跳過已經複製的(存在)內容,

[rclone_copy](https://rclone.org/commands/rclone_copy/)

也可以雲端之間的搬動(轉移), 像以下的例子,

```cmd
sudo rclone copy --drive-chunk-size=64M --transfers=2 --fast-list --stats=20s -v remote_1:folder remote_2:folder
```

[rclone_move](https://rclone.org/commands/rclone_move/)

```cmd
sudo rclone move remote_1:folder remote_2:folder
```

sync, copy, move 之間詳細的差異請自行閱讀文件 :smile:

可以再搭配 [crontab-tutorual](https://github.com/twtrubiks/linux-note/tree/master/crontab-tutorual) 解決定時同步的問題 :satisfied:

## 雲端加密

rclone 還有加密的功能, 舉個例子, 假設今天我對 A 的雲端空間沒什麼信心,

我希望我上傳的檔案有被加密過, 這時候可以透過這個選項

![alt tag](https://i.imgur.com/PWYoc8y.png)

然後選擇你建立的 remote, 像是前面我建立的 `dropbox:`, 命名時可以命名為

`dropbox:Encrypt` 之類的, 總之就是要先建立一個 remote, 然後才能建立加密

的. 如果有人對這部份有興趣, 我之後再拍影片教大家 :smile:
