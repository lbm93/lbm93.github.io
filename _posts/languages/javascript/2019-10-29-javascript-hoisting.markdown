---
layout: article
title: "호이스팅(hoisting)"
date: 2019-10-29 18:00:32 +0900
categories: [development, javascript]
# description: "웹 통신 프로토콜인 URL, HTTP, SMTP, MIME, FTP 을 정리"
excerpt: "호이스팅(hoisting)"
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

# 호이스팅(hoisting)
ES6에서 let을 도입하기 전에는 var를 써서 변수를 선언했고, 이렇게 선언된 변수들은 함수 스코프라 불리는 스코프를 가졌습니다.  
let으로 변수를 선언하면, 그 변수는 선언하기 전에는 존재하지 않습니다.  

```javascript
x;          //ReferenceError : x는 정의되지 않았습니다.
let x = 3;  //에러가 일어나서 실행이 멈췄으므로 여기에는 결코 도달할 수 없습니다.
```  

```javascript
x;          //undefined
var x = 3;
x;          //3
```  


위의 두 코드에서 let과 var로 변수를 선언했을때의 차이가 보이십니까?  
let은 let으로 변수를 선언하기도 전에 변수를 사용하지 못하지만 var는 사용이 가능합니다.  
이게 가능한 이유는 바로 **호이스팅(hoisting)**이라는 메커니즘때문입니다.  
  
`자바스크립트는 함수나 전역 스코프 전체를 살펴보고 var로 선언한 변수를 맨위로 끌어올립니다.`  

여기서 중요한 것은 선언만 끌어올리고, 할당은 끌어올려지지 않습니다.  
위의 var를 예제로 삼은 코드를 다시 hoisting이됬다 가정해서 작성하면 다음과 같이 동작합니다.  

```javascript
var x;
x;          //undefined
x = 3;
x;          //3
```    

그런대 이러한 코딩 스타일은 복잡할뿐더러 좋지 않습니다. 그런대 왜 이러한 방식의 hoisting을 알아야할까요?  
바로 자바스크립트의 ES6방식이 아직 어디에서든 쓸려면 시간이 필요로하기 때문입니다.  
그때문에 기존에 작성했던 코드들은 대부분 ES5방식이였으므로 `트랜스컴파일`을 하기위해서는 알아야겠죠?



# 함수 호이스팅
추가적으로 함수도 호이스팅이 되는거 아시나요? 변수에 함수 표현식을 할당 할 수 있죠? `그러나 변수에 할당한 함수 표현식은 끌어올려지지 않습니다.`  
예제를 살펴보죠.  

```javascript
f();                //'f'
function f(){
    console.log('f');
}
```  

```javascript
f();                //ReferenceError:f는 정의되지 않았습니다
let f = function(){
    console.log('f');
}
```

두개의 예제가 어떤 차이가 있는지 꼭 한번 확인해주세요!!

# 참고문헌
- *러닝 자바스크립트: ES6로 제대로 입문하는 모던 자바스크립트 웹 개발*