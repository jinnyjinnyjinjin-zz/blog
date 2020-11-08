---
title: "[Docker] Login Error"
date: 2020-11-08T13:52:02+09:00
tags: [Docker]
draft: false
---
## 실습환경

- Ubuntu 16

우분투 터미널에서 `docker login` 명령어로 도커 로그인을 시도했을 때, 아래와 같은 에러가 발생 한다면 이렇게 해결할 수 있다.

## 에러내용

```bash
Remote error from secret service: org.freedesktop.Secret.Error.IsLocked: Cannot create an item in a locked collection
Error saving credentials: error storing credentials - err: exit status 1, out Cannot create an item in a locked collection.
```

## 해결방법

`gnupg2` 를 설치한다. `gnupg2` 는 안전한 통신과 데이터 저장을 위한 디지털 시그니쳐와 증명서 암호화 툴이다. 

```bash
$ sudo apt-get update
$ sudo apt install gnupg2 pass
```

설치 후,  docker login 재시도

```bash
$ docker login
```

## 결과

```bash
Password:
WARNING! Your password will be stored unencrypted in /home/User_id/.docker/config.json.
Configure a credential helper to remove this warning. See
[https://docs.docker.com/engine/reference/commandline/login/#credentials-store](https://docs.docker.com/engine/reference/commandline/login/#credentials-store)

Login Succeeded
```

로컬 `/home/User_id/.docker/config.json` path 에 내 패스워드가 암호화되지 않고 저장된다는 경고가 뜨고 로그인은 성공한다. 만약 내 암호가 이렇게 저장 되는 것이 내키지 않는다면, 경고문과 함께 안내되는 주소로 접속 해 가이드를 따르면 된다.

