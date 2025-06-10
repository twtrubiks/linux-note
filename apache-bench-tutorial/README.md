[English Version](README_en.md)

# Apache Bench 教學

也被簡稱為 ab,

主要拿來測試 server 的執行效能(壓力測試)

安裝

```cmd
sudo apt-get install apache2-utils
```

查看安裝版本

```cmd
ab -V
```

查看指令說明

```cmd
ab --help
```

範例

```cmd
ab -n 10 -c 2 http://www.xxxxxx.com/
```

總共發出 10 個 request (2 個使用者同時進行)

`-n` requests 代表 Number of requests to perform

`-c` concurrency 代表 Number of multiple requests to make at a time

輸出結果

```text
Server Software:        nginx/1.21.6
Server Hostname:        xxxx
Server Port:            80

Document Path:          /
Document Length:        84 bytes

Concurrency Level:      2              //並行數
Time taken for tests:   0.041 seconds  //測試的時間
Complete requests:      10             //完成的請求
Failed requests:        0              //失敗的請求
Total transferred:      4030 bytes     //全部的網路傳輸量
HTML transferred:       840 bytes      //html 網路傳輸量
Requests per second:    241.13 [#/sec] (mean) //吞吐率 throughput (Requests per second), 平均每秒處理 request 數量
Time per request:       8.294 [ms] (mean)     //使用者平均等待時間
Time per request:       4.147 [ms] (mean, across all concurrent requests) // Time per request // Concurrency Level
Transfer rate:          94.90 [Kbytes/sec] received // 平均每秒網路流量

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0    0   0.1      0       0
Processing:     5    7   1.6      6      10
Waiting:        5    6   1.6      6      10
Total:          5    7   1.6      6      10

Percentage of the requests served within a certain time (ms)
  50%      6
  66%      6
  75%      8
  80%      8
  90%     10
  95%     10
  98%     10
  99%     10
 100%     10 (longest request)

// 請求分佈的時間, 像是 90% 的請求在 10ms 處理完畢.
```
