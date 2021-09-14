Spring Cloud Gateway Benchmark
=======

TL;DR

Proxy | Avg Latency | Avg Req/Sec/Thread
-- | -- | -- 
gateway | 6.61ms | 3.24k
linkered | 7.62ms | 2.82k
zuul | 12.56ms | 2.09k
none | 2.09ms | 11.77k

## Terminal 1 (simple webserver)

```bash
cd static
./webserver # or ./webserver.darwin-amd64 on a mac
```

## Terminal 2 (zuul)
```bash
cd zuul
./mvnw clean package
java -jar target/zuul-0.0.1-SNAPSHOT.jar 
```

## Terminal 3 (gateway)
```bash
cd gateway
./mvnw clean package
java -jar target/gateway-0.0.1-SNAPSHOT.jar 
```

## Terminal 4 (linkerd)
```bash
cd linkerd
java -jar linkerd-1.3.4.jar linkerd.yaml
```

## Terminal N (wrk)

### install `wrk`
Ubuntu: `sudo apt install wrk`

Mac: `brew install wrk`

NOTE: run each one multiple times to warm up jvm

### Gateway bench (8082)
```bash
➜  spring-cloud-gateway-bench git:(master) ✗ wrk -t 10 -c 200 -d 30s http://localhost:8082/hello.txt
Running 30s test @ http://localhost:8082/hello.txt
  10 threads and 200 connections
  Thread Stats   Avg      Stdev     Max   +/- Stdev
    Latency    33.64ms   51.46ms 608.67ms   86.33%
    Req/Sec     1.41k   538.66     4.39k    67.46%
  418952 requests in 30.04s, 59.13MB read
  Socket errors: connect 0, read 147, write 0, timeout 0
Requests/sec:  13945.53
Transfer/sec:      1.97MB


➜  spring-cloud-gateway-bench git:(master) ✗ wrk -t 10 -c 200 -d 30s http://localhost:8082/hello.txt
Running 30s test @ http://localhost:8082/hello.txt
  10 threads and 200 connections
  Thread Stats   Avg      Stdev     Max   +/- Stdev
    Latency    33.75ms   55.07ms   1.07s    87.96%
    Req/Sec     1.37k   566.04     4.43k    66.21%
  409098 requests in 30.04s, 57.74MB read
  Socket errors: connect 0, read 160, write 0, timeout 0
Requests/sec:  13617.84
Transfer/sec:      1.92MB


➜  spring-cloud-gateway-bench git:(master) ✗ wrk -t 10 -c 200 -d 30s http://localhost:8082/hello.txt
Running 30s test @ http://localhost:8082/hello.txt
  10 threads and 200 connections
  Thread Stats   Avg      Stdev     Max   +/- Stdev
    Latency    27.74ms   43.19ms 482.55ms   86.74%
    Req/Sec     1.75k   494.14     3.73k    68.96%
  521358 requests in 30.03s, 73.59MB read
  Socket errors: connect 0, read 135, write 0, timeout 0
Requests/sec:  17363.24
Transfer/sec:      2.45MB


```

### zuul bench (8081)
```bash
➜  spring-cloud-gateway-bench git:(master) ✗ wrk -t 10 -c 200 -d 30s http://localhost:8081/hello.txt
Running 30s test @ http://localhost:8081/hello.txt
  10 threads and 200 connections
  Thread Stats   Avg      Stdev     Max   +/- Stdev
    Latency    29.13ms   44.11ms 727.22ms   91.01%
    Req/Sec     1.13k   369.58     2.68k    69.43%
  336643 requests in 30.09s, 52.71MB read
  Socket errors: connect 0, read 175, write 0, timeout 0
Requests/sec:  11188.37
Transfer/sec:      1.75MB


➜  spring-cloud-gateway-bench git:(master) ✗ wrk -t 10 -c 200 -d 30s http://localhost:8081/hello.txt
Running 30s test @ http://localhost:8081/hello.txt
  10 threads and 200 connections
  Thread Stats   Avg      Stdev     Max   +/- Stdev
    Latency    18.68ms   32.39ms 749.92ms   93.13%
    Req/Sec     1.74k   465.37     3.54k    73.43%
  520015 requests in 30.05s, 81.43MB read
  Socket errors: connect 0, read 91, write 0, timeout 0
Requests/sec:  17302.69
Transfer/sec:      2.71MB


➜  spring-cloud-gateway-bench git:(master) ✗ wrk -t 10 -c 200 -d 30s http://localhost:8081/hello.txt
Running 30s test @ http://localhost:8081/hello.txt
  10 threads and 200 connections
  Thread Stats   Avg      Stdev     Max   +/- Stdev
    Latency    21.02ms   32.94ms 674.93ms   90.21%
    Req/Sec     1.72k   491.71     3.72k    71.90%
  515743 requests in 30.09s, 80.76MB read
  Socket errors: connect 0, read 95, write 0, timeout 0
Requests/sec:  17139.86
Transfer/sec:      2.68MB

```

### linkerd bench (4140)
```bash
➜  spring-cloud-gateway-bench git:(master) ✗ wrk -H "Host: web" -t 10 -c 200 -d 30s http://localhost:4140/hello.txt
Running 30s test @ http://localhost:4140/hello.txt
  10 threads and 200 connections
  Thread Stats   Avg      Stdev     Max   +/- Stdev
    Latency    21.91ms   37.22ms 557.51ms   92.27%
    Req/Sec     1.57k   576.26     4.33k    66.00%
  466950 requests in 30.10s, 84.59MB read
  Socket errors: connect 0, read 222, write 0, timeout 0
Requests/sec:  15515.07
Transfer/sec:      2.81MB



➜  spring-cloud-gateway-bench git:(master) ✗ wrk -H "Host: web" -t 10 -c 200 -d 30s http://localhost:4140/hello.txt
Running 30s test @ http://localhost:4140/hello.txt
  10 threads and 200 connections
  Thread Stats   Avg      Stdev     Max   +/- Stdev
    Latency    11.80ms   15.98ms 376.08ms   91.48%
    Req/Sec     2.40k   487.13     6.20k    74.10%
  715845 requests in 30.03s, 129.69MB read
  Socket errors: connect 0, read 124, write 0, timeout 0
Requests/sec:  23841.02
Transfer/sec:      4.32MB


➜  spring-cloud-gateway-bench git:(master) ✗ wrk -H "Host: web" -t 10 -c 200 -d 30s http://localhost:4140/hello.txt
Running 30s test @ http://localhost:4140/hello.txt
  10 threads and 200 connections
  Thread Stats   Avg      Stdev     Max   +/- Stdev
    Latency    12.98ms   17.13ms 381.08ms   92.39%
    Req/Sec     2.08k   505.32     4.65k    71.77%
  620989 requests in 30.04s, 112.50MB read
  Socket errors: connect 0, read 66, write 0, timeout 0
Requests/sec:  20672.41
Transfer/sec:      3.75MB

```

### no proxy bench (8000)
```bash
➜  spring-cloud-gateway-bench git:(master) ✗  wrk -t 10 -c 200 -d 30s http://localhost:8000/hello.txt
Running 30s test @ http://localhost:8000/hello.txt
  10 threads and 200 connections

  Thread Stats   Avg      Stdev     Max   +/- Stdev
    Latency     5.42ms    3.05ms  63.07ms   82.85%
    Req/Sec     3.83k     1.16k   57.02k    93.27%
  1143813 requests in 30.10s, 161.44MB read
  Socket errors: connect 0, read 62, write 0, timeout 0
Requests/sec:  37994.34
Transfer/sec:      5.36MB

➜  spring-cloud-gateway-bench git:(master) ✗ wrk -t 10 -c 200 -d 30s http://localhost:8000/hello.txt
Running 30s test @ http://localhost:8000/hello.txt
  10 threads and 200 connections
  Thread Stats   Avg      Stdev     Max   +/- Stdev
    Latency     5.67ms    3.56ms  94.98ms   84.43%
    Req/Sec     3.68k   661.30    13.18k    73.13%
  1099764 requests in 30.03s, 155.22MB read
  Socket errors: connect 0, read 63, write 0, timeout 0
Requests/sec:  36616.91
Transfer/sec:      5.17MB

➜  spring-cloud-gateway-bench git:(master) ✗ wrk -t 10 -c 200 -d 30s http://localhost:8000/hello.txt
Running 30s test @ http://localhost:8000/hello.txt
  10 threads and 200 connections
  Thread Stats   Avg      Stdev     Max   +/- Stdev
    Latency     5.12ms    2.30ms  37.79ms   83.41%
    Req/Sec     3.98k   487.03     5.76k    74.10%
  1188734 requests in 30.01s, 167.78MB read
  Socket errors: connect 0, read 61, write 0, timeout 0
Requests/sec:  39610.96
Transfer/sec:      5.59MB


```
