---
layout: article
title: "화살표 표기법"
date: 2019-10-27 18:00:32 +0900
categories: [development, javascript]
# description: "웹 통신 프로토콜인 URL, HTTP, SMTP, MIME, FTP 을 정리"
excerpt: "화살표 표기법"
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

# 화살표 표기법
화살표 표기법은 ES6에서 새로만든 화살표 표기법(Arrow ntation)이다.  

화살표 표기법은 간단히 말해 `function이라는 단어와 중괄호 숫자를 줄이려고 고안된 단축 문법이다.`  

화살표 함수에는 세가지 단축 문법이 있다.

```
1.function을 생략해도 된다.
2.함수에 매개변수가 단 하나 뿐이라면 괄화(())도 생략할 수 있습니다.
3.함수 바디가 표현식 하나라면 중괄호와 return문도 생략할 수 있습니다.
```
`화살표 함수는 항상 익명이다. 화살표 함수도 변수에 할당할 수는 있지만, function 키워드처럼 이름 붙은 함수를 만들수는 없다.`

---

예제를 살펴보자,

```javascript
//const f1 = function() {return "hello!";}
const f1 = () => "hello";               //1.function생략

//const f2 = function(name){return `Hello, ${name}`;}
const f2 = name => `Hello, ${name}`;    //2. 함수에 매개변수가 단 하나뿐이므로 ()생략

//const f3 = function(a,b){return a+b};
const f3 = (a,b) => a+b;                //3. 함수 바디가 표현식 하나라면 중괄호와 return문 생략
```


# 화살표 함수와 일반적인 함수의 차이점
화살표 함수와 일반적인 함수의 중요한 차이가 있다. 첫번째로, this가 다른 변수와 마찬가지로 정적으로 묶인다.
저번에 만들었던 greetBackwards예제를 고쳐보자.

```javascript
const o = {
    name: 'Julie',
    greetBackwards: function(){
        function getReverseName = () =>{
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

원본 코드는 [여기클릭](https://byeongminlee.github.io/language/2019/10/27/language-javascript-3/)

이런식으로 화살표 함수를 내부함수로 사용하면 내부 함수안에서 this를 사용할 수 있다.

두번째로는, 화살표함수는 객체 생성자로 사용할 수 없다.

세번째로는, arguments 변수도 사용할 수 없다.

추가적으로, 알아야 할 것이 화살표 함수에 없는 것은 다음과 같다.  

- 함수이름

- this

- arguments

# 참고문헌
- *러닝 자바스크립트: ES6로 제대로 입문하는 모던 자바스크립트 웹 개발*