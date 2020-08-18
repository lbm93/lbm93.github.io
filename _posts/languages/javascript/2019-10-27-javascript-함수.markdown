---
layout: article
title: "함수"
date: 2019-10-27 18:00:32 +0900
categories: [development, javascript]
# description: "웹 통신 프로토콜인 URL, HTTP, SMTP, MIME, FTP 을 정리"
excerpt: "함수"
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

# 함수
함수는 하나의 단위로 실행되는 문의 집합이다. 자바스크립트에서의 함수는 강력함과 표현성의 근간이다.

---

# 함수 형태
모든 함수에는 바디가 있다. 

```
function sayHello(){
    //함수 바디는 여는 중괄호로 시작

    console.log("Hello world!");
    console.log("i Hola mundo!");

    //닫는 중괄호르 끝납니다.
}
```

# 호출과 참조
**함수도 객체**이다. 따라서 `다른 객체와 마차간지로 넘기거나 할당할 수 있다.`
`함수 호출과 참조의 차이를 이해하는 것이 중요하다.` 함수 식별자 뒤에 괄호를 쓰면 함수를 호출하는 것이고 바디를 실행한다
괄호를쓰지않고 함수이름만 쓴다면 함수를 참조하는 것이며 그 함수는 실행되지 않는다.

```
getGreeting();  //"Hello world!";
getGreeting;    //function getGreeting();
```

함수를 호출하지 않고 다른 값과 마찬가지로 참조만 한다는 특징은 자바스크립트를 매우 유연한 언어로 만든다.
예를 들어 함수를 변수에 할당하면 다른 이름으로 함수를 호출할 수 있다.

```
cons f = getGreeting;   // f에 getGreeting 함수를 담음
f();                    // "Hello world!"
```

배열 요소로도 할당 가능하다.

```
const arr = [1,2,3];
arr[1] = getGreeting;   //arr의 첫번째 인덱스에 함수할당
arr[1]();               //"Hello world!"
```


# 함수와 매개변수
함수 안에서 매개변수에 값을 할당해도 함수 바깥에 있는 어떤 변수에도 아무런 영향이 없다.
하지만 `함수 안에서 객체 자체를 변경하면, 그 객체는 함수 바깥에서도 바뀐 점이 반영된다.`

```
function f(o){
    o.message = `f 안에서 수정함 (이전 값: `${o.message}')`;
}
let o = {
    message: "초기 값"
}
console.log(`f를 호출하기 전: o.message="${o.message}"`);
f(o);
console.log(`f를 호출한 다음: o.message="${o.message}"`);
```

위 코드의 실행의 결과는 다음과 같이 나온다

```
f를 호출하기 전 : o.message="초기 값"
f를 호출한 다음:  o.message="f 안에서 수정함(이전 값: '초기 값')"
```

보다싶이 객체는 함수안에서도 수정이 가능해진다.


# 객체의 프로퍼티인 함수
객체의 프로퍼티인 함수를 **메서드(method)**라고 불러서 일반적인 함수와 구별합니다.
객체 리터럴에서도 메서드를 추가할 수 있습니다.

```
const o = {
    name: 'Wallace',                    //원시 값 프로퍼티
    bark: function(){ return 'Woof!';}, //함수 프로퍼티(메서드)
}
```

ES6에서는 간편하게 메서드를 추가할 수 있는 문법이 새로 생겼다.
위의 예제와 동일한 내용이다

```
const o = {
    name: 'Wallace',
    bark(){ return 'Woof!;},
}
```

# 참고문헌
- *러닝 자바스크립트: ES6로 제대로 입문하는 모던 자바스크립트 웹 개발*





