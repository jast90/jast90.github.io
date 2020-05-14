---
layout: bootstrap
title: 'kubernetes minikube安装和运行nginx实例'
description: 'kubernetes安装和运行nginx实例'
date: '2019-8-2 14:26:00'
author: Jast
---

### 1.参考

1. [install minikube](https://kubernetes.io/docs/tasks/tools/install-minikube/)
2. [由于中国网络问题导致安装失败的解决方法](https://github.com/kubernetes/minikube/issues/3860)
3. [hello-minikube](https://kubernetes.io/docs/tutorials/hello-minikube/)

### 2.minikube安装

参考1、2

#### 2.1 遇到的问题

1. minikube-v1.3.0.iso文件下载失败。手动下载后将文件复制到`~/.minikube/cache/iso`目录下
2. 由于中国网络问题无法下载google的相关容器，可以通过`--image-repository=registry.cn-hangzhou.aliyuncs.com/google_containers` 参数来创建kubernetes集群，即：`minikube start --image-repository=registry.cn-hangzhou.aliyuncs.com/google_containers`


### 3.使用

#### 3.1创建、查看Pod

nginxPod.yaml：
```yaml
apiVersion: v1
kind: Pod
metadata:
    name: nginx
    labels:
      app: nginx
spec:
    containers:
    - image: nginx
      name: nginx
      ports:
      - containerPort: 80
```

创建Pod
```shell
kubectl create -f nginxPod.yaml
>>
pod/nginx created
```

查看Pod
```shell
 kubectl get pod
 >>
 NAME    READY   STATUS    RESTARTS   AGE
 nginx   1/1     Running   0          5m31s
```

#### 3.2创建 Service

nginxService.yml：
```yaml
apiVersion: v1
kind: Service
metadata:
    name: nginx
spec:
    type: LoadBalancer
    ports: 
    - port: 80
      targetPort: 80
    selector: 
      app: nginx
```
创建Service
```shell
kubectl create -f nginxService.yaml
>>
service/nginx created
```
查看Service
```shell
kubectl get pod
>>
NAME         TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
kubernetes   ClusterIP      10.96.0.1        <none>        443/TCP        6h39m
nginx        LoadBalancer   10.103.218.102   <pending>     80:30521/TCP   8m23s
```

#### 3.3访问服务

在支持负载均衡器的云提供商上，将配置外部IP地址以访问服务。在Minikube上，LoadBalancer类型通过`minikube service`命令使服务可访问。

minikube：
```
minikube service nginxService
```

或者直接通过`宿主机IP:端口`访问，如：`http://192.168.99.100:30521`

#### 3.4 dashboard使用

运行如下命令会自动打开浏览器到管理界面
```shell
minikube dashboard
```