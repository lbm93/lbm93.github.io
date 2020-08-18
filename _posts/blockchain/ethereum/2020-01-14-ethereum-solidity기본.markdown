---
layout: article
title: "Solidity 기본문법"
date: 2020-01-14 18:00:32 +0900
categories: [development, ethereum-programming]
# description: 
excerpt: "Soilidity 문법에 대하여 알아본다."
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

# Solidity 기본
SmartContract를 작성하기 위하여 Solidity의 기본적인 Syntax들을 정리한다.  

---

# 솔리티디 버전
```s
pragma solidity ^0.4.21
```
  
솔리디티는 컴파일 버전별로 컴파일을 할 수 있다. 즉, 컴파일러 버전을 지정할 수 있다.  


# 스마트 컨트랙트 기본 구조
스마트 컨트랙트의 기본 구조는 아래와 같다.  

```javascript
// 1. 컨트랙트 선언
contract Sample {
    // 2. 상태 변수 선언
    uint256 data;
    address owner;
    
    // 3. 이벤트 정의
    event logData(uint256 dataToLog);
    
    // 4. 함수 변경자 정의
    modifier onlyOwner() {
        if(msg.sender != owner) throw;
        _;
    }
    
    // 5. 생성자
    function Sample(uint256 initData, address initOwner) {
        data = initData;
        onwer = initOwner;
    }
    
    // 6. 함수(메소드) 정의
    function getData() returns (uint256 returned) {
        return data;
    }
    function setData(uint256 newData) onlyOwner {
        logData(newData);
        data = newData;
    }
}
```


# 접근제어 지정자
접근제어자 가시성(Visibility) - 4가지로 구분된다.  

**external**  

 - 외부컨트랙만 접근가능(같은 함수 내부에서 호출되면 안됨)
 - 상태변수는 external 불가능  

**internal**  

 - 컨트랙 내부호출가능
 - 상속받은 컨트랙도 호출가능
 - 상태변수는 디폴트로 internal 선언  

**public**  
 - 컨트랙 내부호출 가능
 - 상속받은 컨트랙도 호출가능
 - 외부컨트랙도 호출가능
   
**private**  
 - 컨트랙 내부만 호출가능
   


# 데이터 위치
솔리디티의 변수는 컨텍스트에 따라 메모리 또는 파일시스템에 저장된다. 함수 매개 변수(리턴 매개 변수 포함)의 기본 위치는 메모리이고, 로컬 변수의 기본 위치는 스토리지이며 상태 변수의 경우 강제로 스토리지에 저장된다.  
  
**storage**  

 - 변수를 블록체인에 영구히 저장
 - 디폴트로 상태변수는 storage  

**memory**  

 - 임시저장변수(휘발성이라는 의미)
 - 디폴트로 매개변수와 리턴값은 memory
 - 배열은 strorage로 선언  


# 함수타입 제어자

 **view**  

 - 데이터를 읽을 수만 있다. read only - '데이터를 읽는다'는 것은 블록체인에서 읽어온다는 말
 - 가스비용 없음
   
**pure**  

 - 데이터 읽지않음
 - 인자값만 활용해서 반환값 정함
 - 가스비용 없음  

**constant**  

 - 0.4.17이전에는 view/pure대신쓰였음 ( 현재는 안쓰는 제어자) payable
 - 함수가 에더(eth)를 받을 수 있게함
 - 가스비용 있음  


# 데이터 타입

**boolean**  

 - 모두 익숙한 불린타입입니다.
 - true/false  

**int**  

 - 모두 익숙한 정수타입입니다.
 - 정수  

**uint**  

 - unsigned int (보통 양수만 사용하기 때문에 주로사용합니다.) address
 - 20bytes 고정 이더리움 계정주소
 - 두개의 멤버소유 balance, transfer bytes
 - 문자열저장(hex로 변환해서 저장해야함)
 - solidity는 string에 최적화 되어있지 않기 때문에 bytes로 사용
 - string 타입은 가스비용이 더 요구됨
 - bytes == bytes1 같은 의미라는 것  

**bytes/string**  

 - 크기 무한, 값타입은 아니고 참조형과 비슷  

**enum**  

 - 모두 익숙한? 열거형입니다.
 - 열거형(문자열배열에 index를 부여한 자료형)
 - 값을 정수형으로 리턴
 - 이름{value1,value2}



# 배열
정적배열과 동적배열이 있다. 

```javascript
contract sample {
    // 동적 배열
    // 배열 리터럴이 보일 때마다 새로운 배열 생성
    // 배열 리터럴이 명시되어 있으면 스토리지에 저장되고, 함수 내부에서 발견되면 메모리에 저장된다.
    int[] myArray = [0, 0];
    function sample(uint index, int value) {
        myArray[index] = value;
        
        // myArray2는 myArray의 포인터를 저장!
        int[] myArray2 = myArray;
        // 메모리 내 고정된 크기의 배열
        uint24[3] memory myArray3 = [1, 2, 99999];
        // myArray4에 메모리에 있는 값을 스토리지에 할당할 수 없으므로 예외가 발생한다.
        // memory를 이용, 메모리에 할당해 주어야 에러가 없다.
        uint8[2] myArray4 = [1, 2];
    }
}
```


# 문자열
```javascript
contract sample {
    // 문자열 리터럴이 있으므로 스토리지에 저장
    string myString = "";
    // 문자열 리터럴이 없어서 myRawString은 memory에 있다.
    bytes myRawString;
    
    function sample(string initString, bytes rawStringInit) {
        // 스토리지
        myString = initString;
        
        // myString2에 myString의 포인터를 저장
        string myString2 = myString
        
        // myString3은 메모리 내의 문자열
        string memory myString3 = "ABCDE";
        
        // 길이 및 내용 변경
        // myString3은 메모리에 위치해서 에러 X
        myString3 = "XYZ";
        myRawString = rawStringInit;
        
        // myRawString의 길이 증가
        myRawString.length++;
        
        // 메모리에 있는 "Example"을 스토리지의 myString에 저장하려 해서 에러 발생
        string myString4 = "Example";
        // 메모리에 있는 매개 변수(initString)을 스토리지의 myString5에 저장하려 해서 에러 발생
        string myString5 = initString;
    }
}
```

# 구조체

함수 외부에서 구조체 메소드 명시: 스토리지 저장  

함수 내부에서 구조체 메소드 명시: 메모리 저장  

```javascript
contract sample {
    struct myStruct {
        bool myBool;
        string myString;
    }
    // s1은 메모리에 있다. (구조체 메소드 명시 x)
    myString s1;
    
    // 스토리지에 인스턴스 저장
    myStruct s2 = myStruct(true, "");
    function sample(bool initBool, stirng initString) {
        // 메모리에 인스턴스 저장
        s1 = myStruct(initBool, initString);
        mySturct memory s3 = myStruct(initBool, initString);
    }
}
```


# 열거형

```javascript
contract sample {
    enum OS { windows, Linux, OSX, UNIX }
    
    OS choice;
    
    function sample(OS chosen) {
        choice = chosen;
    }
    
    function setLinuxOS() {
        choice OS.Linux;
    }
    
    function getChoice() returns (OS chosenOS) {
        return choice;
    }
}
```

# mapping
스토리지에만 사용할 수 있기 때문에 오직 상태 변수로만 선언된다. 매핑은 키-값의 쌍으로 이루어진 해시 테이블이다. 키는 실제로 저장되지 않고, 키의 keccak256 해시 값이 검색에 사용된다. 매핑은 길이를 가지지 않으며, 다른 매핑에 할당될 수 없다.  

```javascript
contract sample {
    mapping (int => string) myMap;
    
    function sample(int key, string value) {
        myMap[key] = value;
        
        // myMap2는 myMap의 참조다
        mapping(int => string) myMap2 = myMap;
    }
}
```

---

# 참고문헌
- <https://d2fault.github.io/2018/03/19/20180319-about-solidity-1/>
- <https://xayahfirst.tistory.com/14>
