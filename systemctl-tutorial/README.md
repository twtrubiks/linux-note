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

## Donation

文章都是我自己研究內化後原創，如果有幫助到您，也想鼓勵我的話，歡迎請我喝一杯咖啡:laughing:

綠界科技ECPAY ( 不需註冊會員 )

![alt tag](https://payment.ecpay.com.tw/Upload/QRCode/201906/QRCode_672351b8-5ab3-42dd-9c7c-c24c3e6a10a0.png)

[贊助者付款](http://bit.ly/2F7Jrha)

歐付寶 ( 需註冊會員 )

![alt tag](https://i.imgur.com/LRct9xa.png)

[贊助者付款](https://payment.opay.tw/Broadcaster/Donate/9E47FDEF85ABE383A0F5FC6A218606F8)
