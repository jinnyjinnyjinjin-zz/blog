---
title: "[Docker] 자주 쓰이는 Docker 명령어"
date: 2020-11-08T17:17:19+09:00
tags: [Docker]
draft: false
---
## Container
실행 중인 컨테이너 확인
```shell
$ docker ps
```
모든 컨테이너 확인
```shell
$ docker ps -a
```
실행중인 컨테이너 정지
```shell
$ docker stop <CONTAINER_ID>
```
정지시킨 컨테이너 삭제
```shell
$ docker rm <CONTAINER_ID>
```
컨테이너 강제 삭제
```shell
$ docker rm -f <CONTAINER_ID>
```
컨테이너 전체 삭제
```shell
$ docker rm $(docker ps -a)
```
## Image
이미지 확인
```shell
$ docker images
```
이미지 삭제
```shell
$ docker rmi <IMAGE_ID>
```
이미지 전체 삭제
```shell
$ docker rmi $(docker images -a -q)
```
