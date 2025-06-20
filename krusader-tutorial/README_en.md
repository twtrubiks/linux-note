[中文版](README.md)

# Krusader Tutorial

[Youtube Tutorial - krusader tutorial](https://youtu.be/u6fVA7ziTjU)

Krusader is a file manager for the KDE desktop, while gnome-commander is a file manager for the GNOME desktop.

To cut to the chase, although I use the GNOME desktop (I'll probably switch to KDE in the future), I still choose Krusader :smile:

## Krusader

The installation command is as follows:

```cmd
sudo apt-get install krusader
```

After installation, you will see this icon:

![alt tag](https://i.imgur.com/v2wIFot.png)

You can open multiple tabs:

![alt tag](https://i.imgur.com/2bZzpvi.png)

Krusader is paired with Konsole. If you want to get 100% out of Krusader, please install Konsole:

```cmd
sudo apt-get install konsole
```

If you have Konsole installed, you can also enable "Show Embedded Terminal":

![alt tag](https://i.imgur.com/vcnQMKY.png)

The terminal and file manager are integrated (as shown in Figure A):

![alt tag](https://i.imgur.com/TKy4znI.png)

If I don't want to use Konsole and want to use my own, but I get this error when I click F9 Term, it's because you haven't installed Konsole:

![alt tag](https://i.imgur.com/d1hgeWx.png)

Please go to the following place to modify it:

Settings -> Configure Krusader...

![alt tag](https://i.imgur.com/tNL4BUH.png)

Find "General" and change the "External Terminal" in the Terminal section to the following:

```cmd
gnome-terminal --working-directory %d
```

![alt tag](https://i.imgur.com/SEPU220.png)

And in "User Actions", change the "External Terminal" in the Terminal section to the following:

```cmd
gnome-terminal --working-directory %d --name %t -e
```

![alt tag](https://i.imgur.com/j7bKk7S.png)

This way, you can use F9 Term to open the terminal.

But note that if you don't use Konsole, you can't enable "Show Embedded Terminal" :worried: (as in Figure A).

Krusader can also compress and decompress:

![alt tag](https://i.imgur.com/HEvvtTw.png)

![alt tag](https://i.imgur.com/SiYzHCy.png)

The rest of the features are left for you to explore on your own :blush:

## gnome-commander

Although gnome-commander is a file manager for the GNOME desktop, I personally think its features and interface are not as good as Krusader's :laughing:

Installation command:

```cmd
sudo apt-get install gnome-commander

![alt tag](https://i.imgur.com/AjTD2jU.png)