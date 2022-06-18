---
title: docker-related
date created: 2022-01-31 23:48
status: note
tags:
  - docker
  - linux
aliases: docker-related
---

# Docker 学习新手笔记：从入门到放弃

[Docker 学习新手笔记：从入门到放弃 - Joe’s Blog](https://hijiangtao.github.io/2018/04/17/Docker-in-Action/#/)

[Docker 中刪除 Images 鏡像 及 Containers - Linux 技術手札](https://www.ltsplus.com/linux/docker-delete-images-containers)

找出 image id:

`docker images`

刪除 image :

`docker rmi image_id`

找出 container id:

`docker ps -a`

停止及刪除的指令:

```
docker stop container_id  
# docker rm container_id
```