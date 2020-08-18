---
layout: article
title: "SSL 개념 및 암호화 방법"
date: 2019-10-31 18:00:32 +0900
categories: [development, cryptography]
# description: "웹 통신 프로토콜인 URL, HTTP, SMTP, MIME, FTP 을 정리"
excerpt: "SSL 개념 및 암호화 방법"
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

# SSL 개념
SSL(Secure Socket Layer) Protocol은 NetScape사에서 웹서버와 웹 브라우저 사이의 보안을 강화화기 위하여 만들었다. SSL은 Certificate Authority(CA)라 불리는 인증기관으로부터 서버와 클라이언트의 인증을 하는데 사용된다. SSL은 대칭키와 공개키의 장점을 이용한 통합 암호화 방식이다.  



# SSL 구조
SSL은 웹서버와 웹 브라우저 사이의 보안을 강화화기 위한 기술로 사용된다.  
즉 웹서버와 웹 브라우저가 통신을 위해 사용하는 HTTP 프로토콜을 강화한 것이 바로 HTTPS이다.이 HTTP뒤에 붙은 S가 SSL이란 말이다.  그렇다면 이 SSL이 어떻게 생겼는지 확인해 보자.

<center>![그림](/images/img/blockchain-cryptography/sslarchitecture.png)</center>


네트워크를 공부해본 사람은 딱 보면 알 것이다.  
OSI 7계층, Application 계층에서 사용되는 HTTP 프로토콜에 SSL 프로토콜이 덧 붙은 것이다.



# SSL 암호화
SSL의 핵심은 암호화이다. SSL은 대칭키 암호화 방식과 공개키 암호화 방식을 혼용해서 사용한다.이 암호화들을 간단하게 실습을 하면서 이해해본다.  



# 대칭 키
1. 평문을 생성한다.<br>
![그림]({{ site.url }}/images/img/blockchain-cryptography/symmetrickeyexam1.png) <br>
  
2. 평문을 DES3 방식으로 암호화한다. 이때 사용되는 비밀번호가 바로 대칭 키 이다.  
여기선 비밀번호를 간단하게 123 이라고 지정하겠다.<br>
`즉, 여기서 키라는 것은 암호를 만들 때 일종의 비밀번호이다.` <br>
![그림]({{ site.url }}/images/img/blockchain-cryptography/symmetrickeyexam2.png) <br>
  
3. DES3 방식으로 만들어진 암호문을 복호화한다.  
이때 대칭 키(123)을 입력하여야 한다. <br>
![그림]({{ site.url }}/images/img/blockchain-cryptography/symmetrickeyexam3.png) <br>
  
4. 내용확인
처음 암호화하기전 평문이 맞는지 확인한다. <br>
![그림]({{ site.url }}/images/img/blockchain-cryptography/symmetrickeyexam4.png) <br>
  
  
# 공개 키
1. RSA방식의 1024bit 길이의 private key를 생성한다. <br>
![그림]({{ site.url }}/images/img/blockchain-cryptography/publickey2.png) <br>   
  
2. 생성한 private key로 비대칭 키 public key 생성 <br>
![그림1]({{ site.url }}/images/img/blockchain-cryptography/publickey3.png) <br>
  
여기서부터 B가 A에게 'coding everybody'를 보낸다 가정,<br>
  
3. 평문 생성  <br>
![그림1]({{ site.url }}/images/img/blockchain-cryptography/publickey4.png) <br> 
  
4. B는 A의 public key로 평문을 암호화해 file.ssl을 생성 후 A에게 전달 <br>
![그림1]({{ site.url }}/images/img/blockchain-cryptography/publickey5.png)  <br>
  
5. A는 private key로 file.ssl을 복호화한다.<br>
![그림1]({{ site.url }}/images/img/blockchain-cryptography/publickey7.png)   <br>
  
6. A는 평문을 확인한다. <br>
![그림1]({{ site.url }}/images/img/blockchain-cryptography/publickey8.png)<br>

# 참고문서
- <https://12bme.tistory.com/80>