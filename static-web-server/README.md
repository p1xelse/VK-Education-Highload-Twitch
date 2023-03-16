# Highload static web server
## Архитектура и ЯП
C++, prefork

## Запуск сервера на порту 80
```
make build-server-docker
make run-server
```

## Запуск nging
```
make build-nginx-docker
make run-nginx
```

## Запуск бенчмарков 
```
make bench-static-serverx
make bench-nginx
```

## Тесты 
```
❯ ./httptest.py
test_directory_index (__main__.HttpServer)
directory index file exists ... ok
test_document_root_escaping (__main__.HttpServer)
document root escaping forbidden ... ok
test_empty_request (__main__.HttpServer)
Send empty line ... ok
test_file_in_nested_folders (__main__.HttpServer)
file located in nested folders ... ok
test_file_not_found (__main__.HttpServer)
absent file returns 404 ... ok
test_file_type_css (__main__.HttpServer)
Content-Type for .css ... ok
test_file_type_gif (__main__.HttpServer)
Content-Type for .gif ... ok
test_file_type_html (__main__.HttpServer)
Content-Type for .html ... ok
test_file_type_jpeg (__main__.HttpServer)
Content-Type for .jpeg ... ok
test_file_type_jpg (__main__.HttpServer)
Content-Type for .jpg ... ok
test_file_type_js (__main__.HttpServer)
Content-Type for .js ... ok
test_file_type_png (__main__.HttpServer)
Content-Type for .png ... ok
test_file_type_swf (__main__.HttpServer)
Content-Type for .swf ... ok
test_file_urlencoded (__main__.HttpServer)
urlencoded filename ... ok
test_file_with_dot_in_name (__main__.HttpServer)
file with two dots in name ... ok
test_file_with_query_string (__main__.HttpServer)
query string with get params ... ok
test_file_with_slash_after_filename (__main__.HttpServer)
slash after filename ... ok
test_file_with_spaces (__main__.HttpServer)
filename with spaces ... ok
test_head_method (__main__.HttpServer)
head method support ... ok
test_index_not_found (__main__.HttpServer)
directory index file absent ... ok
test_large_file (__main__.HttpServer)
large file downloaded correctly ... ok
test_post_method (__main__.HttpServer)
post method forbidden ... ok
test_request_without_two_newlines (__main__.HttpServer)
Send GET without to newlines ... ok
test_server_header (__main__.HttpServer)
Server header exists ... ok

----------------------------------------------------------------------
Ran 24 tests in 0.135s

OK
```

## Результаты бенчмарков ab
### static web server

```
ab -n 1000 -c 100 http://localhost/httptest/wikipedia_russia.html

...

Server Software:        PreforkServer
Server Hostname:        localhost
Server Port:            80

Document Path:          /httptest/wikipedia_russia.html
Document Length:        954824 bytes

Concurrency Level:      100
Time taken for tests:   9.665 seconds
Complete requests:      1000
Failed requests:        0
Total transferred:      954934000 bytes
HTML transferred:       954824000 bytes
Requests per second:    103.47 [#/sec] (mean)
Time per request:       966.501 [ms] (mean)
Time per request:       9.665 [ms] (mean, across all concurrent requests)
Transfer rate:          96487.51 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0    1   2.0      0      11
Processing:   345  948 196.1    936    2084
Waiting:        4  113 149.9     86    1107
Total:        346  949 196.1    936    2090

Percentage of the requests served within a certain time (ms)
  50%    936
  66%    984
  75%   1017
  80%   1029
  90%   1083
  95%   1116
  98%   1954
  99%   2002
 100%   2090 (longest request)
```

### nginx

```
ab -n 1000 -c 100 http://localhost:8888/httptest/wikipedia_russia.html

...

Server Software:
Server Hostname:        localhost
Server Port:            8888

Document Path:          /httptest/wikipedia_russia.html
Document Length:        0 bytes

Concurrency Level:      100
Time taken for tests:   1.617 seconds
Complete requests:      1000
Failed requests:        0
Total transferred:      0 bytes
HTML transferred:       0 bytes
Requests per second:    618.42 [#/sec] (mean)
Time per request:       161.701 [ms] (mean)
Time per request:       1.617 [ms] (mean, across all concurrent requests)
Transfer rate:          0.00 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0    0   0.0      0       1
Processing:     1    1   0.3      1       6
Waiting:        0    0   0.0      0       0
Total:          1    2   0.3      1       6
ERROR: The median and mean for the total time are more than twice the standard
       deviation apart. These results are NOT reliable.

Percentage of the requests served within a certain time (ms)
  50%      1
  66%      2
  75%      2
  80%      2
  90%      2
  95%      2
  98%      2
  99%      3
 100%      6 (longest request)
```

