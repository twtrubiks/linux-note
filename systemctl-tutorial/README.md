# systemctl

systemctl 命令是系統服務管理指令，

假設 Linux 上有安裝 postgresql，

啟動服務 start service

```cmd
systemctl start postgresql
```

停止服務 stop service

```cmd
systemctl stop postgresql
```

重啟服務 restart service

```cmd
systemctl restart postgresql
```

查看當前服務狀態 show status of service

```cmd
systemctl status postgresql
```

目前為沒有啟動。

![alt tag](https://i.imgur.com/IjtRINb.png)

啟動開機自動啟動 enable service ( auto-start )

```cmd
systemctl enable postgresql
```

停止開機自動啟動 disable service ( not auto-start )

```cmd
systemctl disable postgresql
```