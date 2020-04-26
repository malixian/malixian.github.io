---
layout:     post
title:      方便的将docker命令转化成K8s Pod Yaml格式文件
subtitle:   Easy dokcer command to k8s yaml
date:       2020-04-26
author:     malixian
header-img: img/post-bg-cook.jpg
catalog: true
tags:
    - docker
    - Kubernetes
---
## Introduction
在使用k8s或者开发k8s相关项目时候，我们通常要写一个yaml文件进行测试，但是写Yaml的文件
通常是一个比较苦的差事并且文件中存在大量的重复。通常我在写一个yaml文件时都要先搜索一个template，在进行调整。
相比于写一个yaml文件我们却更熟悉docker命令，所以我开发了一个可以轻松的将docker 命令转化为Pod Yaml的一个小工具。

## Install
- git clone git@github.com:malixian/cmd2yaml.git
- sh build.sh

## Example

use c2y like this
```
c2y -i 'docker run --network host -v /tmp:/tmp -p 12345:12345 --device /dev/ your-container bash -c "python test.py' -n "test"
```

then you can find test.yaml in current path

```
apiVersion: v1
kind: Pod
metadata:
  name: test
  labels:
    name: test
spec:
  hostNetwork: true
  containers:
  - name: your-container
    image: your-container
    command:
    - bash
    - -c
    args:
    - python test.py
    ports:
    - hostPort: 12345
      containerPort: 12345
    volumeMounts:
    - name: ssh
      mountPath: /tmp
  volumes:
  - name: ssh
    hostPath:
      path: /tmp
```

## Attention

Now support docker flag has:
- command
- args
- volumeMount
- network host

Now support pod options has:
- containers
- volumes
