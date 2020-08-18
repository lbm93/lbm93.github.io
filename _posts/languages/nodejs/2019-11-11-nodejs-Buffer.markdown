---
layout: article
title: "Buffer 사용법"
date: 2019-11-11 18:00:32 +0900
categories: [development, nodejs]
# description: "웹 통신 프로토콜인 URL, HTTP, SMTP, MIME, FTP 을 정리"
excerpt: "Buffer 사용법"
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

# Buffer 사용법
버퍼는 Nodejs에서 제공하는 바이너리 데이터를 담을 수 있는 객체이다. 버퍼를 이용해 입력을 받고 버퍼의 내용을 파일에 쓴다거나 이런 경우에 버퍼가 사용된다.

---

# Buffer 객체 생성
Buffer 객체를 생성하기 위해서는 new 연산자를 이용한다.

```javascript
//생성자에 길이를 파라미터로 보내 길이가 10인 버퍼 객체를 생성
const buffe1 = new Buffer(10);

//생성자에 저장 할 데이터와 인코딩형식을 파라미터를 보내 버퍼 객체를 생성
//인코딩 생략가능 생략시 default는 utf-8
const buffe2 = new Buffer('안녕', 'utf-8');
```


# Buffer의 write()
Buffer를 만들어 놓고 버퍼에 데이터를 동적으로 쓰기 위해서는 write 메서드를 이용한다.

```javascript
const buffe1 = new Buffer(10);
const output = '안녕!'
buffer1.write(output, 'utf-8');
```


# Buffer의 toString()
Buffer 객체를 문자열 객체로 바꿀 때 사용되는 메서드이다.

```javascript
const buffe1 = new Buffer(10);
const output = '안녕!'
buffer1.write(output, 'utf-8');
console.log(`버퍼객체를 문자열 객체로 바꿔 출력 ${buffe1.toString()}`);
```


# Buffer의 copy()
copy메서드는 하나의 버퍼 객체 내용을 다른 버퍼 객체에 복사하는 메서드이다.

```javascript
//첫번째 버퍼 생성
const output = '안녕 1!';
const buffer1 = new Buffer(10);
const len = buffer1.write(output, 'utf-8');

//두번째 버퍼 생성
const buffer2 = new Buffer('안녕 2!','utf-8');

//첫번째 버퍼 내용을 두번째 버퍼에 복사
buffer1.copy(buffer2, 0, 0, len);
```


# Buffer의 concat()
concat 메서드는 두개의 버퍼의 내용을 합치는 메서드이다.

```javascript
//첫번째 버퍼 생성
const output = '안녕 1!';
const buffer1 = new Buffer(10);
const len = buffer1.write(output, 'utf-8');

//두번째 버퍼 생성
const buffer2 = new Buffer('안녕 2!','utf-8');

//첫번째 버퍼와 두번째 버퍼 내용을 합쳐 세번째 버퍼 객체 생성
const buffer3 = Buffer.concat([buffer1,buffer2]);
```

---

# 참고문헌
- *Do it! Node.js 프로그래밍*

