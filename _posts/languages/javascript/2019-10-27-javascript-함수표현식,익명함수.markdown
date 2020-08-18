---
layout: article
title: "함수 표현식과 익명함수"
date: 2019-10-27 18:00:32 +0900
categories: [development, javascript]
# description: "웹 통신 프로토콜인 URL, HTTP, SMTP, MIME, FTP 을 정리"
excerpt: "함수 표현식과 익명함수"
image:
  teaser: posts/javascript/javascript.png
  credit: 
  creditlink: 
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
---
{% include toc.html %}

# 함수 표현식과 익명 함수
지금까지 우리는 함수 선언만 봤다. 자바스크립트에서는 익명 함수(Anonymous function)도 지원한다. 익명함수에는 함수에 식별자가 주어지지 않는다.
식별자가 없다면 어떻게 호출할까? 답은 함수 표현식(Function Expression)에 있다. 우리는 표현식이 값이 되고, 함수 역시 값이 되는걸 알고 있다.
함수 표현식은 식별자에 할당할 수도 있고 즉시 호출 할 수도 있다.

함수 표현식은 함수 이름을 생략할 수 있다는 점을 제외하면 함수 선언과 문법적으로 완전히 같다.

```javascript
cosnt f = function(){
    //....
};
```
위의 함수 표현식은 식별자를 생략한 함수 표현식이다. 그렇다면 식별자를 생략하지 않은 함수 표현식은 어떻게 될까?


```javascript
const f = function g(){
    //...
}
```

이런식으로 함수를 만들면 이름 f에 우선순위가 있다. 그리고 함수 바깥에서 함수에 접근할 때는 f를 써야하며, g로 접근하려면 변수가 정의되지 않았다는 에러가 발생한다.
그렇다면 왜 이런 방법을 사용할까? 함수 안에서 자신을 호출할 때(재귀) 이런 방식이 필요로 하다. 예제를 보자


```javascript
const g = function f(stop){
    if(stop) console.log('f stopped');
    f(ture);
};

g(false);
```

함수 안에서 g를 써서 자기 자신을 참조하고, 함수 바깥에서는 f를 써서 함수를 호출한다. 
함수에 두 가지 이름을 붙이는건 안좋지만 여기서 이렇게 예를 든건 이름이 붙은 함수표현식과 익명 함수표현식이 뭐가 다른지 이해하기 위해서이다.

# 참고문헌
- *러닝 자바스크립트: ES6로 제대로 입문하는 모던 자바스크립트 웹 개발*

