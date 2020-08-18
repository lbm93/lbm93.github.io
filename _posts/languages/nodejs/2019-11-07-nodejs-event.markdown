---
layout: article
title: "Event"
date: 2019-11-07 18:00:32 +0900
categories: [development, nodejs]
# description: "웹 통신 프로토콜인 URL, HTTP, SMTP, MIME, FTP 을 정리"
excerpt: "Event"
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

# Event
노드는 대부분 이벤트를 기반으로 하는 비동기 방식으로 처리한다.  

그리고 `비동기 방식으로 처리하기 위해 서로 이벤트를 전달한다.`    

예를들어, 어떤 함수를 실행한 결과물도 이벤트로 절단한다.  

이벤트는 한쪽에서 다른 쪽으로 알림 메시지를 보내는 것과 비슷하다.    
  
노드에는 이런 이벤트를 보내고 받을 수 있도록 EventEmitter라는 것이 만들어져 있다.  

이벤트를 사용하기 위해서는 `events` module를 사용하고 `eventEmitter`라는 객체를 만들어 사용한다. 

--- 

# 이벤트 처리 과정
이벤트 처리과정은 밑에 그림과 같이 이루어진다.

![그림]({{ site.url }}/images/img/language-nodejs/event1.PNG) 

아마 그림만 보고 잘 이해가 가지 않을 것이다. 밑에까지 읽어보고 다시 확인해보자.


# 이벤트 보내고 받기
노드의 객체는 EventEmitter를 상속받을 수 있으며, 상속받은 후에는 EventEmitter 객체의 on()과 emit()메소드를 사용할 수 있다.
on()메소드는 이벤트가 전달될 객체에 이벤트 리스너를 설정하는 역할을 하는데 이 리스너 함수는 객체로 전달된 이벤트를 받아 처리할 수 있다.
이벤트를 다른 쪽으로 전달하고 싶다면 emit() 메소드를 사용한다.

|메소드이름|설명|
|:---|:---|
|on(event,listener)|지정한 이벤트의 리스너를 추가한다.|
|once(event,listener)|지정한 이벤트의 리스너를 추가하지만 한 번 실행한 후에는 자동으로 리스너가 제거된다.|
|removeListener(event,listener)|지정한 이벤트에 대한 리스너를 제거한다.|

```javascript
//process는 내부적으로 EventEmitter를 상속받도록 만들어져 있다.
process.on('exit', function(){                  //'exit'이벤트 등록
    console.log('exit 이벤트 발생함');
});

setTimeout(function(){
    console.log('2초 후에 시스템 종료 시도함');
    process.exit();                             //'exit'이벤트 발생
}, 2000);
```
  
process 객체는 EventEmitter를 상속받게 만들어져 있어서 on()과 emit() 메소드를 바로 사용할 수 있다. 위 코드는 process 객체의 on()메소드를 호출하면서 이벤트 이름을 exit로 지정하면 프로세스가 끝날 때를 알 수 있게 만들어져 있다. 그 아래 코드에서는 2초있다가 콘솔로 종료 시도를 출력하고 이벤트를 발생시킨다.  

```javascript
process.on('tick', function(count){
    console.log(`tick 이벤트 발생함 ${count}`);
});

setTimeout(function(){
    console.log('2초 후에 tik 이벤트 전달 시도함');
    process.emit('tick','2');
}, 2000);
```

이번에는 tick 이벤트를 직접 만들고 2초 후에 setTimeout()메소드를 사용해 process.emit()메소드를 호출하면서 tick 이벤트를 process 객체로 전달한다. process.on()을 호출해 tick 이벤트를 등록하면 이 메소드를 호출하면서 파라미터로 전달한 tick 이벤트가 발생했을 때 그다음에 나오는 콜백 함수가 실행된다.  

![그림]({{ site.url }}/images/img/language-nodejs/event2.PNG)  


만약 우리가 이벤트를 리스너를 직접 등록해서 이벤트를 처리하게끔 만든다면 다음과 같이 만들어야 한다.

```javascript
//events 모듈
const events = require('events');

//EventEmitter 객체 생성
const eventEmitter = new events.EventEmitter();

//EventHandler 함수 생성
let EventHandler = () =>{
    console.log('연결이 성공적으로 이루어짐');

    eventEmitter.emit('holly_event');
}

//connection 이벤트와 EventHandler와 연동
eventEmitter.on('connection', EventHandler);

//holly_event 이벤트와 익명함수 연동
//함수를 변수안에 담는 대신, .on() 메소드의 인자로 직접 함수 전달

eventEmitter.on('holly_event', () =>{
    console.log('holly event 발생');
})

//connection 이벤트 발생
eventEmitter.emit('connection');

console.log('program has ended');
```

event를 사용하기 위해 events 모듈을 먼저 등록한다. 그 이후에 EventEmitter 객체를 생성해서 리스너와 연동하거나 이벤트를 전달(발생)하는 메서드를 이용할 수 있게 한다.

---

# 참고문헌
- *Do it! Node.js 프로그래밍*

