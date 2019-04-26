---
layout: bootstrap
title: 'CentOS install docker CE and docker-compose'
description: 'CentOS install docker and docker-compose'
date: '2019-04-26 14:31:00'
author: Jast
---
## centos 安装docker CE
0.[参考](https://docs.docker.com/install/linux/docker-ce/centos/)

1. 安装需要的包
```
sudo yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2
```

2.添加仓库
```
sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
```

3. 安装docker
```
sudo yum install docker-ce docker-ce-cli containerd.io
```

4. 启动  
```
sudo systemctl start docker
```

## centos 安装docker compose
1. [下载](https://github.com/docker/compose/releases)docker compose 

```
sudo curl -L "https://github.com/docker/compose/releases/download/1.24.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```
2. 更改执行权限

```
sudo chmod +x /usr/local/bin/docker-compose
```

3. 查询版本

```
docker-compose --version
```