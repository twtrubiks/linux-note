# krusader tutorial

[Youtube Tutorial - krusader tutorial](https://youtu.be/u6fVA7ziTjU)

krusader 是為 KDE 桌面提供的 file manager, 而 gnome-commander 是為 GNOME 桌面提供的 file manager，

先說結論, 雖然我用 GNOME 桌面( 未來應該會跳 KDE), 但我還是選 krusader :smile:

## krusader

安裝指令如下

```cmd
sudo apt-get install krusader
```

安裝完會看到這個圖式

![alt tag](https://i.imgur.com/v2wIFot.png)

可以開多 tab

![alt tag](https://i.imgur.com/2bZzpvi.png)

krusader 是搭配 konsole, 如果你想要讓 krusader 發揮到 100%, 請安裝 konsole

```cmd
sudo apt-get install konsole
```

如果安裝了 konsole, 還可以啟用 Show Embedde Terminal

![alt tag](https://i.imgur.com/vcnQMKY.png)

terminal 和 file manager 整合在一起了 (如圖 A)

![alt tag](https://i.imgur.com/TKy4znI.png)

如果我不想要使用 konsole, 想要使用自己的, 但點了 F9 Term 出現這個錯誤,

原因是你沒安裝 konsole

![alt tag](https://i.imgur.com/d1hgeWx.png)

請到以下地方修改

Settings -> Configure Krusader...

![alt tag](https://i.imgur.com/tNL4BUH.png)

找到 General, 將 Terminal 裡的 External Terminal 修改成如下

```cmd
gnome-terminal --working-directory %d
```

![alt tag](https://i.imgur.com/SEPU220.png)

以及 User Actions, 將 Terminal 裡的 External Terminal 修改成如下

```cmd
gnome-terminal --working-directory %d --name %t -e
```

![alt tag](https://i.imgur.com/j7bKk7S.png)

這樣就可以使用 F9 Term 打開 terminal 了

但注意, 如果不是用 konsole, 就無法啟用 Show Embedde Terminal :worried: (如圖 A).

krusader 也可以壓縮, 解壓縮

![alt tag](https://i.imgur.com/HEvvtTw.png)

![alt tag](https://i.imgur.com/SiYzHCy.png)

剩下的功能就留給大家自己去研究了 :blush:

## gnome-commander

雖然 gnome-commander 是為 gnome 桌面提供的 file manager,

但我個人覺得它的功能以及介面沒有 krusader 好用 :laughing:

安裝指令

```cmd
sudo apt-get install gnome-commander
```

![alt tag](https://i.imgur.com/AjTD2jU.png)
