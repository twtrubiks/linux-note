# NeoFetch

[Youtube Tutorial - Linux 指令教學-NeoFetch](https://youtu.be/nuobFAxCaw8)

Neofetch is a command-line system information tool written in bash 3.2+. Neofetch displays information about your operating system, software and hardware in an aesthetic and visually pleasing way.

## 教學

github:[neofetch](https://github.com/dylanaraps/neofetch),

在 ubuntu 18.04 下安裝使用以下指令即可, [ubuntu-1704-and-up](https://github.com/dylanaraps/neofetch/wiki/Installation#ubuntu-1704-and-up)

```cmd
sudo apt update
sudo apt install neofetch
```

如果是其他 distro, 請參考 [https://github.com/dylanaraps/neofetch/wiki/Installation](https://github.com/dylanaraps/neofetch/wiki/Installation)

使用方法很簡單,

```cmd
neofetch
```

![alt tag](https://i.imgur.com/UaIoTuV.png)

查看板本

```cmd
neofetch -v
```

![alt tag](https://i.imgur.com/syS0jpk.png)

截圖指令

```cmd
neofetch --scrot
```

查看指令說明

```cmd
neofetch --help
```

![alt tag](https://i.imgur.com/4fnJwHj.png)

## 打開 terminal 時自動執行

依照自己的 terminal 設定 ( 這邊使用 `zsh` 示範 )

```cmd
vim ~/.zshrc
```

在裡面加入 neofetch (最前面或最後面都可以)

![alt tag](https://i.imgur.com/7pbqz1P.png)

這樣每次打開 terminal 時就會自動執行 `neofetch` 了:smile:
