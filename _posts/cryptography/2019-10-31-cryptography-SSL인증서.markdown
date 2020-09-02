---
title: "SSL 인증서 "
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

**SSL이 인증서는 무엇인가 알아본다.**


# 목록    
1. [**SSL-#1.SSL의 개념**](https://lbm93.github.io/cryptography/cryptography-SSL개념및암호화/)
2. [**SSL-#2.SSL 인증서**](https://lbm93.github.io/cryptography/cryptography-SSL인증서/)
3. [**SSL-#3.SSL에서의 SSC,CSR 개념**](https://lbm93.github.io/cryptography/cryptography-SSL(SSC,CSR)/)
4. [**SSL-#4.SSL동작 방법**](https://lbm93.github.io/cryptography/cryptography-SSL동작방법/)  
  
---

# SSL 인증서
SSL 인증서는 클라이언트와 서버간의 통신을 제 3자가 보증해주는 전자화된 문서이다. 클라이언트가 서버에 접속한 직후에 서버는 클라이언트에게 이 인증서 정보를 전달하게 된다. 클라이언트는 이 인증서 정보가 신뢰할 수 있는 것인지를 검증 한 후에 다음 절차를 수행한다.
  
![그림]({{ site.url }}/assets/images/img/blockchain-cryptography/ssl인증서.png)
  
이 인증서를 이용한다면 서로간의 통신 내용을 보호할 수 있고 클라이언트가 접속하려는 서버가 신뢰할 수 있는 서버인지 판단이 가능하다. 또 통신 내용의 악의적인 변경을 방지할 수 있다.  


# SSL 인증서 역할
SSL의 인증서가 하는 역할은 다음과 같다.

1. 클라이언트가 접속한 서버가 신뢰할 수 있는 서버임을 보장한다.
  
    
2. SSL 통신에 사용할 공개키를 클라이언트에게 제공해야 한다.  
  

1번은 서버가 공인된 인증기관(CA)에게 자신의 서버가 안전한 서버임을 인증받아 인증서를 발급받기 때문에 신뢰할 수 있는 서버임을 보장한다.  

2번은 인증서 안에는 서버의 공개키가 들어있기 때문에 클라이언트가 복호화만 한다면 서버의 공개키획득에 문제가 없다.  


# CA(Certicate Authority)
인증서의 역할은 클라이언트가 접속한 서버가 클라이언트가 의도한 서버가 맞는지 보장하는 역할을한다. 이 역할을 수행하는 민간 기업들이 있는데 이 기업들을 CA(Certicate Authority)혹은 Root Certificate라고 부른다.
CA는 아무 기업이나 할 수 없고, 신뢰성이 엄격하게 공인된 기업들만 참여가 가능하다.  

- Symantec (VeriSign, Thawte, Geotrust) with 42.9% market share
- Comodo with 26%
- GoDaddy with 14%
- GlobalSign with 7.7%

SSL을 통해서 암호화된 통신을 제공하려는 서비스는 CA를 통해서 인증서를 구입해야한다. 하지만 우리가 테스트하기엔 문제가 되지 않는다. 그이유는 SSC(Self Signed Certificate)를 이용하면 끝이다. 이에대한 내용은 다음에 다루겠다.  


마지막으로 모든 과정을 정리한 그림을 보며 이해 바란다.

![그림]({{ site.url }}/assets/images/img/blockchain-cryptography/ssl인증서1.png)

# 참고문서
- <https://preamtree.tistory.com/38>
- <https://12bme.tistory.com/80>
