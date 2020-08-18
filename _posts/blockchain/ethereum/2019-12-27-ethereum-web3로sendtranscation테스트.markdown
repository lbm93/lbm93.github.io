---
layout: article
title: "Web3.js를 이용한 SendTransaction"
date: 2019-12-27 18:00:32 +0900
categories: [development, ethereum-programming]
# description: 
excerpt: "Web3.js를 이용해서 트랜잭션을 날려보자"
image:
  teaser: posts/blockchain/ethereum.png
  credit: 
  creditlink: 
  #url to their site or licensing
locale: "ko_KR"
# 리플 옵션
comments: true
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
---
{% include toc.html %}

# Web3.js를 이용한 SendTranscation
이번 시간에는 Web3를 이용해 내가 구축한 private Block chain Network에서 작동중인 노드에 연결을 하여 Transaction을 테스트 해본다.

---

# 시작 전 Web3 version
저는 Web3 1.2.4v을 사용했습니다.


# node 실행 옵션 
난 총 3개의 노드를 구성했고 3개의 노드를 static_nodes로 연결하였다. 그리고 각각의 노드의 실행 옵션은 다음과 같다.  

1. 첫번째 노드 실행
- nohup geth --networkid 201951281022 --nodiscover --datadir .  --maxpeers 3 --rpc --rpcaddr "0.0.0.0" --rpcport 3016 --rpccorsdomain "*" --rpcapi "admin,db,eth,miner,net,shh,txpool,personal,web3" --port 30000 --allow-insecure-unlock 2>> ./geth.log &

2. 두번째 노드 실행
- nohup geth --networkid 201951281022 --nodiscover --datadir .  --maxpeers 3 --rpc --rpcaddr "0.0.0.0" --rpcport 3017 --rpccorsdomain "*" --rpcapi "admin,db,eth,miner,net,shh,txpool,personal,web3" --port 30001 --allow-insecure-unlock 2>> ./geth.log &

3. 세번째 노드 실행
- nohup geth --networkid 201951281022 --nodiscover --datadir .  --maxpeers 3 --rpc --rpcaddr "0.0.0.0" --rpcport 3018 --rpccorsdomain "*" --rpcapi "admin,db,eth,miner,net,shh,txpool,personal,web3" --port 30002 --allow-insecure-unlock 2>> ./geth.log &

일단 Web3.js를 사용하기 위해선 JSON_RPC 포트를 열어야한다. 나는 사설 IP를 구성해서 사용하므로 포트포워딩을 통하여 각각의 포트를 3016, 3017, 3018로 구성했다.  

그리고 이번 포스팅의 목적인 sendTranscation을 하기 위해서는 해당 노드의 --unlock 옵션을 줘야하지만 해당 옵션을 주게되면 오류가 발생한다. 이건 보안 업데이트로 인하여 기존 unlock account default가 변경되었다고 한다.

![그림](/images/img/blockchain-ethereum/web3test/sendtransaction1.png)

아마 기존에 --unlock 0 옵션을 주었던 분들은 아마 위의 에러가 발생하면서 실행이 안될 것이다.  

방법은 간단하게 노드 실행시 다음과 같은 옵션을 주면 된다.  

- --allow-insecure-unlock  

그리고 추가적으로 리눅스에서 노드 3개 실행시에 옵션을 더 추가해야할 때 ps로 하나하나 죽이지말고 다음 명령으로 한번에 죽일 수 있다.  

- killall -9 [processname]  

---

# web3로 sendtranscation 하기 
  
먼저 다음과 같이 코드를 작성한다.   

```javascript
//Node1의 Account 잠금 해제하기
web3.personal.unlockAccount(account,'123');
console.log("unlock Account default sucess");

/*Node1의 account[0]이 Node2의 account[0]에게 송금하는 Transcation발생시키기*/

//Node2의 account[0] public Key(address)
let toAddress  = "0xade48d02c948834bd6c5e006b0e3c3024c3e11e7"

//보낼 금액
let sendAmount = web3.toWei(10, 'ether');

let txHash = web3.eth.sendTransaction({
    from: account,
    to: toAddress,
    value: sendAmount
});

console.log(txHash);
```

코드를 간단하게 정리해보자면 먼저 무분별한 transcation발생을 막기 위하여 이더리움은 기본적으로 sendtranscation기능을 잠가둔다. transcation을 발생시킬 account의 잠금을 해제하기 위하여 다음과 같은 명령어로 해제한다.  

- web3.personal.unlockAccount(account,'123');

그리고 sendtransaction에 보내는 account의 address와 받는 account의 address의 주소를 담고 보낼 금액을 적어 transaction을 발생시킨다. transaction 발생 시키면 아래 그림과 같이 해당 transactiond의 해쉬값이 나온다.  

![그림](/images/img/blockchain-ethereum/web3test/sendtransaction2.png)

맨 아래 부분이 발생시킨 transaction에 대한 hash값이다.이 hash 값을 이용하여 transaction을 추적할 수 있다.  
  
geth attach를 사용하여 다음과 같은 명령으로 transaction을 추적한다.  

- eth.getTranscation("transcation의 hash값")

![그림](/images/img/blockchain-ethereum/web3test/sendtransaction3.png)

위 그림을 보면 blocknumber와 blockhash가 null인걸 볼 수 있다.  
  
그 말은 아직 mining이 안되어 체인에 연결이 안되있기 때문이다. 다음과 같은 명령으로 마이닝을 시작한다.  

- miner.start()

마이닝이 정삭적으로 작동하여 체인에 연결이 된다면 blocknumber와 blockhash값이 주어진다.  

![그림](/images/img/blockchain-ethereum/web3test/sendtransaction5.png)

우리는 node1의 0번째 account에서 node2의 0번째 account에 10eth를 전송했기 때문에 node2의 account[0]을 확인 해 본다.  

# transaction을 발생시키기 전

![그림](/images/img/blockchain-ethereum/web3test/sendtransaction4.png)

# transaction이 성공적으로 마이닝된 후 

![그림](/images/img/blockchain-ethereum/web3test/sendtransaction6.png)

4680에서 4700으로 바뀐 이유는 내가 transaction을 두번 발생시켰기 때문이다.  
--

이상으로 이더리움에서 web3.js를 이용해 JSON_RPC 명령 중 sendTranscation에 관한 포스팅였습니다.  