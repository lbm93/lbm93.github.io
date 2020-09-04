---
title: "ERC20 토큰 만들기"
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
lastmod: 2020-09-02 19:00:00 -0900
---

**많은 토큰들이 ERC20을 기반으로 만들어지는데 나도 만들어 보았다.**  

---

# 개발환경
- Blockchain client : Ganache
- Wallet : Metamask
- Smart Contract : Remix


---
이번시간에도 마찬가지로 Ganache를 실행하고 두개의(1번 Account, 2번 Account) Account를 Metamask에 등록하고 Smartcontract를 통해 ERC20 토큰을 발행한다. 그리고 2번 Account로 토큰을 구매해본다.


---
# Account 등록
아래 그림처럼 두개의 public key가 있는 두개의 Account를 Metamask에 등록한다.

![그림](/assets/images/img/blockchain-ethereum/Token생성/account등록.png)


---
# Smartcontract 배포
ERC20 기반의 토큰 발행 contract는 다음과 같다.

```javascript
pragma solidity ^0.4.4;


/*erc20 interface*/
contract Token {


    function totalSupply() public view returns (uint256 supply) {}


    function balanceOf(address _owner) public view returns (uint256 balance) {}


    function transfer(address _to, uint256 _value) public returns (bool success) {}

   
    function transferFrom(address _from, address _to, uint256 _value) public returns (bool success) {}


    function approve(address _spender, uint256 _value) public returns (bool success) {}


    function allowance(address _owner, address _spender) public view returns (uint256 remaining) {}

    event Transfer(address indexed _from, address indexed _to, uint256 _value);
    event Approval(address indexed _owner, address indexed _spender, uint256 _value);

}

contract StandardToken is Token {

    function transfer(address _to, uint256 _value) public returns (bool success) {

        if (balances[msg.sender] >= _value && _value > 0) {
            balances[msg.sender] -= _value;
            balances[_to] += _value;
            Transfer(msg.sender, _to, _value); //(이벤트)블록체인에 브로드캐스트
            return true;
        } else { return false; }
    }


    function balanceOf(address _owner) public view returns (uint256 balance) {
        return balances[_owner];
    }



    mapping (address => uint256) balances;


   uint256 public totalSupply;
}

contract TestToken is StandardToken { 

    string public name;                   // Token 이름
    uint8 public decimals;                // ETH의 wei,Gwei와 같이 해당 코인의 최소 단위를 설정
    string public symbol;               
    string public version = 'H1.0'; 
    uint256 public Token_OneEthCanBuy;    // 1ETH로 살 수 있는 토큰 양
    uint256 public totalEthInWei;         // 토큰을 구매하면서 쌓이는 총 ETH 양(WEI 단위)
    address public fundManager;           // 토큰 최초 발행자

    // This is a constructor function 
    // which means the following function name has to match the contract name declared above
    constructor () public {
        balances[msg.sender] = 1000000000000000000000;
        totalSupply = 1000000000000000000000;
        name = "TK";
        decimals = 18;
        symbol = "TTN"; 
        Token_OneEthCanBuy = 10;   
        fundManager = msg.sender;  
    }

    function() external payable{
        totalEthInWei = totalEthInWei + msg.value; 
        uint256 amount = msg.value * Token_OneEthCanBuy; //구매자가 사려하고 하는 토큰 양
        require(balances[fundManager] >= amount);

        balances[fundManager] = balances[fundManager] - amount; //토큰 발행자의 토큰 차감
        balances[msg.sender] = balances[msg.sender] + amount; //토큰 구매자에게 토큰 전달

        Transfer(fundManager, msg.sender, amount); // 블록체인에 브로드캐스트

 
        fundManager.transfer(msg.value);                               
    }
}
```

위 코드를 보면 맨 위에 Token이라는 contract가 interface처럼 형성되어있다. 


그리고 StandardToken, TestToken의 contract가 있다. StandardToken은 Token을 상속받고 TestToken은 StandardToken을 상속받는다. 


그리고 TestToken에는 생성자가 있으므로 배포시 가장 먼저 실행되는 컨트랙트는 TestToken이 된다. 


생성자를 보면 공급할 토큰의 총 량이 1000000000000000000000가 있고 토큰의 이름은  TTN 토큰이 된다. 


배포자가 컨트랙트 배포시 생성자가 실행되며 토큰이 생성되고 다른 Account들은 이름이 없는 fallback 함수로 토큰을 구매하게 된다.



아래 그림에 빨간 줄 처럼 contract중 TestToken을 누르고 depoly한다.

![그림](/assets/images/img/blockchain-ethereum/Token생성/토큰디플로이.png)


---
# Token 등록
컨트랙트를 제대로 배포하였다면 저번 시간에 했던 것 처럼 토큰을 등록하고 확인해본다.

![그림](/assets/images/img/blockchain-ethereum/Token생성/TTN토큰등록1.png)
![그림](/assets/images/img/blockchain-ethereum/Token생성/TTN토큰등록2.png)


그림을 확인해보면 1000개의 토큰이 들어온 것을 알 수 있다. 그런데 위에서 코드에선 1000000000000000000000개를 발행했는데 왜 1000개 밖에 없을까? 그것은 바로 decimal이라는 변수때문이다 이 변수가 실수 단위를 정수로 바꾸어 뒤에 0 18개를 생략해버리기 때문이다.


---
# Token 구매
다른 계좌로 Token을 구매해본다. Metamask로 Accoount7이라는 계좌를 선택하고 아래 그림처럼 배포한 컨트랙트의 주소를 복사한다.

![그림](/assets/images/img/blockchain-ethereum/Token생성/토큰구매1.png)

그리고 Metamask에서 전송 버튼을 눌러 복사한 컨트랙트 주소에 30eth를 보내 토큰을 구매한다.

![그림](/assets/images/img/blockchain-ethereum/Token생성/토큰구매2.png)
![그림](/assets/images/img/blockchain-ethereum/Token생성/토큰구매3.png)

그리고 이 토큰을 확인해보려면 먼저 컨트랙트 주소를 이용해 토큰을 사용자 등록해야 한다. 이 과정은 전에도 많이 해보았으니 스킵하고 다음으로 토큰을 확인해본다!

![그림](/assets/images/img/blockchain-ethereum/Token생성/토큰구매4.png)

30eth에 토큰이 300개인 이유는 위에 코드에서 Token_OneEthCanBuy = 10; 라고 정의해놨기 때문에 1eth 당 10개의 토큰을 구매할 수 있기 때문이다.


마지막으로 토큰 판매자의 계좌를 살펴보면 Account7이 보낸 30eth가 들어와있고 300개의 토큰이 빠진걸 확인할 수 있다.

![그림](/assets/images/img/blockchain-ethereum/Token생성/토큰구매5.png)


---
# 마무리하며
ethereum scan에 있는 많은 토큰들이 ERC20을 기반으로 만들어져 ICO를 통해 펀딩되었다. 나도 직접 ERC20 기반으로 만들어봤지만 어렵지 않은 작업이라고 생각했다. 생각보다 어렵게 만들어진게 아니구나 생각했고 쉽게 만들만큼 많이 만들어진 토큰들이 신뢰할 수 있을까? 라는 생각을 하게 되었다. 이상!!

