# SSH Tunneling 教學

## 指令說明

`-N` 代表不執行任何指令.

```text
Do not execute a remote command.
This is useful for just for‐warding ports.
```

`-f` 代表在背景執行.

```text
Requests ssh to go to background just before command execution.
```

`-L` 代表 local port 轉向.

`-R` 代表 remote port 轉向.

詳細的指令說明, 可執行 `man ssh` 觀看.

## Local Port Forwarding

假設有一台 remote server A `root@123.123.123.123`,

在 remote server A 上, port 5601 有一個服務,

為 `http://123.123.123.123:5601`.

在本機 localhost 執行以下指令

```cmd
ssh -N -L 3000:localhost:5601 root@123.123.123.123
```

(執行後就可以不用裡他了)

接著在自己本機瀏覽 `http://127.0.0.1:3000/`,

就等於連接 `http://123.123.123.123:5601`.

因為他將本地的 3000 port 的資料, 都轉到 remote server A 上的 5601 port.

## Remote Port Forwarding

### 情境一

假設有一台 remote server A `root@123.123.123.123`,

公司內網電腦 ip 為 `192.168.7.104`.

如何透過 remote server A 在家(外面)能成功訪問公司內網電腦呢:question:

跳板, 在公司電腦執行,

```cmd
ssh -N -R 8007:192.168.7.104:22 root@123.123.123.123
```

(執行後就可以不用裡他了)

接著當我們在外面(或家裡時),

先 ssh 進入 remote server A `root@123.123.123.123`,

之後輸入以下的指令,

```cmd
ssh admin@localhost -p 8007
```

這邊指定 8007 port, 這個 port 會幫我們進入 `192.168.7.104:22`,

因為 8007 port 的流量都被轉去 `192.168.7.104:22` 了.

這樣就能成功透過 remote server A,

在家(外面)能成功訪問公司內網電腦.

但這邊注意, 還是有可能會有安全上的問題:exclamation::exclamation:

### 情境二

假設有一台 remote server A `root@123.123.123.123`,

並且有一個對外的網址, 假設是 `www.example.com`.

然後我的本機上有一個 8069 port 的服務, `http://127.0.0.1:8069/`.

我想要讓客戶透過 `www.example.com` 這個網址,

連接到我的本機的 8069 port 上的服務 (也就是 `http://127.0.0.1:8069/`).

教學,

首先, 先在你的 remote server A 上安裝 nginx,

並且設定 proxy_pass

```cmd
vim /etc/nginx/sites-available/default
```

內容大約如下,

```text
...

location / {
    proxy_pass http://localhost:8002/;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header Host $host;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    #proxy_set_header X-Forwarded-Proto https;
}
...

```

一些 nginx 指令

```cmd
# 重新啟動 nginx
sudo systemctl restart nginx

# 查看 log
cat /var/log/nginx/access.log
```

然後在本機上執行,

```cmd
ssh -N -R 8002:localhost:8069 root@123.123.123.123
```

訪問 `www.example.com` 等於訪問公司電腦上的 `http://127.0.0.1:8069/`,

這個邏輯剛剛好和 Local Port Forwarding 相反.

8002 port 這個完全就是讓我們去監聽而已,

透過這個 port 去完成兩邊的連接.

## Reference

* [Reverse SSH Tunneling - From Start to End](https://jfrog.com/connect/post/reverse-ssh-tunneling-from-start-to-end/)
* [SSH Tunneling (Port Forwarding) 詳解](https://johnliu55.tw/ssh-tunnel.html)
* [ngrok 不求人：自己搭一個窮人版的 ngrok 服務](https://5xruby.tw/posts/easy-ngrok-by-nginx-ssh-tunnel)