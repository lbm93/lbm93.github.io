---
layout: article
title: "EVM 실행구조"
date: 2020-01-14 18:00:32 +0900
categories: [development, ethereum-programming]
# description: 
excerpt: "Ethereum의 EVM이 어떤 구조로 되어있는지 알아본다."
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

# EVM(Ethereum Virtual Machine) 실행구조
Solidity로 작성된 SmartContract가 컴파일되어 BlockChain에 실려 EVM에서 어떻게 처리되는지 알기 위하여 EVM의 구조에 대하여 알아본다.

---

# EVM 실행구조
아래 그림은 EVM의 실행구조이다.  

![그림](/images/img/blockchain-ethereum/EVM구조/EVM구조.png)

**Program Counter**  
   - 프로그램 카운터는 다음 차례에 실행할 EVM 명령어의 위치를 가리킵니다.  
  

**Program**  
   - 프로그램 영역에는 EVM이 실행할 스마트 컨트랙트의 EVM 명령어 목록을 보관합니다.  
     
  
**Stack**  
   - 연산에 필요한 데이타를 저장하는 공간으로 32바이트 크기의 값들이 저장되며, 최대 2014개가 저장될수 있습니다.    
  
    
**Storage**  
   - 블록체인에 영구적으로 기록하기 위한 저장공간으로 스토리지의 구조는 키/값을 매핑하기위한 구조이며, 키/값은 모두 256비트 크기를 사용합니다. 이더리움의 모든 어카운트는 별도의 스토리지를 가지고 있으며, 다른 어카운트의 스토리지에 있는 데이타를 읽어오거다 값을 쓸수 없습니다.  

  

**Memory**  
   - 함수를 호출하거나 메모리 연산을 수행할때 임시로 사용되는 공간입니다. 데이타를 읽을때는 256비트 단위로 읽도록 제한되어 있지만, 쓸때는 8비트단위나 256비트 단위로도 가능합니다.  
  

**Log**
   - 스마트 컨트랙트가 실행될때 부가적인 정보를 저장하기 위한 공간입니다.   
  
    
**Call Data**
   - 이더리움에 트랜잭션을 요청했을때 전송되는 데이타들이 저장되는 공간입니다.  

---

# 참고문헌
- <https://ihpark92.tistory.com/53>
