# raspberrypi 教學

本篇文章會紀錄一些 Raspberry Pi OS 設定的東西

## 說明

pi 預設的帳號為 `pi` 密碼為 `raspberry`

### 設定固定 ip ( static ip address)

執行以下指令查看網路

```cmd
ip route | grep default
```

![alt tag](https://i.imgur.com/wux0QsZ.png)

接著設定 dhcp

```cmd
sudo vim /etc/dhcpcd.conf
```

格式如下

```conf
interface <Network>
static ip_address=<Static_IP>/24
static routers=<Router_IP>
```

`interface` 選擇你的網路, 這邊選擇 eth0 (如果是 wifi, 可能是 wlan0)

`static ip_address` 設定你的固定 ip (給 pi 的 ip)

`static routers` 設定你的 router ip

基本上, 要設定的東西在 `ip route | grep default` 指令下都有.

以下為範例

```conf
interface eth0
static ip_address=192.168.7.88/24
static routers=192.168.7.254
```

## 安裝 selenium 的 chromedriver

有時後會透過 pi 寫一些簡單的爬蟲, 這邊紀錄一下如何安裝 selenium 的 chromedriver

```cmd
sudo apt update
sudo apt upgrade -y
sudo apt-get install chromium-chromedriver
```

如果要查安裝的路徑, 可執行以下指令

```cmd
which chromedriver
```