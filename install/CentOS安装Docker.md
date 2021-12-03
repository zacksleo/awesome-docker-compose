# Centos 安装 Docker

## 1. 删除旧版

```bash
sudo yum remove docker \
                  docker-common \
                  docker-selinux \
                  docker-engine
```

## 2.  安装库

```bash
sudo yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2
```

## 3. 配置stable repo

```bash
sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
```

### 4. 安装

sudo yum install docker-ce

### 5. 启动

sudo systemctl start docker

### 6. 开机启动

sudo systemctl enable docker

### 7 . hello world

sudo docker run hello-world

### 8. 安装日志插件(可选)

docker plugin install grafana/loki-docker-driver:latest --alias loki --grant-all-permissions
