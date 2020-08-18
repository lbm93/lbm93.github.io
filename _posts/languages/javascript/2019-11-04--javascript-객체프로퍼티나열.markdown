---
layout: article
title: "객체프로퍼티 나열"
date: 2019-11-04 18:00:32 +0900
categories: [development, javascript]
# description: "웹 통신 프로토콜인 URL, HTTP, SMTP, MIME, FTP 을 정리"
excerpt: "객체프로퍼티 나열"
image:
  teaser: posts/javascript/javascript.png
  credit: 
  creditlink: 
  #url to their site or licensing
locale: "ko_KR"
# 리플 옵션
comments: true
tags:
- web protocols
- web developer
- web development basic
- 웹 개발 기초
- 웹 개발자
- 웹 개발자를 위한
- 웹 개발자를 위한 웹 프로토콜
- 웹 프로토콜
- HTTP
- MIME
- SMTP
- URL
- javascript
- 자바스크립트
---
{% include toc.html %}

# 객체프로퍼티 나열
그동안 객체 프로퍼티를 나열할 떄 for..in을 주로 사용했다.

```javascript
const SYM = Symbol();

const o = {a:1, b:2, c:3, [SYM]:4};

for(let prop in o){
    if(!0.hasOwnProperty(prop)) continue;
    console.log(`${prop} : ${o[prop]}`);
}
```

hasOwnProperty는 상속된 프로퍼티가 for..in에 나타날 위험을 제거하기 위해 사용한다.  
자주 사용하는 습관을 기르자.  

# 참고문헌
- *러닝 자바스크립트: ES6로 제대로 입문하는 모던 자바스크립트 웹 개발*