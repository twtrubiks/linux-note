# fstrim ssd 教學

測試硬碟資訊以及讀寫速度

安裝指令

```cmd
sudo apt-get install hdparm
```

可以先用 `df -h` 查看硬碟代號,

然後執行

```cmd
sudo hdparm -T -t /dev/sda2
```

`-t` 代表 Perform device read timings

`-T` 代表 Perform cache read timings

輸出訊息為

```txt
Timing cached reads:   23086 MB in  1.99 seconds = 11606.17 MB/sec
Timing buffered disk reads: 5192 MB in  3.00 seconds = 1730.51 MB/sec
```

第一行代表快取硬碟速度, 第二行則代表硬碟實體讀寫速度.

更多指令用法可使用 `hdparm` 查詢.
