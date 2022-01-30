## 安装 Docker

`scoop install docker`

_请注意这不会安装 Docker Engine_

## 使用 Docker CLI

通过 [Docker CLI](https://docs.docker.com/engine/reference/commandline/cli/) 连接到 Docker Engine:

```
docker -H <host:port>
```

Docker Engine 守护程序必须监听一个 [tcp socket](https://docs.docker.com/engine/reference/commandline/dockerd/#daemon-socket-option).

## 使用 Docker Machine 配置 Docker Engine

要求: [Virtualbox](https://www.virtualbox.org/), [VMware](https://www.vmware.com/), Hyper-V 或任何 [Docker Machine](https://docs.docker.com/machine/overview/) [drivers](https://docs.docker.com/machine/drivers/)

1. 创建我们的 Docker 基础机器 (会被命名为 _default_):

```
docker-machine create default
```

2. 每次开始使用 Docker

```
docker-machine start
docker-machine env | Invoke-Expression
```

3. 然后我们可以调出任何 Docker 镜像

```
docker run ubuntu /bin/echo "Hello world"
```

4. 完成时:

```
docker-machine stop
```

5. 获取我们的 Docker machine:

```
docker-machine ls
```

### 从 WSL 环境访问

```
eval $(docker-machine.exe env docker-host --shell wsl ) && export DOCKER_CERT_PATH=$(wslpath $DOCKER_CERT_PATH)
```