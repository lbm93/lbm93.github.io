---
layout: article
title: "Express 4탄"
date: 2019-11-20 18:00:32 +0900
categories: [development, nodejs]
# description: "웹 통신 프로토콜인 URL, HTTP, SMTP, MIME, FTP 을 정리"
excerpt: "Express 4탄"
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

#  express 4탄(라우터 미들웨어, URL 파라미터, 오류 페이지 보여주기, 토큰과 함께 요청한 정보 처리)
express 프레임워크 사용하기 4탄에서는 라우터 미들웨어, URL 파라미터, 오류 페이지 보여주기, 토큰과 함께 요청한 정보 처리하기에 대하여 알아본다.

---

# 라우터 미들웨어
앞에서 로그인 페이지를 열고 버튼을 눌렀을 때 로그인 처리를 use()메소드로 처리했다. use()메소드를 사용할 경우 등록한 미들웨어가 항상 호출되기 때문에 요청 url이 무엇인지 일일이 확인해야 하는 번거로움이 있었는데 라우터 미들웨어를 통해 이를 해결할 수 있다. router도 미들웨어 중의 하나이지만 app 객체의 속성으로 제공되며 사용자가 요청한 기능이 무엇인지 path를 기준으로 구별해 주기 때문에 아주 중요한 역할을 담당한다.  

`라우터 미들웨어는 익스프레스에 미리 등록되어 있어 바로 사용 가능하다.`  

다음은 라우터 미들웨어 메소드들이다.  

|메소드 이름|설명|
|:---|:---|
|get(path, callback)|GET 방식으로 특정 패스 요청이 발생했을 때 사용할 콜백 함수를 지정한다.|
|post(path, callback)|POST 방식으로 특정 패스 요청이 발생했을 때 사용할 콜백 함수를 지정한다.|
|put(path, callback)|PUT 방식으로 특정 패스 요청이 발생했을 때 사용할 콜백 함수를 지정한다.|
|delete(path, callback)|DELETE 방식으로 특정 패스 요청이 발생했을 때 사용할 콜백 함수를 지정한다.|
|all(path, callback)|모든 요청 방식을 처리하며, 특정 패스 요청이 발생했을 때 사용할 콜백 함수를 지정한다.|

예시로 살펴보자. 먼저 앞서 만들었던 login.html에 <form> 태그에 action 속성을 추가한다.  

```html
<form method = "post", action = "/process/login">
```

위 페이지를 통해 로그인 정보가 전송될때의 path는 /process/login이 된다.   

다음은 app.js를 수정한다. 이전에 사용하던 use() 대신에 post()를 사용한다.  

```javascript
app.post('/process/login',(req,res)=>{
    console.log(`첫 번째 미들웨어에서 요청을 처리함`);

    let paramId = req.param('id');
    let paramPassword = req.param('password');

    res.writeHead('200', {'Content-Type' : 'text/html; charset=utf8'});
    res.write(`<h1>Express 서버에서 응답한 결과입니다.</h1>`);
    res.write(`<div><p>User-Agent : ${paramId}</p></div>`);
    res.write(`<div><p>Param name : ${paramPassword}</p></div>`);
    res.write("<br><br><a href='/public/login2.html'>로그인 페이지로 돌아가기</a>");
    res.end();
});
```

위 코드를 보면 post 메소드를 이용해 /process/longin 패스로 요청이 들어오면 콜백함수를 수행한다.  

![그림]({{ site.url }}/images/img/language-nodejs/express10.PNG)  

![그림]({{ site.url }}/images/img/language-nodejs/express11.PNG)  

위 과정을 그림으로 나타내면 다음과 같다.  

![그림]({{ site.url }}/images/img/language-nodejs/express12.PNG)  

콜백 함수에서는 로그인에 필요한 기능을 실행한 후 클라이언트로 응답을 보내 줄 수 있다.  


# URL 파라미터 사용하기
URL 뒤에 ? 기호를 붙이면 필요에 따라 요청 파라미터(query string)을 추가하여 보낼 수 있다. 이런 요청 파라미터는 서버에 데이터를 전달하기 위해 사용되는데, 요청 파라미터 대신 URL 파라미터를 사용할 수도 있다. URL 파라미터는 요청 파라미터와 달리 URL 주소의 일부로 들어간다.  

먼저 앞서 만들었던 loging.html의 <form> 태그에 있는 action에 URL 파라미터를 추가한다. 만약 전송 버튼을 누르게 되면 이 URL 파라미터가 포함된체 서버로 전송된다.  


```html
<form method="post" action="/process/login/lbm">
```

다음은 /login 뒤에 있는 /lbm을 파싱한다. app.js를 수정한다.

```javascript
app.post('/process/login/:name',(req,res)=>{
    console.log(`첫 번째 미들웨어에서 요청을 처리함`);

    let paramName = req.params.name;
    let paramId = req.param('id');
    let paramPassword = req.param('password');

    res.writeHead('200', {'Content-Type' : 'text/html; charset=utf8'});
    res.write(`<h1>Express 서버에서 응답한 결과입니다.</h1>`);
    res.write(`<div><p>ParamName : ${paramName}</p></div>`);
    res.write(`<div><p>User-Agent : ${paramId}</p></div>`);
    res.write(`<div><p>Param name : ${paramPassword}</p></div>`);
    res.write("<br><br><a href='/public/login2.html'>로그인 페이지로 돌아가기</a>");
    res.end();
});
```
post로 전송되는 /process/login 패스로 요청이 왔을 경우 뒤에 `:name`을 파라미터로 받아들인다고 적어 놓은 것이다. 그렇기 때문에 req.params.name으로 쉽게 URL 파라미터를 파싱할 수 있다.  

![그림]({{ site.url }}/images/img/language-nodejs/express13.PNG) 

![그림]({{ site.url }}/images/img/language-nodejs/express14.PNG) 


# 오류 페이지 보여주기
로그인을 위해 웹 브라우저에 주소를 입력할 때 주소가 입력되는 부분을 보면 /process/login 패스로 처리된 것을 알 수 있다. 이 패스는 웹 서버에서 라우터 미들웨어로 등록했기 때문에 등록된 콜백 함수에서 요청을 전달받아 처리하였다. 그런데 등록되지 않은 패스로 요청이 들어왔을 경우 우리는 오류 페이지를 보여줘야한다.  

따라서 위에서 추가한 post()메소드 아래쪽에 다음과 같이 all()메소드 호출 부분을 추가하여 오류 페이지를 보여줄 수 있다.  

```javascript
app.all('*', (req,res) =>{
    res.send(404, '<h1>ERROR - 페이지를 찾을 수 없습니다.</h1>');
});
```

![그림]({{ site.url }}/images/img/language-nodejs/express15.PNG) 

---

# express-error-handler 미들웨어로 오류 페이지 보내기
express-error-handler 미들웨어를 사용해 404.html 페이지를 응답한다. 먼저 express-error-handler도 외장 모듈이기 때문에 다음과 같이 설치한다.  

- %npm install express-error-handler --save

그럼 예제를 살펴보기전에 그림으로 먼저 이해해보자.  

![그림]({{ site.url }}/images/img/language-nodejs/express17.PNG) 

위 그림을 보면 미리 등록된 라우팅패스가 없으면 핸들러에의해 404.html을 보여주게 처리된다. 이제 app.js에 다음과같이 추가하여 실행해보자.  

```javascript
...

//오류 핸들러 모듈 사용
const expressErrorHandler = require('express-error-handler');

...

//모든 router 처리 끝난 후 404 오류 페이지 처리
const errorHandler = expressErrorHandler({
    static:{
        '404': './public/404.html'
    }
});

app.use(expressErrorHandler.httpError(404));
app.use(errorHandler);

...

```

위 코드는 먼저 에러 핸들러 모듈을 가지고 객체를 만든 후 모든 라우터 처리가 끝난 후 해당 패스에 존재하지 않으면 에러 핸들러에 의하여 404.html을 처리하게끔 하는 소스코드이다.  

이제 public/404.html을 만들어보자.  

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>오류 페이지</title>
    </head>
    <body>
        <h3>ERROR-페이지를 찾을 수 없습니다.</h3>
        <hr/>
        <p>/public/404.html 파일의 오류 페이지를 표시한 것입니다.</p>
    </body>
</html>
```
코드를 다 작성하고 app을 실행시켜보면 라우터에 등록된 패스를 요청하지 않았을 경우 다음과 같이 결과가 나온다.  

![그림]({{ site.url }}/images/img/language-nodejs/express16.PNG) 



# 토큰과 함께 요청한 정보 처리하기
라우터 미들웨어는 유용하게 사용할 수 있는 요청 URL에 포함된 파라미터, 즉 토큰(Token)을 사용할수 있다. 요청 패스에 토큰을 추가하여 보내 온 요청을 처리하도록 만들어보자.  

```javascript
/url 파라미터 추가한 패스 등록
app.post('/process/users/:id',(req,res)=>{

    let paramId = req.params.id;

    console.log(`/process/users와 토큰 ${paramId}를 사용해 처리함.`);

    res.writeHead('200', {'Content-Type' : 'text/html; charset=utf8'});
    res.write(`<h1>Express 서버에서 응답한 결과입니다.</h1>`);
    res.write(`<div><p>User-Agent : ${paramId}</p></div>`);

    res.end();
});
```

---

# 참고문헌
- *Do it! Node.js 프로그래밍*

