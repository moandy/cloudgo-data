### xorm体验
- 全功能ORM（几乎）
- 关联（包含一个，包含多个，属于，多对多，多种包含）
- Callbacks（创建/保存/更新/删除/查找之前/之后）
- 预加载（急加载）
- 事务
- 复合主键
- SQL Builder
- 自动迁移
- 日志
- 可扩展，编写基于GORM回调的插件
- 每个功能都有测试
- 开发人员友好


#### 运行
`go run main.go`
服务跑起来后，进行测试，8000端口运行原版程序，8080运行xorm程序
```
mohk@moandy:~/Workspace/Go/src/cloudgo-data$ curl -d "username=ooo&departname=1" http://localhost:8080/service/userinfo
{
  "UID": 1,
  "UserName": "ooo",
  "DepartName": "1",
  "CreateAt": "2017-11-30T20:31:42.9758369+08:00"
}
mohk@moandy:~/Workspace/Go/src/cloudgo-data$ curl http://localhost:8080/service/userinfo?userid=
[
  {
    "UID": 1,
    "UserName": "ooo",
    "DepartName": "1",
    "CreateAt": "2017-11-30T04:31:42+08:00"
  }
]
mohk@moandy:~/Workspace/Go/src/cloudgo-data$ curl http://localhost:8080/service/userinfo?userid=1
{
  "UID": 1,
  "UserName": "ooo",
  "DepartName": "1",
  "CreateAt": "2017-11-30T04:31:42+08:00"
}
```
```
mohk@moandy:~/Workspace/Go/src/cloudgo-data$ ab -n 1000 -c 100 http://localhost:8080/?userid=
This is ApacheBench, Version 2.3 <$Revision: 1706008 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/

Benchmarking localhost (be patient)
Completed 100 requests
Completed 200 requests
Completed 300 requests
Completed 400 requests
Completed 500 requests
Completed 600 requests
Completed 700 requests
Completed 800 requests
Completed 900 requests
Completed 1000 requests
Finished 1000 requests


Server Software:
Server Hostname:        localhost
Server Port:            8080

Document Path:          /?userid=
Document Length:        19 bytes

Concurrency Level:      100
Time taken for tests:   2.295 seconds
Complete requests:      1000
Failed requests:        0
Non-2xx responses:      1000
Total transferred:      176000 bytes
HTML transferred:       19000 bytes
Requests per second:    435.76 [#/sec] (mean)
Time per request:       229.484 [ms] (mean)
Time per request:       2.295 [ms] (mean, across all concurrent requests)
Transfer rate:          74.90 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0   10  29.0      0     162
Processing:     3  207  50.7    206     446
Waiting:        2  205  52.6    206     446
Total:          4  217  42.7    211     446

Percentage of the requests served within a certain time (ms)
  50%    211
  66%    221
  75%    237
  80%    261
  90%    278
  95%    301
  98%    312
  99%    317
 100%    446 (longest request)
```
```
mohk@moandy:~/Workspace/Go/src/cloudgo-data$ ab -n 1000 -c 100 http://localhost:8000/?userid=
This is ApacheBench, Version 2.3 <$Revision: 1706008 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/

Benchmarking localhost (be patient)
Completed 100 requests
Completed 200 requests
Completed 300 requests
Completed 400 requests
Completed 500 requests
Completed 600 requests
Completed 700 requests
Completed 800 requests
Completed 900 requests
Completed 1000 requests
Finished 1000 requests


Server Software:
Server Hostname:        localhost
Server Port:            8000

Document Path:          /?userid=
Document Length:        19 bytes

Concurrency Level:      100
Time taken for tests:   1.078 seconds
Complete requests:      1000
Failed requests:        0
Non-2xx responses:      1000
Total transferred:      176000 bytes
HTML transferred:       19000 bytes
Requests per second:    927.27 [#/sec] (mean)
Time per request:       107.844 [ms] (mean)
Time per request:       1.078 [ms] (mean, across all concurrent requests)
Transfer rate:          159.37 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        1   27  11.9     26      75
Processing:    14   76  24.7     75     187
Waiting:        5   51  23.5     46     157
Total:         32  103  24.4    104     232

Percentage of the requests served within a certain time (ms)
  50%    104
  66%    115
  75%    118
  80%    122
  90%    130
  95%    137
  98%    153
  99%    179
 100%    232 (longest request)
```