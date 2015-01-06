---
layout: post
title: Web performance optimization
---

Some tips about web performance optimization based on LNMP platform


###Nginx

worker_processes 8;nginx进程数，建议按照cpu数目来指定，一般为它的倍数。
 
worker_cpu_affinity 00000001 00000010 00000100 00001000 00010000 00100000 01000000 10000000;为每个进程分配cpu，上例中将8个进程分配到8个cpu，当然可以写多个，或者将一个进程分配到多个cpu。
 
worker_rlimit_nofile 102400;这个指令是指当一个nginx进程打开的最多文件描述符数目，理论值应该是最多打开文件数（ulimit -n）与nginx进程数相除，但是nginx分配请求并不是那么均匀，所以最好与ulimit -n的值保持一致。
 
use epoll;使用epoll的I/O模型，这个不用说了吧。
 
worker_connections 102400;每个进程允许的最多连接数，理论上每台nginx服务器的最大连接数为worker_processes*worker_connections。
 
keepalive_timeout 60;keepalive超时时间。


###php-fpm

```
pm.max_children：静态方式下开启的php-fpm进程数量
pm.start_servers：动态方式下的起始php-fpm进程数量
pm.min_spare_servers：动态方式下的最小php-fpm进程数
pm.max_spare_servers：动态方式下的最大php-fpm进程数量

listen = /var/run/php5-fpm.sock; fpm进程通讯方式
```

###测试工具

####siege

```
Lifting the server siege...      done.

siege aborted due to excessive socket failure; you

can change the failure threshold in $HOME/.siegerc

Transactions:         3045 hits

Availability:       61.58 %

Elapsed time:       10.88 secs

Data transferred:         2.49 MB

Response time:         1.12 secs

Transaction rate:       279.87 trans/sec

Throughput:         0.23 MB/sec

Concurrency:       313.87

Successful transactions:        3045

Failed transactions:         1900

Longest transaction:         3.77

Shortest transaction:         0.00
```


####wrk

```
./wrk -t16 -c2000 -d30s http://127.0.0.1/

Running 30s test @ http://127.0.0.1/

  16 threads and 2000 connections

  Thread Stats   Avg      Stdev     Max   +/- Stdev

    Latency    18.85s     1.97s   25.01s    95.80%

    Req/Sec    34.03     58.71   444.00     78.41%

  9196 requests in 30.03s, 32.10MB read

  Socket errors: connect 995, read 969, write 0, timeout 19808

  Non-2xx or 3xx responses: 29

Requests/sec:    306.25

Transfer/sec:      1.07MB
```


