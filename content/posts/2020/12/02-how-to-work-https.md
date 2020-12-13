---
title: "[Network] https 동작 과정"
date: 2020-12-03T18:35:29+09:00
tags: [Network]
draft: false
---
## Http 란?
`Hyper Text Transfer Protocol` 의 약자로 인터넷 통신 규약 입니다. 
접속하고자 하는 주소 앞에 `http` 를 붙여 줌으로써, `나는 지금 http 형식으로 요청 하는거야~` 라는 클라이언트의 요청에 서버는 `아~ 이 요청은 http 형식이구나~` 라고 파악하고 `http` 형식으로 해석하여 클라이언트가 요청한 정보를 전달합니다.

## Https 란?
기존의 `http` 에 보안이라는 뜻의 `Secure`의 `s`를 붙여 보안 기능을 제공합니다.

## Https 필요성
1. 내가 사이트에 보내는 정보들을 제 3자가 보지 못하게 합니다.
2. 접속한 사이트가 믿을 만한 곳인지를 알려줍니다.

## Https 동작 과정
[1. Hand shake](#1.-sand-shake)   
[2. 인증서 검증](#2.-인증서-검증)   
[3. 데이터 암호/복호화](#3.-데이터-암호/복호화)
### 1. Hand shake
우리가 브라우저 주소 입력창에 `http://` 주소를 입력하면 입력한 당사자(클라이언트)와 접속하려는 해당 주소의 서버는 서로의 신뢰 관계를 형성하기 위해 핸드 쉐이크 과정을 수행 합니다. 

1. 클라이언트가 랜덤 데이터를 생성하고 서버에 전달 합니다.

<p align="center">
    <img src="/images/2020/12/hand_shaking_1.png" alt="hand_shaking_1" /><br>
</p>

2. 클라이언트의 랜덤 데이터를 받은 서버는 서버의 랜덤 데이터를 생성하고 생성된 데이터와 인증서를 클라이언트에 전달 합니다.

<p align="center">
    <img src="/images/2020/12/hand_shaking_2.png" alt="hand_shaking_2" /><br>
</p>

여기까지가 클라이언트와 서버의 `Hand shake` 과정입니다.

### 2. 인증서 검증
서버로 부터 받은 인증서를 발급 할 수 있는 기관을 `CA(Certificate Authority)` 라고 합니다. 이러한 `CA` 는 여러 곳이 존재 하는데 공식적으로 인정 받은 곳만 유효한 인증서를 발급할 수 있는 권한을 가지고 있습니다.

<p align="center">
    <img src="/images/2020/12/CAs.png" alt="cas" title="CA 기관" width="40%"/><br>
    <figurecaption>CA 기관 [출처: Wikipedia]</figurecaption>
</p>

1. 서버로부터 전달 받은 인증서는 CA 의 개인키로 암호화되어 있습니다. 브라우저에 내장된 CA 공개키를 가지고 인증서를 복호화하여 검증합니다.

<p align="center">
    <img src="/images/2020/12/cert_verification.png" alt="cert_verification" />
    <figurecaption>인증서 검증</figurecaption>
</p>

만약 CA 리스트에 해당하는 인증서가 아니라면 주소창에 `Not secure` 라는 경고가 뜨게 됩니다.
<p align="center">
    <img src="/images/2020/12/http_warning.png" alt="http_warning" width="40%"/>
</p>

### 3. 데이터 암호/복호화
핸드 쉐이크와 인증서의 검증 과정을 거치고 서로의 신뢰를 확인한 클라이언트와 서버가 데이터를 주고 받습니다. 이 때는 대칭키와 비대칭키를 활용하여 데이터를 암호/복호화 합니다.

1. 성공적으로 복호화된 인증서에는 서버의 공개키가 포함되어 있습니다.

<p align="center">
    <img src="/images/2020/12/server_pub_key.png" alt="server_pub_key" />
</p>

2. 클라이언트는 앞전에 생성하고 서버로부터 전달 받은 랜덤한 데이터를 가지고 임시 키를 생성 합니다.

<p align="center">
    <img src="/images/2020/12/temp_key.png" alt="temp_key" />
</p>

3. 생성한 임시 키를 서버의 공개키로 암호화하여 서버에게 전달 합니다.

<p align="center">
    <img src="/images/2020/12/send_temp_key.png" alt="send_temp_key" />
</p>

4. 서버가 암호화된 임시 키를 전달 받으면 자신이 가지고 있는 개인키로 복호화 하고 클라이언트와 서버 양측에서는 각각 일련의 과정을 거쳐 동일한 대칭키를 생성합니다.

<p align="center">
    <img src="/images/2020/12/symmetric_key.png" alt="symmetric_key" />
</p>

5. 이렇게 생성된 대칭키는 오직 클라이언트와 서버만이 가지고 있습니다. 이 대칭키를 가지고 데이터를 암호화하여 전달하고, 받은 쪽에서 같은 대칭키로 복호화하여 외부로부터 안전하게 데이터를 주고 받을 수 있습니다.

> 출처: https://www.youtube.com/watch?v=H6lpFRpyl14


