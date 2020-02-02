# Crontab

[Youtube Tutorial - Linux 指令教學-Crontab]()

顯示目前排程，

```cmd
crontab -l
```

編輯排程 ( 進入修改 )，

```cmd
crontab -e
```

移除排程，將使用者的 crontab 全部刪除，

```cmd
crontab -r
```

讓我們來嘗試做一次，舉個例子，`hello.py` 如下，

```python
#!/usr/bin/python3
import datetime

with open('dateInfo.txt','a') as outFile:
    outFile.write('\n' + str(datetime.datetime.now()))
```

先執行 `crontab -e`，

這時候它會問你要用什麼編輯器，可以選一個你喜歡的，

之後如果要修改，可以使用 `select-editor` 去修改。

選完後，會跳出一個畫面，

這邊就是要設定你的 crontab，這邊填入

```shell
# 每分鐘執行一次 hello.py
* * * * *  /usr/bin/python3 /home/twtrubiks/hello.py
```

![alt tag](https://i.imgur.com/YAHgSTd.png)

接著啟動服務

```cmd
service cron start
```

接著每分鐘你就會發現 `dateInfo.txt` 有新的資料寫入了。

關閉服務

```cmd
service cron stop
```

重啟服務

```cmd
service cron restart
```

重新載入設定

```cmd
service cron reload
```

查看 crontab 服務的狀態

```cmd
service cron status
```

![alt tag](https://i.imgur.com/yuakltL.png)

設定時間說明，

每分鐘執行一次，

```cmd
* * * * *  /usr/bin/python3  /home/twtrubiks/hello.py
```

每 5 分鐘執行一次，

```cmd
*/5 * * * *  /usr/bin/python3  /home/twtrubiks/hello.py
```

每小時執行一次，

```cmd
* */1 * * *  /usr/bin/python3  /home/twtrubiks/hello.py
```

每天的 1 點執行一次，

```cmd
0 1 * * *  /usr/bin/python3  /home/twtrubiks/hello.py
```

1 至 20 號每天執行一次，

```cmd
0 1 1-20 * *  /usr/bin/python3  /home/twtrubiks/hello.py
```

10 點到 15 點每 5 分鐘執行一次，

```cmd
*/5 10-15 * * *  /usr/bin/python3  /home/twtrubiks/hello.py
```

有時候為了要對 crontab 做確認，也可以將執行完的檔案或 log 寫出去，

```cmd
* * * * *  /usr/bin/python3  /home/twtrubiks/hello.py > /tmp/test_check.txt
```

`*`，代表任何時間。

```cmd
* * * * *
minute hour dayOfMonth month dayOfWeek
```

`,`，代表分隔時段。舉例，`30 10,18 * * *` command，代表早上 10 點半和下午 6 點半都執行 command。

`-`，代表一段時間的範圍。舉例，`15 10-12 * * * command`，代表從 10 點到 12 點的**每個** 15 分都執行 command。

`/n`，n 代表數字，表示每 n 單位間隔。舉例，`*/5 * * * * command`，代表每 5 分鐘執行一次 command。

這邊整理一個表格給大家，

|    *    |    *    |      *     |    *    |                              *                              |
|:-------:|:-------:|:----------:|:-------:|:-----------------------------------------------------------:|
|   分鐘  |   小時  |    日期    |   月份  |                             星期                            |
|  minute |   hour  | dayOfMonth |  month  |                          dayOfWeek                          |
| 0 to 59 | 0 to 59 |   1 to 31  | 1 to 12 | 0 to 7  0 and 7 means Sunday 1 means Monday 2 means Tuesday |

可搭配 [https://crontab.guru/](https://crontab.guru/) 這個網站使用。

其實還有更簡單的方法，直接用 [python-crontab](https://pypi.org/project/python-crontab/) 去控制 crontab。