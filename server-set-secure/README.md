# server 相關安全設定

這邊參考 linode 的 [Guides - Setting Up and Securing a Compute Instance](https://www.linode.com/docs/products/compute/compute-instances/guides/set-up-and-secure/)

## 設定

```cmd
sudo vim /etc/ssh/sshd_config
```

相關設定如下

```config
Port 8056
PermitRootLogin no
PasswordAuthentication no
```

`Port 8056` 預設為 22, 這邊改成一個自己喜歡的, 降低被攻擊的機會.

`PermitRootLogin no` 關閉 root 登入.

`PasswordAuthentication no` 把輸入密碼的登入方式關閉.

設定完要執行以下指令才會生效

```cmd
sudo service ssh restart
```

## 建立新的 user

剛剛上面我們已經把 root user 關閉了,

所以這邊我們要自己再建立一個 user

新增一個 user

```cmd
adduser twtrubiks
```

將這個 user 加入管理員群組

```cmd
usermod -a -G sudo twtrubiks
```

如果今天你還想要修改密碼, 可使用以下的指令

```cmd
passwd twtrubiks
```

之後的登入就會變成

```cmd
ssh twtrubiks@xx.xx.xxx.xx -p 8056
```

## 修改 host name

```cmd
sudo vim /etc/hostname
```

直接把 hostname 改成你想要的名字即可.

```cmd
sudo vim /etc/hosts
```

設定類似如下

```txt
127.0.0.1       localhost
127.0.1.1      twtrubiks
```

這邊要重開機才會生效

## 其他

在 linode 的 Network 中有一個 Reverse DNS,

可以把這邊改成和你的網址一樣, 修改後大約 4~24 小時內會生效.

(可用 ping 網址 檢查是否生效)

然後如果使用 docker, 之後去設定了 linode 的防火牆,

你的 docker 容器要重啟, 這樣防火牆才會生效.
