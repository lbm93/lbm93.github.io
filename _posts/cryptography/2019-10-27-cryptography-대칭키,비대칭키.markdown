---
layout: article
title: "대칭키와 비대칭키"
date: 2019-10-27 18:00:32 +0900
categories: [development, cryptography]
# description: "웹 통신 프로토콜인 URL, HTTP, SMTP, MIME, FTP 을 정리"
excerpt: "대칭키와 비대칭키"
image:
  teaser: posts/ssl/ssl.png
  credit: 
  creditlink: https://topis.me/48
  #url to their site or licensing
locale: "ko_KR"
# 리플 옵션
comments: true
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
{% include toc.html %}


# 대칭키와 비대칭키
대칭키와 비대칭키방식은 평문을 암호화와 복호화할때 쓰이는 키의 종류로 구분한다.



# 대칭키
**대칭키 암호화 기법은 암호화와 복호화를 하나의 동일한 Private Key로만 수행한다.**

대칭키의 대표적인 종류는 **AES**와 **DES**가 있다.

![symmetrickey]({{ site.url }}/images/img/blockchain-cryptography/symmetrickey.PNG)


예를들어, A와 B가 대칭키 기법을 이용하여 데이터를 주고 받을 때 동일한 Private Key를 A와 B가 공유하게된다. 그럼 A나 B는 하나의 Private Key를 가지고 암호화 복호화를한다. 그 과정을 살펴보자.



# 대칭키를 이용하여 A가 B에게 평문 T를 보낸다 가정

![symmetrickey1]({{ site.url }}/images/img/blockchain-cryptography/symmetrickey1.PNG)

대칭키는 비교적 간단하게 암호화하여 전송할 수 있지만 `대칭키를 Shared하기` 때문에 평문이 안전하지가 않다.

`즉, 보안성이 떨어진다.`



# 비대칭키

비대칭키 암호화 기법은 공개키 암호화 기법이라고도 한다.
이 기법은 암호화와 복호화를 위해 서로 다른 키를 사용한다.
Private Key를 생성해 Private Key를 이용해 Public Key를 만든다. 보통 Public Key를 공개해 암호화할떄 사용하도록 하고 Private Key를 이용해 복호화한다. 그러므로 Private Key는 노출되면 안되는 Key이다.
때로는 Private Key로 암호화를 하기도하고 Public Key로 복호화를하기도 한다. 이렇기 때문에 비대칭키이다.

비대칭키의 대표적인 종류로는 RSA가있다.

![그림3]({{ site.url }}/images/img/blockchain-cryptography/publickey.PNG)


마찬가지로 A와 B가 비대칭키 기법을 이용하여 데이터를 주고 받을 때를 살펴보자.



# 비대칭키를 이용하여 A가 B에게 평문 T를 보낸다 가정

![그림4]({{ site.url }}/images/img/blockchain-cryptography/publickey1.PNG)

비대칭키는 암호화시 PublicKey를 사용하고, 복호화시 Private Key를 사용한다. 복호화키를 감추기때문에 보안의 필수 요소인 기밀성이 좋아진다.


 
# 대칭키와 공개키 기법 비교

*대칭키는 누군가가 대칭키를 획득하게 된다면 암호화된 데이터를 알아낼 수 있다.
*대칭키는 비교적 간단하므로 데이터를 지속적으로 주고 받을 때 사용되면 좋다.
*공개키는 이러한 대칭키의 단점을 보완하기위해 만들어졌다.
*공개키는 암호화키와 복호화키가 따로 있어 로직이 복잡해 데이터 기밀성이 높아진다.
*공개키는 로직이 복잡한만큼 CPU소모가 크므로 지속적으로 데이터를 주고 받는데 사용하는건 좋지 않다.

# 참고문헌
- <https://preamtree.tistory.com/38>