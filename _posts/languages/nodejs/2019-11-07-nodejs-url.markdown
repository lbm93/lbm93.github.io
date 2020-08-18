---
layout: article
title: "url,querystring module"
date: 2019-11-07 18:00:32 +0900
categories: [development, nodejs]
# description: "웹 통신 프로토콜인 URL, HTTP, SMTP, MIME, FTP 을 정리"
excerpt: "url,querystring module"
image:
  teaser: posts/nodejs/nodejs.PNG
  credit: nodejs
  creditlink: https://nodejs.org/en/
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
- nodejs
- node.js
---
{% include toc.html %}

# URL Module
URL Module은 웹 사이트에 접속하기 위한 사이트 주소 정보를 URL 객체로 만들어준다. 서버에서는 이 정보를 받아 처리할 때 주소와 요청 파라미터를 구분하기위해 URL 객체를 이용한다.

---

# url Module
URL Module에는 다음과같이 두가지 메소드가 있다.

|메소드이름|설명|
|:---|:---|
|parse()|주소 문자열을 파싱하여 URL 객체를 만든다.|
|format()|URL 객체를 주소 문자열로 변환한다.|

```javascript
//URL 모듈 
const url = require('url');

//주소 문자열을 URL 객체로 만들기
const curURL = url.parse('https://search.naver.com/search.naver?sm=top_hty&fbm=0&ie=utf8&query=stevejobs');

//URL 객체를 주소 문자열로 만들기
const curStr = url.format(curURL);

//주소 문자열 출력
console.log(`주소 문자열 : ${curStr}`);

//dir을 이용해 객체를 더 자세하게 출력
console.dir(curURL);

//URL 프로퍼티중 p로 시작하는 프로퍼티를 추출하고 프로퍼티와 프로퍼티 값을 출력
Object.keys(curURL).filter(prop => prop.match(/^p/)).forEach(prop=>console.log(`${prop}: ${curURL[prop]}`));
```

![그림]({{ site.url }}/images/img/language-nodejs/url1.PNG) 


# querystring Module
querystring Module은 URL에서 query 프로퍼티를 구분하여 객체로 만들기 위하여 사용한다. querystring Modual은 두가지 메소드를 제공한다.  

|메소드이름|설명|
|:---|:---|
|parse()| 요청 파라미터 문자열을 파싱하여 요청 파라미터 객체로 만든다.|
|stringfy()|요청 파라미터 객체를 문자열로 변환한다.|

```javascript
var querystring = require('querystring'); //var로 선언해서 hoisting 됌
var param = querystring.parse(curURL.query); //URL 객체중 query부분을 골라냄

console.log(`요청 파라미터 중 query의 값 : ${param.query}`); //요청 파라미터중 query 정보를 출력
console.log(`요청 파라미터 중 sm의 값: ${param.sm}`);   //요청 파라미터중 sm 정보 출력
console.log(`원본 요청 파라미터 : ${querystring.stringify(param)}`);    //요청 파라미터들을 문자열로 변환 후 출력
```

![그림]({{ site.url }}/images/img/language-nodejs/url2.PNG) 

---

# 참고문헌
- *Do it! Node.js 프로그래밍*

