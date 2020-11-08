---
title: "[Docker] Docker로 MariaDB 구동하기"
date: 2020-11-08T14:10:23+09:00
tags: [Docker, DB]
draft: false
---
로컬에서 DB 를 시스템에 직접 설치하기보다 도커를 활용해 쉽고 간단하게 컨테이너를 띄워 사용할 수 있다.

## 시스템 환경
* MacOS
* Docker 19.03.8
* Docker-compose 1.25.4
* MariaDB 10.3

## 1. docker run 명령어로 실행
`docker run` 명령어로 도커 허브에서 이미지를 바로 다운 받아 컨테이너를 바로 실행 시킨다. 
```shell
$ docker run --name <컨테이너 이름> -e MYSQL_ROOT_PASSWORD=<db 접속 비밀번호> -p 3306:3306 -d mariadb:tag
```
`run` 다운 받은 도커 이미지로 컨테이너를 실행시키는 명령어.
`--name` 실행시키는 컨테이너의 이름을 지정할 수 있는 옵션.
`-p` 포트를 지정. 호스트에서 `3306` 으로 접속 시, 컨테이너 `3306` 으로 포워딩.
`-d` 컨테이너를 백그라운드로 실행.
`tag` 실행하고자 하는 MariaDB 의 버전을 지정. `ex) mariadb:10.3`

실행 후, 아래 명령어로 컨테이너가 실행되었는지 확인할 수 있다.
```shell
$ docker ps
```
정상적으로 실행된 컨테이너
```shell
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    NAMES
dfce24bf4cac        mariadb:10.3        "docker-entrypoint.s…"   48 seconds ago      Up 47 seconds       0.0.0.0:3306->3306/tcp   mariadb_test
```

## 2. yaml 파일로 실행
`yaml` 파일을 생성해서 실행시킬 어플리케이션에 대해 더욱 상세한 설정이 가능하다. 파일 실행을 위해서는 `docker-compose`가 설치되어 있어야 한다. 

<b>`yaml` 파일 생성</b>

yaml 파일명은 원하는 이름으로 생성할 수 있는데`docker-compose`명령어로 실행할 때 옵션값 `-f` 를 주어야 한다. 반면에, `docker-compose up` 명령어를 옵션 값 없이 실행하면 자동으로 `docker-compose.yaml` 을 찾아 실행시킨다. 또한, `.yaml` 또는 `yml` 모두 사용 가능하다.

```shell
$ vi docker-compose.yaml
```
다음과 같이 `mariadb` 컨테이너 실행에 필요한 설정을 `yaml` 파일 형식에 따라 작성한다.
```bash
version: '3.1'

services:
  mariadb:
    image: mariadb:10.3
    container_name: mariadb-test
    environment:
      MYSQL_ROOT_PASSWORD: password
      MYSQL_DATABASE: my_db 
      MYSQL_USER: jinnyjinnyjinjin
      MYSQL_PASSWORD: password
    ports:
      - 3306:3306
```
`yaml` 파일에서는 `services` 와 `networks` 그리고 `volumes` 설정 파트가 있다.   
`version`  compose 파일 포맷 버전.
`services` 실행하고자 하는 컨테이너의 설정 값을 셋팅하는 영역. `command-line` 으로 실행할 때, `docker run` 하위로 들어가는 파라미터 값을 설정하는 것과 동일.
`mariadb` 실행하고자 하는 서비스명. (직접 지정 가능)
`container_name` 컨테이너명 지정.
`image` 실행하려는 컨테이너의 이미지명과 버전(tag).
`environment` 어플리케이션 환경설정. 데이터베이스에 접속하기 위한 비밀번호 및 유저명 등을 설정한다.
`port` 포트를 지정. 호스트에서 `3306` 으로 접속 시, 컨테이너 `3306` 으로 포워딩.

파일을 생성하고 `compose` 명령어로 실행한다. `-d` 옵션을 주어 백그라운드로 실행한다.
```shell
$ docker-compose up -d
```
실행 후 아래 명령어를 입력하면 실행 중인 컨테이너를 확인할 수 있다.
```
$ docker ps
```
```shell
CONTAINER ID        IMAGE                                                 COMMAND                  CREATED             STATUS                      PORTS                    NAMES
dfce24bf4cac        mariadb:10.3                                          "docker-entrypoint.s…"   42 minutes ago      Up 42 minutes               0.0.0.0:3307->3307/tcp   mariadb_test
```

