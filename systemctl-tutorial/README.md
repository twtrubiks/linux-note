# systemctl

systemctl 命令是系統服務管理指令，

假設 Linux 上有安裝 postgresql，

啟動服務 start service

```cmd
sudo systemctl start postgresql
```

or

```cmd
sudo systemctl start postgresql.service
```

結尾要不要加入 `.service` 都可以(真實檔案其實結尾是有 `.service`)

停止服務 stop service

```cmd
sudo systemctl stop postgresql
```

重啟服務 restart service

```cmd
sudo systemctl restart postgresql
```

查看當前服務狀態 show status of service

```cmd
sudo systemctl status postgresql
```

目前為沒有啟動

![alt tag](https://i.imgur.com/IjtRINb.png)

查看服務設定檔

```cmd
sudo systemctl cat postgresql
```

如下輸出, 第一行就是指你的設定檔路徑

```text
# /lib/systemd/system/postgresql.service
# postgresql.service is the meta unit for managing all PostgreSQL clusters on
# the system at once. Conceptually, this unit is more like a systemd target,
# but we are using a service since targets cannot be reloaded.
#
# The unit actually managing PostgreSQL clusters is postgresql@.service,
# instantiated as postgresql@15-main.service for individual clusters.

[Unit]
Description=PostgreSQL RDBMS

[Service]
Type=oneshot
ExecStart=/bin/true
ExecReload=/bin/true
RemainAfterExit=on

[Install]
WantedBy=multi-user.target
```

查看更詳細的服務設定檔

```cmd
sudo systemctl show postgresql
```

啟動開機自動啟動 enable service ( auto-start )

```cmd
sudo systemctl enable postgresql
```

停止開機自動啟動 disable service ( not auto-start )

```cmd
sudo systemctl disable postgresql
```

查看 postgresql 是否 active

```cmd
sudo systemctl is-active postgresql
```

查看 postgresql 是否有設定自動啟動

```cmd
sudo systemctl is-enabled postgresql
```

查看 postgresql 是否啟動失敗

```cmd
sudo systemctl is-failed postgresql
```

查看全部已啟動的服務

```cmd
sudo systemctl list-units
```

查看全部已啟動的 service 服務

```cmd
sudo systemctl list-units --type=service
```

查看全部服務狀態

```cmd
sudo systemctl list-unit-files --type service -all
```

編輯服務設定,

通常我是直接進入 `/etc/systemd/system` 修改,

但是也可以透過以下的方式修改

```cmd
sudo systemctl edit --full postgresql
```

預設是 nano EDITOR, 可改成自己喜歡的 EDITOR,

這邊用 vim 做示範, 輸入以下指令

```cmd
export SYSTEMD_EDITOR=vim
```

再執行以下指令

```cmd
sudo visudo
```

之後會跳出編輯介面, 在裡面加上一行,

```text
Defaults  env_keep += "SYSTEMD_EDITOR"
```

這樣預設就是用 vim 打開了 :smile:

重新載入 systemd

```cmd
sudo systemctl daemon-reload
```

實際應用可參考 [在 Linux 中自動啟動 docker](https://github.com/twtrubiks/docker-tutorial/tree/master/docker-auto-run-linux)