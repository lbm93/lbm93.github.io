---
layout: article
title: "LOG 파일 남기기"
date: 2019-11-11 18:00:32 +0900
categories: [development, nodejs]
# description: "웹 통신 프로토콜인 URL, HTTP, SMTP, MIME, FTP 을 정리"
excerpt: "LOG 파일 남기기"
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

# 로그 파일 남기기
console 객체의 log 또는 error 메소드 등을 호출하면 로그를 출력할 수 있다. 하지만 프로그램의 크기가 커질수록 로그의 양도 많아지고 로그를 보관했다가 나중에 확인해야 하는 경우도 생긴다. 따라서 어떻게 로그를 남기고 보관할 것인지가 중요해집니다. 로그를 보관하려면 화면에만 출력하는 것만으로는 부족한다. 이 때문에 다양한 방식으로 로그를 남길 수있도록 외부 모듈을 사용한다. 여기에서는 여러 가지 로그 모듈 중에서 `winston 모듈`로 로그를 남기는 방법을 알아보자.  

---

```javascript
const appRoot = require('app-root-path');    // app root 경로를 가져오는 lib(npm install --save app-root-path) 
const winston = require('winston');            // winston lib
const process = require('process');
 
const { combine, timestamp, label, printf } = winston.format;
 
const myFormat = printf(({ level, message, label, timestamp }) => {
  return `${timestamp} [${label}] ${level}: ${message}`;    // log 출력 포맷 정의
});
 
const options = {
  // log파일
  file: {
    level: 'info',
    filename: `./logs/info.log`, // 로그파일을 남길 경로 ,${appRoot}도 사용가능
    handleExceptions: true,
    json: false,
    maxsize: 5242880, // 5MB
    maxFiles: 5,
    colorize: false,
    format: combine(
      label({ label: 'winston-test' }),
      timestamp(),
      myFormat    // log 출력 포맷
    )
  },
  // 개발 시 console에 출력
  console: {
    level: 'debug',
    handleExceptions: true,
    json: false, // 로그형태를 json으로도 뽑을 수 있다.
    colorize: true,
    format: combine(
      label({ label: 'nba_express' }),
      timestamp(),
      myFormat
    )
  }
}
 
let logger = new winston.createLogger({
  transports: [
    new winston.transports.File(options.file) // 중요! 위에서 선언한 option으로 로그 파일 관리 모듈 transport
  ],
  exitOnError: false, 
});
 
if(process.env.NODE_ENV !== 'production'){
  logger.add(new winston.transports.Console(options.console)) // 개발 시 console로도 출력
}
 
module.exports = logger;

```

간단하게 디버깅을위해 복사해 조금만 수정해 사용하면된다.

파일이름과, lable(보통은 app의 이름)으로 수정해서 사용하면 편리하다.

마지막으로 module.exports를 해놨으니 외부에서 require해서 사용하면 된다.

---

# 참고문헌
- *Do it! Node.js 프로그래밍*

