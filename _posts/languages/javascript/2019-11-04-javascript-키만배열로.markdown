---
layout: article
title: "객체의 키만 배열로 가져오기"
date: 2019-11-04 18:00:32 +0900
categories: [development, javascript]
# description: "웹 통신 프로토콜인 URL, HTTP, SMTP, MIME, FTP 을 정리"
excerpt: "객체의 키만 배열로 가져오기"
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

# 객체의 키만 배열로 가져오기
객체의 키만 배열로 가져오기 위해 사용되는 메서드는 Object.keys이다.

```javascript
const o = {apple : 1, xochitl : 2, ballon : 3, guitar : 4, xylophone : 5 };

Object.keys(o);
```
위 코드는 단순히 객체의 키값만 가져오는 예제이고, 다음 예제는 filter와 mathch와 forEach를 통한 특정 프로퍼티만 가져오는 예제이다.

```javascript
const o = {apple : 1, xochitl : 2, ballon : 3, guitar : 4, xylophone : 5 };

Object.keys(o).filter(prop => prop.match(/^x/)).forEach(prop=>console.log(`${prop}:${o[prop]}`));
```
위 코드 수행시 키가 x로 시작되는 프로퍼티만 가져와 키와 value를 출력한다.

# 참고문헌
- *러닝 자바스크립트: ES6로 제대로 입문하는 모던 자바스크립트 웹 개발*