## post请求

  ```
  # curl -H [headers] -X [POST] -d [JSON-Data] url
  
  curl -H "Content-Type:application/json" -X POST -d {'content':'name'} http://localhost:8080/test
  ```
