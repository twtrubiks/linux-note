# Linux Ubuntu 教學 - scrcpy 使用電腦控制(投影)手機

[Youtube Tutorial - Linux Ubuntu 教學 - scrcpy 使用電腦控制(投影)手機](https://youtu.be/4mwrO3p1xHk)

使用電腦控制手機 [scrcpy](https://github.com/Genymobile/scrcpy)

## 安裝教學

這邊使用 ubuntu 18.04 示範,

如果你的 ubuntu 是 20.04, 可以使用簡單

```cmd
apt install scrcpy
```

又或是使用 `snap` 無腦安裝.

( 但是我不喜歡 snap, 所以剩下自己 build 這條路 )

建議可以搭配 [build the app manually](https://github.com/Genymobile/scrcpy/blob/master/BUILD.md) 觀看.


Debian/Ubuntu - runtime dependencies

```cmd
sudo apt install ffmpeg libsdl2-2.0-0 adb
```

client build dependencies

```cmd
sudo apt install gcc git pkg-config meson ninja-build \
                 libavcodec-dev libavformat-dev libavutil-dev \
                 libsdl2-dev
```

server build dependencies

```cmd
sudo apt install openjdk-8-jdk
```

然後等下你執行以下的指令一定會出現 `meson` 版本太低,

所以請再執行以下的指令,

```cmd
sudo apt install python3-pip
pip3 install meson
```

這邊提醒一下, 使用 pip3 安裝的路徑在 `~/.local/bin/meson`.

為了保證版本的正確, 請直接到 [scrcpy/releases](https://github.com/Genymobile/scrcpy/releases) 這邊下載最新的版本.

需要下載兩個東西, 一個是 Source code, 另一個是 scrcpy-server-vx.xx,

將 Source code 解壓縮後將資料夾名稱改為 `scrcpy`, 然後將 scrcpy-server-vx.xx

檔名改為 `scrcpy-server`, 並將它丟到 `scrcpy` 資料夾中

(要改檔名的原因是避免編譯錯誤)

接下來 cd 進 scrcpy 裡面,

build

```cmd
~/.local/bin/meson x --buildtype release --strip -Db_lto=true -Dprebuilt_server=scrcpy-server
```

( ~/.local/bin/meson 建議改成絕對路徑 )

以下指令很容易錯誤 (記得一定要按照我前面說的去做, 不然很容易失敗)

```cmd
ninja -Cx
```

如果到這邊都沒錯誤, 再執行最後一個指令

```cmd
sudo ninja -Cx install
```

大功告成.

要使用 scrcpy 前,

建議先用 `adb devices` 確認是否連接手機,

注意, 手機請打開 debug,

![alt tag](https://i.imgur.com/gZxctGj.jpg)

直接執行 `scrcpy` 即可

![alt tag](https://i.imgur.com/Ed7ga1a.png)
