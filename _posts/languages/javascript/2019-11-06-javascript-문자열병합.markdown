---
layout: article
title: "문자열 병합"
date: 2019-11-06 18:00:32 +0900
categories: [development, javascript]
# description: "웹 통신 프로토콜인 URL, HTTP, SMTP, MIME, FTP 을 정리"
excerpt: "문자열 병합"
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

# 문자열 병합
배열의 문자열 요소들을 몇몇 구분자로 합쳐야 할 때가 많다. Array.prototype.join은 매개변수로 구분자 하나를 받고
요소들을 하나로 합친 문자열을 반환한다. 이 매개변수가 생략됐을 때의 기본값은 쉼표이며, 문자열 요소를 합칠 때 정의되지 않은 요소, 삭제된 요소, null, undefined는 모두 빈 문자열로 취급한다.

```javascript
const arr = [1,null,"hello","world",true, undefined];
delete arr[3];          //"world"
arr.join();             //"1,,hello,,true,"
arr.join('');           //"1hellotrue
arr.join('--');         //"1 -- -- hello -- -- true --"
```

# 참고문헌
- *러닝 자바스크립트: ES6로 제대로 입문하는 모던 자바스크립트 웹 개발*