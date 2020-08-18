---
layout: article
title: "Express 3탄"
date: 2019-11-20 18:00:32 +0900
categories: [development, nodejs]
# description: "웹 통신 프로토콜인 URL, HTTP, SMTP, MIME, FTP 을 정리"
excerpt: "Express 3탄"
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

#  express 3탄(static 미들웨어와 body-parser 미들웨어)
express 프레임워크 사용하기 3탄에서는 static 미들웨어와 body-parser 미들웨어에 대하여 알아보도록한다.

---

# static 미들웨어
먼저 static 미들웨어는 특정 폴도의 파일들을 특정 패스로 접근할 수 있도록 만들어준다. 예를들어 [public] 폴더에 있는 모든 파일을 웹 서버의 루트 패스(/)로 접근할 수 있도록 만들고 싶다면 다음 코드를 추가하면 된다.  

```javascript
app.use(express.static(path.join(__dirname,'public')));
```

이 코드는 익스프레스 프로젝트가 만들어질 때 app.js 파일에 자동으로 추가된다. 따라서 [public]폴더 안에 있는 파일들은 클라이언트에서 바로 접근할 수 있다. 정확히 이해하기 위해 밑의 예를 참고하자.  


EX)
- ExpressExapmle/public/index.html

- ExpressExample/public/images/house.png

- ExpressExample/public/login.html

기존에 위와 같은 방식으로 폴더의 파일에 접근했다면 밑의 방식은 static을 이용함으로써 루트패스로 바로 파일에 접근할 수 있도록 한다.

EX)
- http://localhost:3000/index.html

- http://localhost:3000/images/house.png

- http://localhost:3000/login.html  

아래 그림은 static으로 특정 패스와 특정 폴더를 맵핑하는 과정을 보여준다.  

![그림]({{ site.url }}/images/img/language-nodejs/express9.PNG)  

이렇게 함으로써 [public] 폴더에 바로 접근할 수 있게 되었다. 각각의 예시에서 두번째 예시인 house.png 이미지 파일을 웹 브라우저에서 보려면 app.js 파일 안에서 다음과 같은 응답을 보내면 된다.  

```javascript
res.end("<img src = '/images/house.png' width='50%'");
```

만약 [public] 폴더 안에 있는 파일을 사이트의 /public 패스로 접근하도록 만들고 싶다면 static() 메소드를 호출할 때 다음과 같이 패스를 지정해주면 된다.

```javascript
app.use('public', express.static(path.join(__dirname,'public')));
```

첫 번째 파라미터로 요청 패스를 지정했으며 두 번째 파라미터 static() 으로 특정 폴더를 지정했다. 이렇게하면 요청 패스와 특정 폴더가 맵핑(Mapping, 서로 연결되는 것)되어 접근할 수 있다.


# body-parser 미들웨어
GET방식으로 요청할 때는 주소 문자열에 요청 파라미터가 들어간다. 하지만 이와 달리 POST방식으로 요청할 떄는 본문인 본문 영역(Body 영역)에 요청 파라미터가 들어 있게 되므로 요청 파라미터를 파싱하는 방법이 GET방식과 달라진다.  

body-parser 미들웨어는 클라이언트가 POST방식으로 요청할 때 본문 영역에 들어 있는 요청 파라미터들을 파싱하여 요청 객체의 params 속성에 넣어준다.  

위에서 살펴보았던 static 미들웨어와 body-parser 미들웨어를 합쳐 로그인 페이지를 구현할 코드를 참고하며 이해해보자.  

`body-parser 미들웨어는 외장 모듈로 만들어져 있으므로 먼저 모듈을 설치해야 한다.`

- %npm install body-parser --save

```javascript
const express = require('express'),
    http = require('http'),
    path = require('path');

const bodyParser = require('body-parser');
const app = express();

app.use(bodyParser.urlencoded({extended : true}));

//클라이언트에서 모든파일을 루트패스로 접근 할 수 있도록 static 미들웨어를 설정
//EX)192.168.10.100:3000/public/login.html -> 192.168.10.100/login.html
app.use(express.static(path.join(__dirname,'public')));

//클라이언트에서 루트 패스가 아닌 public 패스로 접근하고 싶을 경우
//app.use('public', express.static(path.join(__dirname,'public')));

app.use((req,res,next)=>{
    console.log(`첫 번째 미들웨어에서 요청을 처리함`);

    let paramId = req.param('id');
    let paramPassword = req.param('password');

    res.writeHead('200', {'Content-Type' : 'text/html; charset=utf8'});
    res.write(`<h1>Express 서버에서 응답한 결과입니다.</h1>`);
    res.write(`<div><p>User-Agent : ${paramId}</p></div>`);
    res.write(`<div><p>Param name : ${paramPassword}</p></div>`);
    res.end();
});


app.set('port', process.env.PORT || 3000);

http.createServer(app).listen(app.get('port'),()=>{
    console.log(`Express 서버가 ${app.get('port')} 포트에서 시작됨`);
});
```

---

# 참고문헌
- *Do it! Node.js 프로그래밍*

