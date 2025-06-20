[中文版](README.md)

# Apache Bench Tutorial

Also known as ab.

It is mainly used to test the performance of a server (stress testing).

## Installation

```cmd
sudo apt-get install apache2-utils
```

Check the installed version:

```cmd
ab -V
```

View command help:

```cmd
ab --help
```

## Example

```cmd
ab -n 10 -c 2 http://www.xxxxxx.com/
```

This sends a total of 10 requests (with 2 concurrent users).

*   `-n` requests: Number of requests to perform.
*   `-c` concurrency: Number of multiple requests to make at a time.

## Output Result

```text
Server Software:        nginx/1.21.6
Server Hostname:        xxxx
Server Port:            80

Document Path:          /
Document Length:        84 bytes

Concurrency Level:      2              // Number of concurrent users
Time taken for tests:   0.041 seconds  // Time taken for the test
Complete requests:      10             // Completed requests
Failed requests:        0              // Failed requests
Total transferred:      4030 bytes     // Total network traffic
HTML transferred:       840 bytes      // HTML network traffic
Requests per second:    241.13 [#/sec] (mean) // Throughput (Requests per second), average number of requests processed per second
Time per request:       8.294 [ms] (mean)     // Average user wait time
Time per request:       4.147 [ms] (mean, across all concurrent requests) // Time per request / Concurrency Level
Transfer rate:          94.90 [Kbytes/sec] received // Average network traffic per second

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

// Distribution of request times, e.g., 90% of requests were processed within 10ms.
