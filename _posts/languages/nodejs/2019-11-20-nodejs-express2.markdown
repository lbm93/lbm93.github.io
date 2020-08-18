---
layout: article
title: "Express 2탄"
date: 2019-11-20 18:00:32 +0900
categories: [development, nodejs]
# description: "웹 통신 프로토콜인 URL, HTTP, SMTP, MIME, FTP 을 정리"
excerpt: "Express 1탄"
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

# express 2탄(req,res에 대하여)
2부에서는 express에서도 사용되고 http 모듈에서도 사용되는 파라미터인 요청객체(req)와 응답객체(res)에 대하여 알아본다.  

---

# 익스프레스의 요청 객체와 응답 객체 알아보기
익스프레스에서 사용하는 요청 객체와 응답 객체는 http 모듈에서 사용하는 객체들과 같다.하지만 몇가지 메소드를 더 추가할 수 있다. 다음은 익스프레스에서 추가로 사용할 수 있는 주요 메소드들이다.  

|메소드이름|설명|
|:---|:---|
|send([body])|클라이언트에 응답 데이터를 보낸다. 전달할 수 있는 데이터는 HTML 문자열, Buffer 객체, JSON객체, JSON 배열이다.|
|status(code)|HTTP 상태 코드를 반환한다. 상태 코드는 end()나 send() 같은 전송 메소드를 추가로 호출해야 전송할 수 있다.|
|sendStatus(statusCode)|HTTP 상태 코드를 반환한다. 상태 코드는 상태 메시지와 함께 전송된다.|
|redirect([status,]path)|웹 페이지 경로를 강제로 이동시킨다.|
|render(view[,local][,callback])| 뷰 엔진을 사용해 문서를 만든 후 전송한다.|

send() 메소드는 응답 데이터를 좀 더 간단한게 전송하기 위해 익스프레스에서 추가 된 것이다. send()메소드를 이용해 json 데이터를 전송하는 예제를 보자.  

```javascript
app.use((req,res,next)=>{
    console.log(`첫 번째 미들웨어에서 요청을 처리함`);

    //send()를 이용해 JSON 데이터를 전송한다.
    res.send({name: '이병민', age :20});
});
```


사실 JSON 객체를 그대로 받아 웹 페이지로 보여주는 경우는 거의 없다. 하지만 모바일 단말기에서 사용하는 웹 앱이나 일반 앱에서는 화면을 보여주기 위해 필요한 웹 문서들을 단말 쪽에 두고 Ajax 또는 웹 소켓으로 데이터만 수신하여 화면에 보여줄 때가 많기 때문에 JSON 객체를 전송하는 기능이 필요하다.  

redirect()일 경우 다른 페이지로 이동 할 수 있게 해주는 메소드이다 간단한 예제를 보자.  

```javascript
app.use((req,res,next)=>{
    console.log(`첫 번째 미들웨어에서 요청을 처리함`);

    res.redirect('http://www.google.co.kr');

});
```

위 코드 수행시 바로 google 홈페이지가 뜨게 된다. redirect() 메소드를 사용하면 URL 주소뿐만 아니라 프로젝트 폴더 안에있는 다른 페이지를 보여 줄 수 있다.  



# 익스프레스에서 요청 객체이 추가한 메소드 알아보기
이번에는 익스프레스에서 요청 객체에 추가한 메소드들을 살펴보자.

|메소드이름|설명|
|:---|:---|
|param(name)|요청 파라미터를 확인한다.|
|header(name)|헤더를 확인한다.|

클라이언트의 요청으로 보내진 파라미터들은 요청 객체의 param() 메소드로 확인할 수 있다. 예를들어 'http://localhost:3000/?name=mike'와 같은 주소로 접속한 경우 요청 파라미터인 name의 값은 다음 코드로 확인할 수 있다.

```javascript
const paramName = req.parapm('name`);
```

요청 파라미터는 영어로 qeury string이라고 한다. 요청 파라미터는 req.query 객체의 속성으로 들어가 다음과같은 코드로 파라미터를 구분할 수 있다.

```javascript
const paramName = req.query.name;
```

요청할 떄 헤더 값들은 header() 메소드로 확인할 수 있다. 그럼 이제 header값과 param값을 확인하는 예제를 살펴보자.

```javascript
const express = require('express'),
    http = require('http'),
    path = require('path');

const app = express();

app.use((req,res,next)=>{
    console.log(`첫 번째 미들웨어에서 요청을 처리함`);

    let userAgent = req.header('User-Agent');
    let paramName = req.param('name');

    res.writeHead('200', {'Content-Type' : 'text/html; charset=utf8'});
    res.write(`<h1>Express 서버에서 응답한 결과입니다.</h1>`);
    res.write(`<div><p>User-Agent : ${userAgent}</p></div>`);
    res.write(`<div><p>Param name : ${paramName}</p></div>`);
    res.end();
});


app.set('port', process.env.PORT || 9928);

http.createServer(app).listen(app.get('port'),()=>{
    console.log(`Express 서버가 ${app.get('port')} 포트에서 시작됨`);
});
```
위 코드를 수행하면 다음과 같은 화면이 나온다.

![그림]({{ site.url }}/images/img/language-nodejs/express7.PNG)

밑의 그림은 익스프레스에서 클라이언트의 요청 파라미터를 확인하는 과정을 간단하게 정리한 것이다.  

![그림]({{ site.url }}/images/img/language-nodejs/express8.PNG)

클라이언트가 주소 문자열에 포함시켜 전달하는 요청 파라미터를 웹 서버에서 받아 확인할 떄는 보통 여러 줄의 코드가 필요하다. 하지만 익스프레스를 사용하면 익스프레스 내부에서 요청 파라미터를 어떻게 관리하는지 잘 모르더라도 한 줄의 코드만으로 요청 파라미터 값을 확인할 수 있다.

---

# 참고문헌
- *Do it! Node.js 프로그래밍*

