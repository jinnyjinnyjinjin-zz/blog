---
title: "[Hyperledger]Ordering Service(오더링 서비스)"
date: 2020-11-08T15:31:24+09:00
tags: [Hyperledger, Blockchain]
draft: false
---
# **오더링 서비스란?**

이더리움이나 비트코인과 같은 블록체인에서는 허가되지 않은 노드 참가자들도 합의 프로세스에 참여하여 블록을 생성할 수 있다. 그렇기 때문에 이러한 시스템들은 PoW 나 PoS 등과 같은 합의 알고리즘에 기반 해, 분산된 장부를 동기화 시켜 모두가 동일한 데이터를 공유함으로써 무결성을 보장하도록 한다. 하지만 여전히 장부(Ledger) 가 Fork되는 취약점을 가지고 있다.

그에 반해 하이퍼레저 패브릭에서는 `Orderer` 라고 불리는 노드가 전달받은 트랜잭션들을 처리한다. 패브릭은 결정론적인 합의 알고리즘에 의존하기 때문에 피어가 오더링 서비스로 부터 생성된 것으로 확인한 모든 블록은 최종적이고 정확해야 한다. 그렇기에 패브릭 장부들은 다른 블록체인 네트워크에서 처럼 Fork 될 수 없다.

무결성을 높이는 것 외에도, 체인 코드 실행 검증(피어에서 발생)을  오더링으로부터 분리하면 성능과 확장 면에서 패브릭 이점을 제공하여 동일한 노드에서 체인코드를 실행하고 검증 및 오더링을 수행 할 때 발생할 수 있는 병목 현상을 제거한다.

# 역할

1. 검증된 트랜잭션들을 차례로 정리하고 패키징하여 블록에 추가

2. 피어들이 트랜잭션을 검증 및 커밋할 수 있도록 블록 전파

# Consensus

트랜잭션을 충돌 없이 순서대로 처리하도록 하고, 분산 장부의 데이터가 모두 같도록 하는 consensus 종류를 선택하여 설정할 수 있다.

### 1. Solo

- 이름처럼 오직 싱글 오더링 노드만 가능
- consensus 과정 없음
- 장애 취약
- 서비스 개발에 적합하지 않음. 테스트용으로 활용
- <p style="color:deeppink;">Fabric v2.0 부터 Deprecated 됨</p>

### 2. Raft

- Raft 프로토콜 기반의 CFT(Crash Fault Tolerance)
- Leader and Follower 모델
- 각 채널에서 팔로워 노드들에 의해 리더 노드가 선출됨
- Kafka 보다 설정 및 관리가 쉬움

### 3. Kafka

- Raft 와 유사함
- Leader and Follower 노드 설정을 사용 해 CFT 구현
- Kafka 수행을 위해 Zookeeper Ensemble 을 사용함
- Fabric v1.0 부터 사용 가능
- Kafka cluster 관리가 어려움
- <p style="color:deeppink;">Fabric v2.0 부터 Deprecated 됨</p>

이렇게 Kafka 또는 Raft 기반의 Orderer 가 트랜잭션을 패키징하고 블록을 생성하고 전파하는 모든 프로세스를 **오더링 서비스** 라고 한다.

