# fail2ban tutorial

[Youtube Tutorial - fail2ban tutorial](https://youtu.be/5tzPamIzo4o)

透過 fail2ban 讓 server 更安全:smile:

Linux 中, 如果要查看最近 ssh 登入是否有異常

```cmd
cat /var/log/auth.log
```

在這裡可以檢查是否 server 有不正常的連線.

如果你是使用雲端, 像是 azure 或是 aws 之類的雲，應該是不需要再使用 Fail2ban, 因為現在大多的雲端都可以

設定特定的 ip 才可以 ssh (其餘全部封鎖), 但還是有些雲端沒辦法, 像是 Linode 就沒有這個功能:unamused:

所以 Fail2ban 適用在像是 Linode 或是自架的實體機器, 目的是讓機器更安全:smirk:

官方文件, [Fail2ban](https://docs.fusionpbx.com/en/latest/firewall/fail2ban.html#)

安裝指令

```cmd
sudo apt install fail2ban
```

狀態

```cmd
service fail2ban status
```

重啟

```cmd
service fail2ban restart
```

```cmd
fail2ban-client status
```

顯示應該如下，代表 sshd 已經加入監控

```cmd
Status
|- Number of jail:      1
`- Jail list:   sshd
```

查看被 ban 的 ip 有哪些

```cmd
fail2ban-client status sshd
```

```cmd
Status for the jail: sshd
|- Filter
|  |- Currently failed: 0
|  |- Total failed:     9083
|  `- File list:        /var/log/auth.log
`- Actions
   |- Currently banned: 1
   |- Total banned:     3726
   `- Banned IP list:   xxx.xxx.xx.xxx
```

設定檔案如下

( 其實如果只是要測試, 預設的設定就夠了, 不用修改, 頂多把自己的 ip 加入 ignoreip 裡面 )

```cmd
vim /etc/fail2ban/jail.conf
```

```conf
[DEFAULT]
ignoreip = 127.0.0.1/8 xxx.xx.xx # 忽略某些 ip (像是可以設定自己公司的 ip)
bantime  = 10m # 封鎖10分鐘
maxretry = 3 # 登入失敗 3 次
findtime  = 10m # 10分鐘內失敗3次(maxretry)
backend = auto
....
```

`backend` 的部分補充一下, 你可以將它改成 systemd, 這樣重啟的指令就會變成如下

( 我會建議如果在支持 systemd 的系統下, 就修改成 systemd )

```cmd
systemctl status fail2ban  # 狀態
systemctl start fail2ban   # 開始
systemctl restart fail2ban # 重啟
```

查看 `fail2ban.log`

```cmd
tail -f /var/log/fail2ban.log
```

取消被 ban 掉的 IP

```cmd
fail2ban-client set sshd unbanip your-ip
```

如何測試 fail2ban, 首先, 要有兩個不同的網路, 我們先稱為 A,B 網路, 先用 A 網路去連接

你的 server (故意輸錯ip, 達到 maxretry ), 然後使用 `fail2ban-client status sshd`,

你會發現 A 網路的 ip 被加入到 Banned IP list, 如果想要解鎖, 請用 B 網路進去你的 server,

然後執行 `fail2ban-client set sshd unbanip A-ip` 解鎖 :blush:

如果你發現 fail2ban 一直沒有效果, 請重開機解百病:smile: