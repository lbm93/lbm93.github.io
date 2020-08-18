---
layout: article
title: "Dir 사용법"
date: 2019-11-11 18:00:32 +0900
categories: [development, nodejs]
# description: "웹 통신 프로토콜인 URL, HTTP, SMTP, MIME, FTP 을 정리"
excerpt: "Dir 사용법"
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

# fs 모듈로 새 디렉터리 생성,삭제
fs 모듈은 파일과 디렉터리를 다루는 여러 가지 메소드를 포함하고 있다. 다음은 디렉터리를 만들었다 삭제하는 예제이다.

```javascript

const fs = require('fs');

fs.mkdir('./docs', 0666, (err)=>{
    if(err) throw err;
    console.log('새로운 docs 폴더를 만들었다.');
});

fs.rmdir('./docs', (err)=>{
    if(err) throw err;
    console.log('docs 폴더를 삭제했습니다.');
})
```

---

# 참고문헌
- *Do it! Node.js 프로그래밍*

