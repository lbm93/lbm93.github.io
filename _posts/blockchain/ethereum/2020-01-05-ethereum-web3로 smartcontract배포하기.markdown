---
title: "Web3.js를 이용한 SmartContract 배포"
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
lastmod: 2020-01-05 19:00:00 -0900
---

**Web3.js를 이용해 smartContract를 배포해보고 배포한 Contract를 call해서 사용해보기까지 알아봅니다.**  

---

# 시작 전 Web3 version
저는 Web3 1.2.4v을 사용했습니다.


시작하기 앞서 간단하게 과정을 살펴봅니다. Contract를 배포하는 과정과 사용하는 과정은 다음과 같습니다.  

* Contract 배포 순서  
Contract 작성 -> 컴파일 -> Contract 객체 생성(ABI포함) -> Contract 인스턴스를 이용해 deploy 생성 후 send 
  
* Contract 사용 순서  
Contract 객체 생성(ABI, 배포된 Contract Address) -> Method 호출   



# SmartContract 작성
먼저 SmartContract를 Solidity라는 언어로 작성 해줍니다. 저는 편의를 위해 Remix에서 작성했습니다. Remix는 <https://remix.ethereum.org> 에서 사용이 가능합니다. 먼저 아래의 그림처럼 간단하게 Contract를 작성해 줍니다.  

![그림](/assets/images/img/blockchain-ethereum/web3test/Contract배포/TestContractCode.png)

위 코드는 간단히 HelloWorld 메소드 호출 시 greeting이라는 변수에 HelloWorld가 들어가고 say 메소드 호출 시 greeting 값을 불러오는 코드입니다.  


# SmartContract 컴파일
작성한 SmartContract를 컴파일하는 방법은 두 가지가 있습니다.

1. Linux에서 solidity 컴파일러 설치  
> npm install -g solc  
  
2. Remix에서 컴파일하기  
Remix에서 컴파일을 하는 방법은 먼저 컴파일러 버전을 선택해줍니다. 아래 그림과 같이 setting 탭에서 컴파일러 버전을 선택하면 됩니다. `컴파일러 버전은 Contract작성 시 맨 위에 기입했던 버전과 동일해야 합니다.` 

![그림](/assets/images/img/blockchain-ethereum/web3test/Contract배포/CompileVersion.png)

버전 선택이 되었다면 아래 그림처럼 Compile 탭에서 컴파일을 시작합니다.  

![그림](/assets/images/img/blockchain-ethereum/web3test/Contract배포/RemixCompile.png)

정상적으로 컴파일이 완료되었다면 run탭에 아래 그림과 같이 컴파일된 Contract를 확인할 수 있습니다.  

![그림](/assets/images/img/blockchain-ethereum/web3test/Contract배포/Compiled.png)


# SmartContract의 ByteCode와 ABI얻기
컴파일이 제대로 성공했다면 바로 위의 그림에서 밑에 HelloWorld라고 생긴걸 확인할 수 있습니다. 여기서 옆에 Details를 누르면 아래 그림처럼 Contrat의 ByteCode와 ABI를 얻을 수 있습니다.  

![그림](/assets/images/img/blockchain-ethereum/web3test/Contract배포/ByteCode.png)

![그림](/assets/images/img/blockchain-ethereum/web3test/Contract배포/ABI.png)

위의 ABI같은 경우에는 옆에 클립보드로 복사를누르면 내용이 복사됩니다.  

우리는 이제 이 데이터를가지고 web3.js를 이용해 Contract를 배포할 것 입니다.  



# SmartContract 배포하기
SmartContract를 배포한다는 것은 블록체인 상에 컴파일한 SmartContract의 ByteCode를 Transaction에 실어 블록에 포함시킨 상태라고 말할 수 있습니다.  



# ByteCode와 ABI 파일로 읽어 들이기
먼저 위에서 얻은 ByteCode와 ABI를 각각 파일로 저장하고 파일을 읽어 들입니다.  

```javascript
const ByteCode_File = JSON.parse(fs.readFileSync('../contract/HelloWorld/bytecode','utf-8'));

const ABI_File = JSON.parse(fs.readFileSync('../contract/HelloWorld/ABI', 'utf-8'));
```


# Contract 객체 생성
Contract 객체를 생성합니다. 이 때 최초 SmartContract를 Deploy하기 때문에 생성자에 인자값으로 ABI코드만 넣어줍니다.  

```javascript
//SmartContract의 ABI를 담은 Contract 객체 생성
const MyContract = new web3.eth.Contract(ABI_File);
```



# Account unlock 하기
Contract를 배포하는 것은 Transaction을 발생 시키는 것이기 때문에 무분별한 Transcation 방지를 위해 막아둔 lock을 풀어줍니다.

```javascript
web3.eth.personal.unlockAccount(Account,'비밀번호', 10000).then(console.log('Account unlock'));
```

비밀번호라 적혀있는 부분에는 Account의 PrivateKey를 즉 비밀번호를 넣어주면 됩니다. 위 방식은 실행 시 보안이 매우 안좋다고 나오므로 테스트용으로만 사용합니다.  



# Contract Deploy하기
Contract를 Deploy하기 위해선 아래와 같이 작성합니다.  

```javascript
//deploy에 data : contract의 bytecode를 넣고 deploy
//arg는 Hex로 표현해야 한다는 Waring 발생 toHex로 해결
const deploy = MyContract.deploy({
    data : '0x' + bytecode
}).send({
        from : web3.utils.toHex(Account),
        gas: web3.utils.toHex(1000000),
        gasPrice : web3.utils.toHex(web3.utils.toWei('10', 'gwei'))
    }, (err, transactionHash) =>{
        console.info('transactionHash', transactionHash);
    }).on('error', (error)=>{
        console.info('error', error);
    }).on('transactionHash', (transactionHash) =>{
        console.info('transactionHash', transactionHash);
    }).on('receipt', (receipt) =>{
        console.info('receipt', receipt);
    }).on('confirmation', (confirmation) =>{
        console.info('confirmation', confirmation);
    }).then((newContractInstance) =>{
        console.info('newContractInstance', newContractInstance);
    });
```

위 코드에서 data 부분에 bytecode 앞에는 반드시 '0x'를 붙여줍니다. 저는 여기서 web3.utils.toHex를 해주었다가 한참 고생했습니다. 이유를 생각해보니 bytecode자체가 16진수로 이루어진 수 이기때문에 앞에 또 toHex를 해버리면 이상한 값으로 발생해 Contract 발행 시 data가 길어질 수 있습니다. 그 결과가 아래에 그림에서 확인할 수 있습니다.  

> toHex를 해버린 ByteCode

![그림](/assets/images/img/blockchain-ethereum/web3test/Contract배포/FaildByteCode.png)

위 그림처럼 Bytecode가 길어진다면 당연히 이 Bytecode를 다 블록에 포함시키기 위해서는 더 많은 비용의 가스가 소모되겠죠?  
  

> '0x' 를 붙여준 ByteCode

![그림](/assets/images/img/blockchain-ethereum/web3test/Contract배포/SuccessByteCode.png)

그리고 gas 부분은 gaslimit이 되는 부분으로 너무 적게도 너무 많게도 적으면 안된다. 그러면 배포시 에러가 발생한다. 정상적으로 배포가 되어 채굴이 된다면 아래그림처럼 Transaction Hash값이 나오고 이 Transaction Hash를 확인 해 볼 수 있다.  

![그림](/assets/images/img/blockchain-ethereum/web3test/Contract배포/Contract배포.png)

![그림](/assets/images/img/blockchain-ethereum/web3test/Contract배포/배포된Contract확인.png)


# 베포 한 SmartContract 사용해 보기
배포 한 SmartContract를 사용하기 위해서는 반드시 BlockChain에 Contract의 ByteCode가 포함된 상태여야한다. 정상적으로 배포가 되었다면 아래 그림처럼 SmartContract의 주소와 JSONINTERFACE를 확인해 볼 수 있다.

![그림](/assets/images/img/blockchain-ethereum/web3test/Contract배포/ContractAddress.png)

위에서 우리는 JSONINTERFACE 를 확인할 수 있다. 이 것은 ABI로 이 인터페이스를 통해 우리가 어떤 메소드가 존재하는지 확인하고 SmartContract를 이용하게 해주는 가이드라고 보면 된다.  

우리는 위에서 say라는 함수를 호출해 greeting이라는 변수의 값을 확인할 것이다.  

코드는 다음과 같다.  

```javascript
//컨트랙트의 주소
const ContractAddress = '0x14f4Ca12EFe6dEaB180525e603DC46B46a1e67Cf';
const ABI_File = JSON.parse(fs.readFileSync('../contract/HelloWorld/ABI', 'utf-8'));

const TestContract = new web3.eth.Contract(ABI_File,ContractAddress);

TestContract.methods.say().call().then(console.log);
```

위 코드를 수행하게 되면 다음과 같은 결과를 확인할 수 있다.  


![그림](/assets/images/img/blockchain-ethereum/web3test/Contract배포/Call.png)


> ps.여기까지 web3를 이용해 SmartContract를 배포하고 사용해보는 시간을 가졌다. 정말 복잡하고 web3.js버전이 자주 바뀌어 안되는 부분도 찾아가면서 하느라 시간이 많이 걸렸습니다 !! 다들 열심히 하십쇼!!

---

# 참고문헌
- <https://web3js.readthedocs.io/en/v1.2.4/web3-eth-contract.html#methods-mymethod-call>
- <https://medium.com/@drhot552/이더리움-블록체인-web3-js로-스마트컨트랙트-실행하기-a9fe0be47dc1>
- <https://opentutorials.org/module/3136/19273>




