## post请求

  ```
  # curl -H [headers] -X [POST] -d [JSON-Data] url
  
  curl -H "Content-Type:application/json" -X POST -d {'content':'name'} http://localhost:8080/test
  ```
## siege 压测post请求

  ```
  # 压测file组装: 每一行 curl POST data
  http://localhost:8080/test? POST {'content':'name'}
  
  # 压测命令
  # -f [压测文件]
  # -b 非阻塞模式
  # -c 并发个数
  # -t 压测时间，H=小时, M=分钟, S=秒
  siege -f file -b -c 100 -t 2H -H "Content-Type:application/json"
  ```
