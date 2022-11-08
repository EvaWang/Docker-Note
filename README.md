# Docker-Note
A note shared with my colleague
This was written in 2018. Some settings may be deprecate.

## What is docker?
https://www.docker.com/

## Why Use Docker?
先說目前用到的(根據我們team的需求）

1. 省時：重啟一個Container比重啟VM快速。
2. 可標準化：根據同一份DockerFile建置的檔案即可產生同樣的image，不需要人工設定環境參數，減少出錯的機率，也使部署可程式化。
3. 容易移植：同樣的image可以跑在任何有架設docker的環境中。

## 環境準備
- 部署位置：Azure虛擬機，docker-test
- DNS：ta543dockerhub.southeastasia.cloudapp.azure.com
- OS：Ubuntu(Xenial) 16.04
- 帳號密碼：於Azure Portal虛擬機器管理頁面中的重設密碼，可新增一個sudo user

### 使用SSH連入Linux
- in case anyone who is not familiar with linux cmd.

``` ssh {user name}@{ip or DNS name}```

### Install Docker CE
參考：https://docs.docker.com/install/linux/docker-ce/ubuntu/

### Install Docker Compose
參考：https://docs.docker.com/compose/install/

### Test Docker Installation
```
docker run hello-world
```

- output: 

Unable to find image 'hello-world:latest' locally

latest: Pulling from library/hello-world

ca4f61b1923c: Pull complete

Digest: sha256:ca0eeb6fb05351dfc8759c20733c91def84cb8007aa89a5bf606bc8b315b9fc7

Status: Downloaded newer image for hello-world:latest


Hello from Docker!

This message shows that your installation appears to be working correctly.
...

```
docker container ls --all
```
