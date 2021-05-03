1.下載 image
docker pull redis

2.執行 image container 的名字為 redis-lab
docker run -itd --name stock-redis -p 6379:6379 redis

3.進 container 測試
docker exec -it _______ bash

4.進cli
redis-cli

5.系統會回應進入到 127.0.0.1:6379 內，透過 Ping 確認會回傳 PONG 
Ping
// PONG

$ docker ps
CONTAINER ID   IMAGE                          COMMAND                  CREATED         STATUS         PORTS                    NAMES
1c43f71e82a0   redis                          "docker-entrypoint.s…"   5 seconds ago   Up 3 seconds   0.0.0.0:6379->6379/tcp   stock-redis


重點應用 https://jed1978.github.io/2018/05/11/Redis-Programming-CSharp-Basic-1.html
