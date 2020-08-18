---
layout: article
title: "Express 5탄"
date: 2019-11-26 18:00:32 +0900
categories: [development, nodejs]
# description: "웹 통신 프로토콜인 URL, HTTP, SMTP, MIME, FTP 을 정리"
excerpt: "Express 5탄"
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

# express 5탄(쿠키와 세션, 파일 업로드 기능)
express 5탄에서는 쿠키와 세션, 그리고 파일 업로드 기능을 하기 위한 multer 모듈에 대하여 알아본다.

---

# 쿠키 처리하기
사용자가 로그인한 상태인지 아닌지 확인하고 싶을 때에는 쿠키나 세션을 사용한다. 쿠키는 클라이언트 웹 브브라우저에 저장되는 정보로 일정 기간 동안 저장하고 싶을 때 사용한다. 익스프레스에서는 cookie-parser 미들웨어를 사용하면 쿠키를 설정하거나 확인할 수 있다. cookie-parser은 외장 모듈이기 때문에 설치가 필요하다.

- %npm install cookie-parser --save

```javascript
...
//쿠키 파서 모듈 사용
const cookieParser = require('cookie-parser');
...
//쿠키 미들웨어 사용
app.use(cookieParser())
...
//쿠키 정보를 확인함
app.get('/process/showCookie', (req,res)=>{
    console.log('/process/showCookie 호출됨');

    console.log(req.cookies);
    res.send(req.cookies); // 응답하려는 데이터가 객체이기때문에 제대로 안보내진다.
});

//쿠키에 이름 정보를 설정함
app.get('/process/setUserCookie', (req,res)=>{
    console.log('/process/setUserCookie 호출됨');
    
    //쿠키 설정
    res.cookie('user', {
        id:'mike',
        name: '소녀시대',
        authorized:true
    });

    //redirect로 응답
    res.redirect('/process/showCookie');
});
...

```

위 코드를 /precess/setUserCooker 패스로 요청을 보내면 다음과같은 화면이 뜬다.  

![그림]({{ site.url }}/images/img/language-nodejs/express18.PNG)  

그 이유는 응답으로 보내는 데이터가 스트링이나 버퍼형태여야 하는데 객체이기때문에 문제가 발생한 것이다. 콘솔로는 다음처럼 제대로 데이터가 나온다.  

![그림]({{ site.url }}/images/img/language-nodejs/express19.PNG)  

위의 과정들은 밑의 그림을 참고하자.

![그림]({{ site.url }}/images/img/language-nodejs/express20.PNG)  


# session 처리하기
이번에는 세션을 만들어보자. 세션도 상태 정보를 저장하는 역할을 하지만, 쿠키와 달리 서버쪽에 저장된다. 세션을 사용하는 대표적인 예로는 로그인을 했을 때 저장되는 세션을 들 수 있다.
사용자가 로그인하면 세션을 만들고 로그아웃하면 세션이 삭제되는 기능을 만들면 사용자가 로그인하기 전에는 접근이 제한된 페이지를 보지 못하도록 설정할 수 있다. session을 처리하는 모듈도 외장모듈이기때문에 설치해준다.  

- %npm install express-session --save  

이제 다음으로 예제를 살펴본다. 이 예제는 '/process/product' 패스로 요청을 할 경우 서버에서 사용자에대한 session이 있을 경우 '/public/product.hmtl'을 보여주고 없으면 '/public/login2.html' 을 보여준다. login2.html은 로그인 버튼 클릭시 post요청을 동작하게 만들어져있어서 로그인 시도시 로그인이 된 상태인지 아닌지 확인해 session을 만드는 라우터가 있다. 또 한 로그아웃처리하는 라우터도 존재한다. 바로 밑의 그림을 보며 동작과정을 이해해보자.  

![그림]({{ site.url }}/images/img/language-nodejs/session1.PNG)  

바로 코드를 살펴보자.  

1. 먼저 모듈을 require 하고 사용 준비를 한다.

```javascript
...

//쿠키 파서 모듈 사용
const cookieParser = require('cookie-parser');

//세션 모듈 사용
const expressSession = require('express-session');

...

//쿠키 미들웨어 사용
app.use(cookieParser())

//body 파서 미들웨어 사용
app.use(bodyParser.urlencoded({extended : true}));

//public디렉터리 접근시 public을 고정시켜 /public경로를 생략
app.use(express.static(path.join(__dirname,'public')));

//세션 미들웨어 사용
app.use(expressSession({
    secret :'my key',
    resave:true,
    saveUninitialized:true  
}));

...
```


2. 사용자가 상품정보를 보기 위해 요청할 /process/product 패스 처리 라우터

```javascript
...

//product 요청시 session이 있으면 product 보여주고 없으면 login2 보여줌
app.get('/process/product', (req,res)=>{
    console.log('/process/product 호출됨');

    if(req.session.user){
        console.log('session이 있을경우');
        res.redirect('/product.html');
    }
    else{
        console.log('session이 없을경우');
        res.redirect('/login2.html');
    }
});

...
```
![그림]({{ site.url }}/images/img/language-nodejs/session2.PNG)  



3. product.html 작성

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>상품 페이지</title>
    </head>
<body>
    <h3>상품정보 페이지</h3>
    <hr/>
    <p>로그인 후 볼 수 있는 상품정보 페이지입니다.</p>
    <br><br>
    <a href='/process/logout'>로그아웃하기</a>
</body>
</html>
```
![그림]({{ site.url }}/images/img/language-nodejs/session4.PNG)  



4. login2.html에서 로그인시도시 /process/login 처리 라우터

```javascript
...

//로그인 시도시 실행
app.post('/process/login', (req,res)=>{
    console.log('/process/login 호출됨');
    
    let paramId = req.param('id');
    let paramPassword = req.param('password');

    if(req.session.user){
        //이미 로그인된 상태
        console.log('이미 로그인되어 상품 페이지로 이동합니다.');

        res.redirect('/product.html');
    }else{
        //로그인되지 않은 상태 세션 저장
        req.session.user={
            id: paramId,
            name:'소녀시대',
            authorized: true
        };

        res.writeHead('200', {'Content-Type':'text/html;charset=utf8'});
        res.write('<h1>로그인 성공</h1>');
        res.write('<div><p>Param id : ' + paramId + '</p></div>');
        res.write('<div><p>Param id : ' + paramPassword + '</p></div>');
        res.write("<br><br><a href= '/process/product'>상품 페이지로 이동하기</a>");
        res.end();
    }
});
...
```

![그림]({{ site.url }}/images/img/language-nodejs/session3.PNG) 



5. 로그아웃시 처리 라우터

```javascript
...

//로그아웃 처리시 실행
app.get('/process/logout', (req,res)=>{
    console.log('/process/logout 호출됨');

    if(req.session.user){
        //로그인된 상태
        console.log('로그아웃 합니다.');

        req.session.destroy((err)=>{
            if(err){throw err;}

            console.log('세션을 삭제하고 로그아웃되었습니다.');

            res.redirect('/login2.html');
        });
    }
    else{
        //로그인 안된 상태
        console.log('아직 로그인되어 있지 않습니다.');

        res.redirect('/login2.html');
    }
});
...
```

---

# 참고문헌
- *Do it! Node.js 프로그래밍*

