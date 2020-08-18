---
layout: article
title: "this 키워드"
date: 2019-10-27 18:00:32 +0900
categories: [development, javascript]
# description: "웹 통신 프로토콜인 URL, HTTP, SMTP, MIME, FTP 을 정리"
excerpt: "this 키워드"
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

# this 키워드
함수 바디 안에는 특별한 읽기 전용 값인 this가 있다. this는 일반적으로 객체지향 프로그래밍 개념에 밀접한 연관이 있다.
일반적으로 this는 객체의 프로퍼티인 함수에서 의미가 있다. 메서드를 호출하면 this는 호출한 메서드를 소유하는 객체가 된다.

```javascript
const o = {
    name: 'Wallace',
    speak(){return `My name is ${this.name}~`;},
}
```
위 객체에서 speak()를 호출하면 this는 o에 묶입니다

```
o.speak();
```
`this는 함수를 어떻게 선언했느냐가 아니라 어떻게 호출했느냐에 따라 달라진다.`
`즉, this가 o에 묶인 이유는 speak가 o의 프로퍼티여서가 아니라, o에서 speak를 호출했기 때문이다.`
같은 함수를 변수에 할당하면 어떻게 되는지 보자.

```javascript
const speak = o.speak;
speak === o.speak;  //true; 두 변수는 같은 함수를 가르킨다.
speak();            //"My name is undefined!"
```
함수를 이렇게 호출하면 자바스크립트는 이 함수가 어디에 속하는지 알 수 없으므로 this는 undefined에 묶인다.

다음 예제를 보자 이 예제에는 메서드 안에 보조 함수가 있다.

```javascript
const o = {
    name: 'Julie',
    greetBackwards: function(){
        function getReverseName(){
            let nameBackwards ='';
            for(let i=this.name.length-1; i>=0; i--){
                nameBackwards += this.name[i];
            }
            return nameBackwards;
        }
        return `${getReversName()} si eman ym, olleH`;
    },
};
o.greetBackwards(); 
```

위 예제는 동작하지 않는다. **o.greetBackwards()를 호출하는 시점에서 자바스크립트는 this를 의도한대로 o에 연결하지만, greetBackwards안에서**
**getReverseName을 호출하면 this는 o가 아닌 다른 것에 묶인다.**

`이런 문제를 해결하기 위해 널리 사용되는 방법은 변수에 this를 할당하는 것이다.`

```javascript
const o = {
    name: 'Julie',
    greetBackwards: function(){
        const self = this;
        function getReverseName(){
            let nameBackwards ='';
            for(let i=this.name.length-1; i>=0; i--){
                nameBackwards += this.name[i];
            }
            return nameBackwards;
        }
        return `${getReversName()} si eman ym, olleH`;
    },
};
o.greetBackwards(); 
```


# 참고문헌
- *러닝 자바스크립트: ES6로 제대로 입문하는 모던 자바스크립트 웹 개발*