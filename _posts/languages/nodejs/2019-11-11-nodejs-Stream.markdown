---
layout: article
title: "Stream 사용법"
date: 2019-11-11 18:00:32 +0900
categories: [development, nodejs]
# description: "웹 통신 프로토콜인 URL, HTTP, SMTP, MIME, FTP 을 정리"
excerpt: "Stream 사용법"
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

# Stream 사용법
파일을 읽거나 쓸 때는 데이터 단위가 아닌 스트림 단위로 처리할 수도 있다. 스트림은 데이터가 전달되는 통로와 같은 개념이다.  

---

# createReadStream(), createWriteStream()
createReadStream(), createWriteStream() 두개의 메서드는 fs 객체로부터 제공된다.  

|메소드이름|설명|
|:---|:---|
|createReadStream(path,[option])|파일을 읽기 위한 스트림 객체를 만든다.|
|createWriteStream(path,[option])|파일을 쓰기 위한 스트림 객체를 만든다.|

두 개의 메서드 option에는 r,w같은 flag가 들어간다. 예제를 살펴보자.  

```javascript
const fs = require('fs');

const infile = fs.createReadStream('./output.txt', {flags:'r'});
const outfile = fs.createWriteStream('./output1.txt', {flags:'w'});

infile.on('data',(data)=>{
    console.log(`읽어 들인 데이터 ${data}`);
    outfile.write(data);
});

infile.on('end',()=>{
    console.log('파일 읽기 종료');
    outfile.end(()=>{
        console.log('파일 쓰기 종료');
    });
});
```
위 코드를 보면 읽기 스트림전용 파일하나 쓰기 스트림전용 파일 하나 만들어 이벤트를 만들어 사용한다.
fs모듈은 event를 inherits하게 만들어져있어 내부적으로 이벤트 메서드 사용이 가능하다.  

즉 잘 보면 output파일에 데이터가 있으면 이벤트를 발생시켜 데이터를 읽음과 동시에 output1에도 쓴다.
그리고 읽기가 종료되면 이벤트를 발생시켜 읽기와 쓰기를 중단한다.


# pipe()
앞서 살펴본 두 메서드는 두 개의 스트림을 붙여 주면 더 간단하게 만들 수 있다. 이때 사용되는 메서드가 pipe()메서드이다. 바로 예제를 살펴보자  

```javascript
const fs = require('fs');

const inname = './output.txt';
const outname = './output1.txt';

fs.exists(outname,(exists)=>{
    if(exists){
        fs.unlink(outname, (err)=>{
            if(err) throw err;
            console.log(`기존 파일 [${outname}] 삭제함`);
        })
    }
    
    let infile = fs.createReadStream(inname, {flags:'r'});
    let outfile = fs.createWriteStream(outname,{flags:'w'});

    infile.pipe(outfile);
    console.log(`파일 복사 ${inname} -> ${outname}`);
});
```

두개의 스트림을 하나로 연결시켜 output파일에 내용이 쓰이면 output1에도 쓰이는걸 확인할 수 있다.  

`이런 스트림을 이용하는 방법은 반복되는 작업에는 사용하면 좋지 않고 한번에 큰 데이터를 받아야 할 때 사용하는 것이 좋다. 그 이유는 메모리를 반복적으로 받게 되면 메모리 소모가 크기 때문이다`


활용으로 http모듈로 요청받은 파일 내용을 읽고 응답해보자.

```javascript
const fs = require('fs');
const http = require('http');

const server = http.createServer((req,res)=>{
    let instream = fs.createReadStream('./output.txt');
    instream.pipe(res);
});

server.listen(7001, '127.0.0.1');
```

위 코드를 간단하게 그림으로 보고 설명을 하겠다.  

![그림]({{ site.url }}/images/img/language-nodejs/stream1.PNG) 

위 그림을 보면 클라이언트로부터 연결 요청이 오면 서버는 읽기 전용 스트림을 하나 만든다(output.txt) 그리고 클라이언트에게 응답할 객체 res 객체와 pipe를 통해 연결한다. 이게 가능한 이유는 내가 파일을 읽을 스트림도 객체고 클라이언트에게 응답해줄 res도 스트림 객체이기 때문이다.


---

# 참고문헌
- *Do it! Node.js 프로그래밍*

