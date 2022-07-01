# linux inotify-tools 監控檔案變動 inotifywait

* [Youtube Tutorial - Linux 教學 - inotify-tools 監控檔案變動 inotifywait](https://youtu.be/BsQqH40eOKY)

inotifywait 是 inotify-tools 底下的一個工具,

主要可以用來監控檔案或資料夾的任何變動(新增刪除修改等等),

可以搭配它來完成自動備份上傳或是自動重新載入設定檔之類的.

安裝指令如下

```cmd
sudo apt install inotify-tools
```

如果你想要看 inotifywait 的文件, 可輸入以下指令

```cmd
inotifywait --help
```

或是

```cmd
man inotifywait
```

## 監控特定目錄資料夾下的所有檔案變化

### 監控目錄

```cmd
inotifywait -m /home/temp
```

`-m` `--monitor` 持續監控, 說明如下,

```txt
Keep listening for events forever.  Without
this option, inotifywait will exit after one
event is received.
```

如果資料夾底下還有子目錄, 可以加上 `-r`,

```cmd
inotifywait -m -r /home/temp
```

`-r` `--recursive`	Watch directories recursively.

### 監控檔案

```cnd
inotifywait -m /home/temp/test.py
```

## 自訂輸出訊息格式

可以自己定義想輸出的文字

```cmd
inotifywait -m --timefmt '%Y/%m/%d %H:%M:%S' --format "%T %w %f %e" -e create  /home/temp
```

這邊貼上 `man inotifywait` 中的 `--format` 說明,

```txt
%w     This will be replaced with the name of the Watched file on which
        an event occurred.

%f     When an event occurs within a directory, this will  be  replaced
        with the name of the File which caused the event to occur.  Oth‐
        erwise, this will be replaced with an empty string.

%e     Replaced with the Event(s) which occurred, comma-separated.

%Xe    Replaced with the Event(s) which occurred, separated  by  which‐
        ever character is in the place of `X'.

%T     Replaced  with  the  current Time in the format specified by the
        --timefmt option, which should be a format string  suitable  for
        passing to strftime(3).
```

## 監控檔案事件

監控 modify 以及 delete 事件,

```cnd
inotifywait -m -e modify,delete /home/temp
```

`-e` `--event` 說明如下

```txt
Listen for specific event(s).  If omitted, all events are listened for.
```

事件有很多, 文件如下,

```txt
Events:
	access		file or directory contents were read
	modify		file or directory contents were written
	attrib		file or directory attributes changed
	close_write	file or directory closed, after being opened in
	           	writable mode
	close_nowrite	file or directory closed, after being opened in
	           	read-only mode
	close		file or directory closed, regardless of read/write mode
	open		file or directory opened
	moved_to	file or directory moved to watched directory
	moved_from	file or directory moved from watched directory
	move		file or directory moved to or from watched directory
	create		file or directory created within watched directory
	delete		file or directory deleted within watched directory
	delete_self	file or directory was deleted
	unmount		file system containing file or directory unmounted
```

## 將輸出訊息寫到檔案中

inotifywait 預設會將訊息都輸出在 stdout 上,

但也可以把它輸出到文件中保存下來,

```cnd
inotifywait -m -o logs.txt /home/temp
```

`-o` `--outfile` Print events to file rather than stdout.

`-s` `--syslog` Send errors to syslog rather than stderr.

## 背景執行

inotifywait 也可以在背景執行, 只需要加上 `-d`, 這用法通常會搭配 `-o` 或 `-s` 使用.

```cmd
inotifywait -m -o events.log -d /home/temp/test.py
```

`-d` `--daemon` 說明如下

```txt
Same as --monitor, except run in the background

logging events to a file specified by --outfile.

Implies --syslog.
```

如果執行在背景, 然後想把 inotifywait 清除, 可使用以下指令,

先找出 PID , 然後再 kill 它,

```cmd
ps -ef | grep inotifywait
kill PID
```

或是直接 kill 掉 inotifywait

```cmd
killall inotifywait
```

## 其他應用範例

### 觸發 bash

```cmd
inotifywait -m -e attrib /home/temp/ |
  while read events; do
    echo "hello world"
  done
```

### 執行程式

```cmd
inotifywait -m -e close_write /home/temp/ |
  while read events; do
    python3 /home/test.py
  done
```

### 自動重新載入設定檔

因為每次修改 crontab 後, 都要手動執行 `sudo service cron reload`,

現在透過 inotifywait 來完成自動執行,

Debian 和 Ubuntu 的檔案保存在 `/var/spool/cron/crontabs`.

也就是當你修改 `crontab -e` 後, 這裡面的檔案會變動.

指令如下

```cmd
sudo inotifywait -m -e modify /var/spool/cron/crontabs |
  while read events; do
    echo <pwd> | sudo service cron reload
  done
```

你也可以試著把它改成在背景運作

```cmd
sudo inotifywait -m -e modify /var/spool/cron/crontabs \
-d -o /home/logs.txt |
  while read events; do
    echo <pwd> | sudo service cron reload
  done
```

最後補充說明一下, 重開機後, `inotifywait` 會失效,

所以可以把上面的指令設定為開機自動執行或是設定 systemctl.

systemctl 可參考之前的文章 [systemctl](https://github.com/twtrubiks/linux-note/tree/master/systemctl-tutorial) 或 [在 Linux 中自動啟動 docker](https://github.com/twtrubiks/docker-tutorial/tree/master/docker-auto-run-linux) :smile:

建立 `watch.service`

```cmd
sudo vim /etc/systemd/system/watch.service
```

`watch.service` 裡面填入,

```sh
[Unit]
Description=my watch
Requires=watch.service
After=watch.service

[Service]
Type=simple
RemainAfterExit=yes
WorkingDirectory=<YOUR PATH>
ExecStart=/usr/bin/sh watch.sh
TimeoutStartSec=10

[Install]
WantedBy=multi-user.target
```

`watch.sh` 如下 (就是剛剛介紹的 code)

```cmd
sudo inotifywait -m -e modify /var/spool/cron/crontabs \
-d -o /home/logs.txt |
  while read events; do
    sudo service cron reload
  done
```

重新載入設定檔

```cmd
sudo systemctl daemon-reload
```

啟動 `watch.service`

```cmd
sudo systemctl start watch
sudo systemctl enable watch
```