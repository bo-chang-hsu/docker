# **docker create redis**

- 搜尋 image:
```
docker search redis
```

- 下載 image:
```
docker pull redis
```

- 執行 image:
```
docker run -itd --name stock-redis -p 6379:6379 redis
```

- 進container 測試
```
docker exec -it _______ bash
```

- 進cli
```
redis-cli
```

- 系統會回應進入到 127.0.0.1:6379 內，透過 Ping 確認會回傳 PONG 
```
Ping
// PONG
```

###### 更多應用可以參考以以下網址
```
https://jed1978.github.io/2018/05/11/Redis-Programming-CSharp-Basic-1.html
```
