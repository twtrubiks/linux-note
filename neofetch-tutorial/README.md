# NeoFetch

[Youtube Tutorial - Linux 指令教學-NeoFetch](https://youtu.be/nuobFAxCaw8)

Fastfetch is a neofetch-like tool for fetching system information and displaying it in a visually appealing way. It is written mainly in C, with a focus on performance and customizability. Currently, it supports Linux, macOS, Windows 7+, Android, FreeBSD, OpenBSD, NetBSD, DragonFly, Haiku, and SunOS.

因為 [neofetch](https://github.com/dylanaraps/neofetch) 不維護了, 改使用 [fastfetch](https://github.com/fastfetch-cli/fastfetch) 使用方式基本上都是一樣的.

## 教學

安裝指令

```cmd
sudo apt update
sudo apt install fastfetch
```

如果是其他 distro, 請直接到 Releases 下載 `.deb` 安裝.

使用方法很簡單,

```cmd
fastfetch
```

![alt tag](https://i.imgur.com/UaIoTuV.png)

查看板本

```cmd
fastfetch -v
```

![alt tag](https://i.imgur.com/syS0jpk.png)

查看指令說明

```cmd
fastfetch --help
```

![alt tag](https://i.imgur.com/4fnJwHj.png)

## 打開 terminal 時自動執行

依照自己的 terminal 設定 ( 這邊使用 `zsh` 示範 )

```cmd
vim ~/.zshrc
```

在裡面加入 fastfetch (最前面或最後面都可以)

![alt tag](https://i.imgur.com/7pbqz1P.png)

這樣每次打開 terminal 時就會自動執行 `fastfetch` 了 :smile:
