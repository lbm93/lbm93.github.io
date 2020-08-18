---
layout: article
title: "Express 1탄"
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

#  express 1탄(개발환경,미들웨어 1탄)
express 프레임워크 사용하기 1탄에서는 vscode에서 express를 설치하는 방법과 미들웨어 사용방법에 대하여 알아본다.

---

# 개발 환경
먼저 개발환경은 vscode로 이어한다. vscode에서 nodejs의 express 모듈을 사용하기 위해서는 express 모듈을 설치해줘야한다. nodejs에서는 이런 모듈 관리를 npm으로 이루어지므로 npm을 이용해 설치하도록 하자.

1. 디렉터리 생성 후 npminit
밑의 그림처럼 디렉터리를 임의로 "NODEEXPRESS"라고 만들고 터미널에서 해당 디렉터리로 이동 후 **npm init**을 수행한다.

![그림]({{ site.url }}/images/img/language-nodejs/express1.PNG)

위 과정을 수행하면 여러가지 입력란이 뜨는데 그냥 엔터로 넘긴다.  
과정을 제대로 수행했으면 현재 디렉터리에 **package.json**이라는 파일이 생긴다.

![그림]({{ site.url }}/images/img/language-nodejs/express2.PNG)

2. express 설치
다음은 express를 설치하기 위하여 **npm install express** 라고 터미널에 입력한다.

![그림]({{ site.url }}/images/img/language-nodejs/express3.PNG)

정상적으로 수행되었으면 아래 그림처럼 디렉터리에 **node_modules**와 **pacakgae-lock.json** 파일이 생겼을 것이다.

![그림]({{ site.url }}/images/img/language-nodejs/express4.PNG)

이렇게 개발환경을 모두 구성을 하였다. 이제 express 를 시작해보도록 하자.



# 미들웨어와 라우터
익스프레스에서는 미들웨어 이외에 라우터도 사용하는데 미들웨어나 라우터는 하나의 독립된 기능을 가진 함수라고 생각할 수 있다. 다시말해, `익스프레스에서는 웹 요청과 응답에 관한 정보를 사용해 필요한 처리를 진행할 수 있도록 독립된 함수로 분리한다. 이렇게 분리한 각각의 것들을 미들웨어라고 한다.`  

예를들어 클라이언트에서 요청했을 때 로그를 남기는 간단한 기능을 함수로 만든 후 use()메소드를 사용해 미들웨어로 등록해 두면, 모든 클라이언트 요청이 이 미들웨어를 거치면서 로그를 남기게 된다. `각각의 미들웨어는 next() 메소드를 호출하여 그다음 미들웨어가 처리할 수 있도록 순서를 넘길 수 있다.`  

`라우터는 클라이언트의 요청 패스를 보고 이 요청 정보를 처리할 수 있는 곳으로 기능을 전달해 주는 역할을 한다.` 이러한 역할을 흔히 라우팅(Routing)이라고 부르는데, `클라이언트의 요청 패스에 따라 각각을 담당하는 함수로 분리시켜준다.` 예를 들어 클라이언트가 /users 패스로 요청한다면 이에대한 응답 처리를 하는 함수를 별도로 분리해서 만든 다음 get()메소드를 호출하여 라우터로 등록할 수 있다. 그러면 등록해 둔 라우터 정보로 찾은 함수가 호출되며, 이 함수 안에서 클라이언트로 응답을 보낼 수 있다.

정리하자면 미들웨어는 클라이언트로부터 요청이 들어오면 미리 미들웨어에 등록해둔 메서드들이 순차적으로 동작하는 반면, 라우터는 요청 경로에따라 등록해둔 함수로 전달해주는 역할을 한다.


# express 의 주요 메소드

|메소드 이름|설명|
|:---|:---|
|set(name,value)|서버 설정을 위한 속성을 지정한다. set()메소드로 지정한 속성은 get()메소드로 꺼내 확인이 가능하다.
|get(name)|서버 설정을 위해 지정한 속성을 꺼내 온다.|
|use([path,]function[,function...])|미들웨어 함수를 사용한다.|
|get([path,]function)|특정 패스로 요청된 정보를 처리한다.|

# set()
set()메소드는 웹 서버의 환경을 설정하는 데 필요한 메소드이다. 만약 title 속성을 app객체에 넣어 두었다가 필요할 때 꺼내 사용하고 싶으면 app.set('title', 'My App') 처럼 set()메소드를 호출하여 넣어 둘 수 있다.
그런데 set()메소드로 설정한 속성의 이름이 미리 정해진 이름이라면 웹 서버의 환경 설정에 영향을 미친다. 서버 설정을 위해 미리 정해진 주요 속성 이름은 다음과 같다.

|속성이름|설명|
|:---|:---|
|env|서버 모드를 설정한다.|
|views|뷰들이 들어 있는 폴더 또는 폴더 배열을 설정한다.|
|view engine|디폴트로 사용할 뷰 엔진을 설정한다.|

```javascript
const express = require('express'),
    http = require('http');

//express 모듈을 이용해 객체생성
const app = express();

//express의 set()메서드를 이용해 서버 포트 설정
app.set('port', process.env.PORT || 9928);

//http모듈을 사용해 서버를 열때 express객체를 넘김
http.createServer(app).listen(app.get('port'), ()=>{
    console.log(`Express server listen on port  ${app.get('port')}`)
})
```

위 코드를 보면 set을 이용해 서버의 port속성을 9928로 해주었다. set()을 이용해 포트 속성을 정하고 가져오는 과정은 밑의 그림을 참고하면 더 이해가 잘 갈 것이다.

![그림]({{ site.url }}/images/img/language-nodejs/express5.PNG)



# use()
use 메소드는 미들웨어를 설정할때 사용한다. 노드에서 미들웨어를 사용하여 필요한 기능을 순차적으로 실행할 수 있다. express에서 미들웨어를 설정하여 사용하는 방식은 다음과 같다.

![그림]({{ site.url }}/images/img/language-nodejs/express6.PNG)

```javascript
const express = require('express'),
    http = require('http'),
    path = require('path');

const app = express();

app.use((req,res,next)=>{
    console.log(`첫 번째 미들웨어에서 요청을 처리함`);

    res.writeHead('200', {'Content-Type' : 'text/html; charset=utf8'});
    res.end('<h1>Express 서버에서 응답한 결과입니다.</h1>');
});

app.set('port', process.env.PORT || 9928);

http.createServer(app).listen(app.get('port'),()=>{
    console.log(`Express 서버가 ${app.get('port')} 포트에서 시작됨`);
});
```
위코드는 한개의 미들웨어 함수만 만들어 클라이언트가 서버에 접속 요청을하면 해당 함수가 수행되게 만들어 둔것이다. 여러개의 미들웨어 함수를 만드는 것은 밑의 예제로 확인해 볼 수 있다.

```javascript
/*미들웨어 여러개 */
app.use((req,res,next)=>{
    console.log(`첫 번째 미들웨어에서 요청을 처리함`);

    req.user = 'mike';
    
    next();
});

app.use('/',(req,res,next)=>{
    console.log('두 번째 미들웨어에서 요청을 처리함');

    res.writeHead('200', {'Content-Type' : 'text/html; charset=utf8'});
    res.end(`<h1>Express 서버에서 ${req.user}가 응답한 결과입니다.</h1>`);
})
```

위의 예제는 첫번째 미들웨어에서 req객체의 user속성을 mike로 바꾸고 next() 메소드를 이용해 두번째 미들웨어를 수행해 req객체의 user속성을 클라이언트에게 응답으로 같이 보내준다.  

use()를 살펴보면 파라미터로 요청 객체와 응답객체 , next 함수 객체도 전달되어 next를 이용해 다음 미들웨어를 호출할 수 있게된다.  

다음 시간에는 이 express에서의 요청 객체, 응답 객체에 대하여 알아보도록 한다.

---

# 참고문헌
- *Do it! Node.js 프로그래밍*

