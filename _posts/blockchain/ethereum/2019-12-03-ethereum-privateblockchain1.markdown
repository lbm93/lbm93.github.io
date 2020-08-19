---
title: "Private blockchain 만들기"
date: 2020-05-27 20:00:00 -0900
categories: [ethereum]
tags: 
- blockchain
- 블록체인
- blockchain client
- 블록체인 클라이언트
- geth
- 게스
- Geth 환경설정
- solidity
- 솔리디티
- smartcontract
- 스마트컨트랙트
lastmod: 2019-12-03 19:00:00 -0900
---

**Test 환경을 구성하기위해 Private Block chain을 만드는 과정을 담는다.**  

---


# Private Block chain 만들기(Geth 백그라운드 실행)
 이번 포스트에서는 Geth를 백그라운드로 실행시키고 실행 옵션에 rpc options를 추가시켜 web3로 간단하게 이용가능하도록 Geth 실행에 관련한 내용을 적는다.

---

# Geth Background로 실행
기본적인 백그라운드 실행 명령은 다음과 같다.  

- nohup geth --networkid 201951281022 --nodiscover --maxpeers 0 --datadir . --mine --minerthreads 1 --rpc 2>> ./geth.log &  

만약 위 명령을 실행 중 ps가 자동으로 종료되면 geth.log를 확인해 봐야할 것이다. 그러나 대부분 --mine , minerthreads 때문일 것이다. 그 이유는 최초 실행 시 만들어진 Account가 없기 때문에 채굴도 못하고 채굴시 받을 balance를 지정할 Account가 없기 때문이다.  

`최초 실행시 다음과 같이 옵션을 줘서 실행한다.`  

- nohup geth --networkid 201951281022 --nodiscover --maxpeers 0 --datadir . --rpc 2>> ./geth.log & 

위 두 옵션 중 하나라도 실행이 됬으면 이제 제대로 작동하는지 rpc 접속으로 확인해보자.  

![그림](/assets/images/img/blockchain-ethereum/privateblockchain/privateblockchain1.PNG)  

![그림](/assets/images/img/blockchain-ethereum/privateblockchain/privateblockchain2.PNG)  

두번째 그림과 같이 나오면 Geth가 정상적으로 동작중인 것을 확인할 수 있다. 위 명령어로 클라이언트를 실행할 시 대신 rpc 명령어를 사용할 수 없다. 예를 들어 새로운 Account를 만들기 위해 사용되는 personal.newAccount()같은 메소드를 사용할 수 없다. 해당 메소드같이 다른 메소드들을 사용하려면 실행 시 다음과 같은 옵션들을 줘야 한다.  


- nohup geth --networkid 201951281022 --nodiscover --maxpeers 0 --datadir . --mine --minerthreads 1 --rpc --rpcaddr "0.0.0.0" --rpcport 8545 --rpccorsdomain "*" --rpcapi "admin,db,eth,miner,net,shh,txpool,personal,web3" 2>> ./geth.log &

> --rpc : 옵션으로 RPC 서버를 활성화시킨다.  

> --rpcaddr "0.0.0.0" : HTTP-RPC 서버의 수신 IP를 지정한다. Default는 localhost이고, 0.0.0.0을 지정하면 외부 인터페이스에 대한 접근도 가능하다.  

> --rpccorsdomain "*" : 자신의 노드에 RPC로 접속할 IP 주소를 지정한다. 쉼표로 구분해서 여러 개를 지정할 수 있고, "*"로 지정하면 모든 IP에서 접속을 허용한다.  

> --rpcapi "admin,db,eth,debug,miner,net,shh,txpool,personal,web3" : RPC를 허가할 명령을 지정한다. Default는 "eth, net, web3" 이다.

RPCAPI는 [RPCAPI](https://github.com/ethereum/go-ethereum/wiki/Management-APIs) 에서 확인하자. 


백그라운드로 실행되는 Geth들은 RPC로 접근이 가능하다. RPC 명령어는 다음과 같다.  

- geth attach http://localhost:8545


# 3개의 Node 생성
나는 test용으로 3개의 Node를 생성해서 각각, Background로 실행한다. 3개의 Node를 생성하는 방법은 간단하다. 3개의 directroy를 생성해서 각각 같은 genesis 파일을 만들고 다음 명령어를 수행한다.  

- geth --datadir . init genesis

genesis 파일은 아래 링크된 포스트에서 작성한 그대로 사용한다. 참고 자료는 [genesis](https://byeongminlee.github.io/blockchain/2019/11/28/blockchain-ethereum-%EC%9D%B4%EB%8D%94%EB%A6%AC%EC%9B%80%ED%99%98%EA%B2%BD%EC%84%A4%EC%A0%952/) 에서 확인하자.


위 과정을 통해 3개의 Node를 생성했으면 아래 그림처럼 확인할 수 있다.  

![그림](/assets/images/img/blockchain-ethereum/privateblockchain/privateblockchain3.PNG)  

그리고 각각의 Node에 RPC로 접근해 다음과 같은 명령어로 Account들을 생성해준다.  

- personal.newAccount()

위 명령을 수행하면 비밀번호를 입력하라고 나오는데 비밀번호는 간단한게 123으로 지정한다.  이 비밀번호가 Private Key로 사용되니 까먹지 말 것!!  

![그림](/assets/images/img/blockchain-ethereum/privateblockchain/personalnewaccount1.PNG)
![그림](/assets/images/img/blockchain-ethereum/privateblockchain/personalnewaccount2.PNG)
![그림](/assets/images/img/blockchain-ethereum/privateblockchain/personalnewaccount3.PNG)



# 3개의 Node를 연결
위에서 3개의 Node를 만들었다. 각각의 노드의 디렉터리로 이동해 다음과같이 실행 명령을 준다.  

1. 첫번째 노드 실행
- nohup geth --networkid 201951281022 --nodiscover --datadir . --mine --minerthreads 1 --maxpeers 3 --rpc --rpcaddr "0.0.0.0" --rpcport 8545 --rpccorsdomain "*" --rpcapi "admin,db,eth,miner,net,shh,txpool,personal,web3" --port 30000 2>> ./geth.log &

2. 두번째 노드 실행
- nohup geth --networkid 201951281022 --nodiscover --datadir . --mine --minerthreads 1 --maxpeers 3 --rpc --rpcaddr "0.0.0.0" --rpcport 8546 --rpccorsdomain "*" --rpcapi "admin,db,eth,miner,net,shh,txpool,personal,web3" --port 30001 2>> ./geth.log &

3. 세번째 노드 실행
- nohup geth --networkid 201951281022 --nodiscover --datadir . --mine --minerthreads 1 --maxpeers 3 --rpc --rpcaddr "0.0.0.0" --rpcport 8547 --rpccorsdomain "*" --rpcapi "admin,db,eth,miner,net,shh,txpool,personal,web3" --port 30002 2>> ./geth.log &  
  

내 시스템에서 사용하는 노드 실행 명령
1. 첫번째 노드 실행
- nohup geth --networkid 201951281022 --nodiscover --datadir .  --maxpeers 3 --rpc --rpcaddr "0.0.0.0" --rpcport 3016 --rpccorsdomain "*" --rpcapi "admin,db,eth,miner,net,shh,txpool,personal,web3" --port 30000 --allow-insecure-unlock 2>> ./geth.log &

2. 두번째 노드 실행
- nohup geth --networkid 201951281022 --nodiscover --datadir .  --maxpeers 3 --rpc --rpcaddr "0.0.0.0" --rpcport 3017 --rpccorsdomain "*" --rpcapi "admin,db,eth,miner,net,shh,txpool,personal,web3" --port 30001 --allow-insecure-unlock 2>> ./geth.log &

3. 세번째 노드 실행
- nohup geth --networkid 201951281022 --nodiscover --datadir .  --maxpeers 3 --rpc --rpcaddr "0.0.0.0" --rpcport 3018 --rpccorsdomain "*" --rpcapi "admin,db,eth,miner,net,shh,txpool,personal,web3" --port 30002 --allow-insecure-unlock 2>> ./geth.log &

---

3개의 Node를 연결하는 방법은 여러가지가 있지만 나는 static-nodes.json 방식을 사용한다 이 방식은 각각의 enode들을 첫번째 Node에 파일로 static하게 만들어 놓으면 실행 시 자동으로 1번 Node를 통해 연결이 되는 방식이다. RPC로 각각의 노드에 접근해서 다음 명령어로 enode를 확인한다.  

- admin.nodeInfo.enode

1. 첫번째 노드 enode 
![그림](/assets/images/img/blockchain-ethereum/privateblockchain/enode1.PNG)

2. 두번째 노드 enode 
![그림](/assets/images/img/blockchain-ethereum/privateblockchain/enode2.PNG)

3. 세번째 노드 enode 
![그림](/assets/images/img/blockchain-ethereum/privateblockchain/enode3.PNG)


다음으로는 첫번째 Node의 디렉터리로 이동해 static-nodes.json 파일을 만든다. 이때 keystore와 같은 위치에 만들어야 한다. static-nodes.json을 만들고 위에서 확인한 노드들을 다음과같이 입력해준다.  

![그림](/assets/images/img/blockchain-ethereum/privateblockchain/staticnodesjson1.PNG)  

그리고 다시 세 노드를 실행하고 admin.peers로 확인을 해보자.  

1. 첫번째 노드 admin.peers 확인
![그림](/assets/images/img/blockchain-ethereum/privateblockchain/staticnodesjson2.PNG)

2. 두번째 노드 admin.peers 확인
![그림](/assets/images/img/blockchain-ethereum/privateblockchain/staticnodesjson3.PNG)

3. 세번째 노드 admin.peers 확인
![그림](/assets/images/img/blockchain-ethereum/privateblockchain/staticnodesjson4.PNG)


이렇게 함으로써 나만의 Private Block Chain 네트워크를 구성했다.   

---

# 참고문헌
- [Genesis옵션링크](https://byeongminlee.github.io/blockchain/2019/11/28/blockchain-ethereum-%EC%9D%B4%EB%8D%94%EB%A6%AC%EC%9B%80%ED%99%98%EA%B2%BD%EC%84%A4%EC%A0%952/)
- [RPCAPI문서링크](https://github.com/ethereum/go-ethereum/wiki/Management-APIs)