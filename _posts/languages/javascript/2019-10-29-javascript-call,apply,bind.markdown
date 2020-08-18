---
layout: article
title: "call,apply,bind"
date: 2019-10-29 18:00:32 +0900
categories: [development, javascript]
# description: "웹 통신 프로토콜인 URL, HTTP, SMTP, MIME, FTP 을 정리"
excerpt: "call,apply,bind"
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
# call과 apply와 bind
call과 apply와 bind는 this를 어디에 동적으로 어떤 객체에 묶고싶거나 정적으로 고정시키고 싶을때 사용되는 함수들이다.

---

# call과 apply와 bind의 기능
여기서는 call과 apply와 bind가 어떤 기능을 하는지 설명만 적어 놓겠다. 어떤 기능을 하는지 알고 뒤에 예제를 풀어본다.  
  
- **call:**

|<center>call</center>|<center>매개변수</center>|
|:---:|:---:|
|<center>call은 this를 특정값으로 지정</center>|<center>첫 번째 this로 사용할 값, 두번째 부터는 호출하는 함수로 전달</center>|  
  

- **apply:**

|<center>apply</center>|<center>매개변수</center>|
|:---:|:---:|
|<center>apply는 call과 같은데 매개변수를 처리하는 방식이 다르다.</center>|<center>첫 번째 this로 사용할 값, 두번째 부터는 배열을 호출하는 함수로 전달</center>|   

- **bind:**

|<center>bind</center>|<center>매개변수</center>|
|:---:|:---:|
|<center>this 값을 영구히 바꾼다.</center>|<center>첫 번째 this로 사용할 값, 두번째 부터는 함수로 전달</center>| 

  
그럼 이제 자세히 알아보자.



# call
*call 메서드는 모든 함수에서 사용가능하며, this를 특정 값으로 지정할 수 있다.*  
예제를 살펴보자.  

```javascript
const bruce = {name: Bruce};
const madeline = {name: Madeline};

//이 함수는 어떤 객체에도 연결되지 않았지만 this를 사용한다.
function greet(){
    retrun `Hello, I'm ${this.name}!`;
}

greet();                //"Hello, I'm undefined!" - this는 어디에도 묶이지 않음
greet.call(bruce);      //"Hello, I'm Bruce!" - this는 Bruce이다.
greet.call(madeline);   //"Hello, I'm Madeline!" - this는 Madeline이다.
```  

위의 예제처럼 call 메서드의 매개변수에 bruce나 madeline을 넣어 this를 객체에 묶어버린다.  
위의 예제는 일반적인 call 메서드를 호출 했을때 this가 어디에 묶이나 확인한 기본 예제이고  
매개변수를 더 추가해 함수로 전달하는 예제를 살펴보자.위의 예제를 이어 작성하겠다.  

```javascript
function update(birtYear, occupation){
    this.birthYear = birthYear;
    this.occupation = occupation;
}

update.call(bruce,1949,'singer');
//burce는 이제 {name : 'bruce', birthYear:1949, occupation:'singer'}이 된다.
update.call(madeline,1944,'actree');
```  

# apply
*apply메서드는 함수 매개변수를 처리하는 방법을 제외하면 call과 완전히 같다.*  
call은 함수의 매개변수를 직접적으로 처리하지만 apply는 배열로 처리한다.  

예제를 살펴보자.  

```javascript
update.apply(bruce,[1955,"actor"]);
//burce는 이제 {name : 'bruce', birthYear:1955, occupation:'actor'}이 된다.
```

그럼 똑같이 this를 동적으로 묶어주고 싶을때 call과 다른게 함수로 전달되는 매개변수를 처리하는 방식이 다른거뿐인가?  
그렇다 하지만 call보다 유용한점은 함수로 배열을 전달하기때문에 배열의 특정 값을 추출하는데 유리하다.  
예제를 살펴보자.  

```javascript
const arr = [2,3,-5,15,7];
Math.min.apply(null,arr);   //-5
Math.max.apply(null,arr);   //15
```  
여기서 apply를 사용할때 왜 첫번째 매개변수에 null을 넣었는가?  
Math.min은 this와 관계없이 동작하기 때문이다. 위의 update 메서드같은 경우에는 this가 있어야만 동작하기 때문에 넣어준 것이다.  


# bind
*bind는 this 값을 영구히 바꿀 수 있다. 즉 위에서 update를 요리조리 불러도 this의 값은 항상 bruce가 되게끔 할 수 있다.*  
바로 예제를 살펴보자.  

```javascript
const updateBruce = update.bind(bruce);

updateBruce(1904,"actor");
//burce는 이제 {name : 'bruce', birthYear:1904, occupation:'actor'}이 된다.
updateBruce.call(madeline,1274,"king");
//burce는 이제 {name : 'bruce', birthYear:1274, occupation:'king'}이 된다.
//madeline은 변하지 않았다.
//또는 bruce의 생일도 같이 고정하고 싶으면 다음처럼 작성한다.
//const upadateBruce = update.bind(bruce,1904);
```  

# 참고문헌
- *러닝 자바스크립트: ES6로 제대로 입문하는 모던 자바스크립트 웹 개발*