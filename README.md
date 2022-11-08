# Docker-Note
- A note shared with my colleague.
- This was written in 2018. Some settings may be deprecate.

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

```
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
ca4f61b1923c: Pull complete
Digest: sha256:ca0eeb6fb05351dfc8759c20733c91def84cb8007aa89a5bf606bc8b315b9fc7
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.
...
```

```
docker container ls --all
```

- if the docker run successully
```
CONTAINER ID     IMAGE           COMMAND      CREATED            STATUS
54f4984ed6a8     hello-world     "/hello"     20 seconds ago     Exited (0) 19 seconds ago
```

## Containerize Your App
- 利用設定檔docker file將所需的環境參數寫入
- take a node.js app as an example
- the sample file is `dockerfile-sample`

## Run Containers
### Build And Run One Container
1. 上傳專案原始檔，含DockerFile到虛擬機 (We should build a pipeline instead of manually upload.)
2. 對資料夾下建置指令
```
docker build -t {別名} {資料夾路徑}
# ex:
docker build -t friendlyhello .
# check if it build successfully
docker image ls
```
- output
```
REPOSITORY            TAG                 IMAGE ID
friendlyhello         latest              326387cea398
```

3. 啟動image
```
docker run -p {內部port}:{外部port} {別名或IMAGE ID}
# ex:
docker run -p 4000:80 friendlyhello
# check running containers
docker container ls
# check log
docker logs {別名或IMAGE ID}
```
- 如果是個網站也可以用curl呼叫
``` curl http://localhost:4000 ```
- 在背景中執行則加上-d
```
docker run -p 4000:80 friendlyhello -d
```

4. stop container
``` 
docker stop {別名或IMAGE ID}
```

5. 上傳 image到registry
- todo: build our private registry

## Use Docker Compose For Multiple Containers
當服務由多個Container組成時，可以利用docker-compose一次將所有的image建置、啟動。

設定檔docker-compose.yml中也可以定義揭露的port、Container之間的相依性、來源檔案等等。
- `docker-compose-sample.yml` 有詳盡的註解

### 透過docker-compose建置並啟動
```
docker-compose up
# or
docker-compose up -d 
# build only
docker-compose build  
```

### 透過docker-compose停止所有相關服務（當然也可以個別下docker stop)
```
docker-compose stop
```

## frequently used docker CMDs

```
## List Docker CLI commands
docker
docker container --help

## Display Docker version and info
docker --version
docker version
docker info

## Execute Docker image
docker run hello-world

## List Docker images
docker image ls

## List Docker containers (running, all, all in quiet mode)
docker container ls
docker container ls --all
docker container ls -aq

## loggin a running docker
docker exec -it <image id> bash

docker build -t friendlyhello .  # Create image using this directory's Dockerfile
docker run -p 4000:80 friendlyhello  # Run "friendlyname" mapping port 4000 to 80
docker run -d -p 4000:80 friendlyhello         # Same thing, but in detached mode
docker container ls                                # List all running containers
docker container ls -a             # List all containers, even those not running
docker container stop <hash>           # Gracefully stop the specified container
docker container kill <hash>         # Force shutdown of the specified container
docker container rm <hash>        # Remove specified container from this machine
docker container rm $(docker container ls -a -q)         # Remove all containers
docker image ls -a                             # List all images on this machine
docker image rm <image id>            # Remove specified image from this machine
docker image rm $(docker image ls -a -q)   # Remove all images from this machine
docker login             # Log in this CLI session using your Docker credentials
docker tag <image> username/repository:tag  # Tag <image> for upload to registry
docker push username/repository:tag            # Upload tagged image to registry
docker run username/repository:tag                   # Run image from a registry

```
