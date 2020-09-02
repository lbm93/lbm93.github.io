---
title: "SSL(SSC,CSR)"
date: 2019-10-27 20:00:00 -0900
categories: [cryptography]
tags: 
- 대칭키
- 비대칭키
- 타원곡선
- RSA
- SSL
- Secure
- 전자서명
- ECDSA
---

**SSL에서 사용하는 SSC와 CSR에 대하여 알아본다.**    
    
# 목록    
1. [**SSL-#1.SSL의 개념**](https://lbm93.github.io/cryptography/cryptography-SSL개념및암호화/)
2. [**SSL-#2.SSL 인증서**](https://lbm93.github.io/cryptography/cryptography-SSL인증서/)
3. [**SSL-#3.SSL에서의 SSC,CSR 개념**](https://lbm93.github.io/cryptography/cryptography-SSL(SSC,CSR)/)
4. [**SSL-#4.SSL동작 방법**](https://lbm93.github.io/cryptography/cryptography-SSL동작방법/)  
  
---

# SSC,CSR
SSL에서 SSC와 CSR은 매우 중요하다. 인증 받을 수 있는 기관이 없을때 즉 가장 최상위 위치에 있을때 자기 스스로 인증하기 위한 방법으로 SSC를 사용하고 서버가 인증기관에게 인증서 받급 요청을 할 떄 필요한 방법이 CSR이다.


# CSR(Certificate Signing Request)
CSR은 인증기관에 인증서 발급 요청을 하는 특별한 ASN.1 형식의 파일로 이루어져있다. 그 안에는 내 공개키 정보와 사용하는 알고리즘 정보등이 들어있다. 개인키는 외부에 유출되면 안되므로 이런 특별한 형식의 파일을 만들어서 인증기관에 전달하여 인증서를 발급받는다. 이 ASN.1 안에 내용에는 국가코드, 도시, 회사명, 부서명, 이메일, 도메인주소 등을 기록해야 한다. 간단한 실습으로 이해해보자.  



1. Private Key 생성
먼저 서버에서 인증기관에게 CSR을 요청하기 위하여 서버의 개인키를 생성한다. 키 생성은 비대칭키 형식으로 만들기 위하여 RSA 방식을 사용하며 2048 bit 길이의 Private Key를 생성한다.

![그림]({{ site.url }}/assets/images/img/blockchain-cryptography/csr1.png)
  

2. Public Key 생성
Pivate Key에 비대칭하는 Public Key를 생성한다.


![그림]({{ site.url }}/assets/images/img/blockchain-cryptography/csr2.png)

3. CSR 생성
서버가 자신의 Private Key를 이용하여 CSR을 생성한다.


![그림]({{ site.url }}/assets/images/img/blockchain-cryptography/csr3.png)

위 명령을 수행하게 되면 국가코드, 도시, 회사명 등등 여러 정보를 입력하라고 한다.


![그림]({{ site.url }}/assets/images/img/blockchain-cryptography/csr4.png)  

이렇게하면 CSR이 만들어진다.




# SSC(Self Signed Certificate)
SSC는 Self Signed Certifacate로 번역해보면 스스로 인증서를 사인한다.  이렇게 번역할 수 있다. 말 그대로 SSC는 스스로 인증서를 만들기 위해 최상위 RootCA로서 자신이 스스스로 인증하는 방법이다. 스스로 인증서에 인증을하기 위해서는 다음과 같은 순서로 이루어져야 한다.

1. RootCA로서 RootCA Private Key를 만든다.
RootCA로서 개인키를 만들 때 RSA 방식으로 만들되 키를 AES 256 bit로 암호화 한다. RootCA의 Private Key이기 때문에 분실되면 안되므로 암호화를 걸어주는 것이다.


![그림]({{ site.url }}/assets/images/img/blockchain-cryptography/ss1.png) 


2. RootCA Private Key를 가지고 CSR을 만든다.
개인키에 암호가 걸려있으므로 암호를 입력해야한다.  
`암호 미입력도 옵션을 빼주면 가능하다`  
여기서는 유효기간이 3650일이되는 CSR을 만들었다.

![그림]({{ site.url }}/assets/images/img/blockchain-cryptography/ss2.png) 


3. 자신이 만든 CSR을 개인키로 서명해 CRT(인증서)를 만든다.
원래 CSR은 인증기관에게 인증서를 발급받기위한 ASN.1형식의 파일이지만 RootCA를 자기 자신으로 했으므로 자기 자신에게 서명을 받아 인증서를 만든다.

![그림]({{ site.url }}/assets/images/img/blockchain-cryptography/ssc3.png)

위 명령이 성공적으로 수행되면 서명이 됬다고 출력 후 RootCA의 Private Key의 비밀번호를 입력하라고 나온다. 성공적으로 수행되면 RootCA의 CRT가 생성이 끝난다.

![그림]({{ site.url }}/assets/images/img/blockchain-cryptography/ssc4.png) 