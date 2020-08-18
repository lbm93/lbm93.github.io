---
layout: article
title: "배열검색"
date: 2019-10-29 18:00:32 +0900
categories: [development, javascript]
# description: "웹 통신 프로토콜인 URL, HTTP, SMTP, MIME, FTP 을 정리"
excerpt: "배열검색"
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

# 배열검색
배열 안에서 뭔가 찾으려 할 때는 몇가지 방법이 있다. 그 방법들에 대하여 알아본다.

---

# indexOf, lastIndexOf
indexOf 메서드는 찾고자 하는 것과 정확히 일치(===)하는 첫 번째 요소의 인덱스를 반환한다.  
그리고 lastIndexOf는 배열의 끝에서부터 검색해 일치(===)하는 요소의 인덱스를 반환한다.
바로 예를 살펴보자.  

```javascript
let c = "Hello world,Helloi work";
c.indexOf("Hello");                       //0
c.lastIndexOf("Hello");                  //12
```  
위의 코드를 보면 감이 오나? 말 그대로 indexOf는 0번째 요소에서 Hello의 시작인 H를 찾았기때문에 그 위치를 반환한다.  
lastIndexOf는 뒤에서부터 찾기 때문에 Helloi를 먼저 발견해 Helloi의 시작요소인 12를 반환한다.  
다음 예제도 보자.  

```javascript
 const o = {name : "Jerry"};
 const arr = [1,5,"a", o, true, 5, [1,2], "9"];
 console.log(arr);

 console.log(arr.indexOf(5));                //1
 console.log(arr.lastIndexOf(5));            //5
 console.log(arr.indexOf("a"));              //2
 console.log(arr.lastIndexOf("a"));          //2
 console.log(arr.indexOf({name: "Jerry"}));  //-1
 console.log(arr.indexOf(o));                //3
 console.log(arr.indexOf([1,2]));            //-1
 console.log(arr.indexOf("9"));              //7
 ```


# find, findIndex
일단 find와 findIndex는 매개변수로 콜백함수를 사용한다.  
그리고 findIndex는 indexOf와 마찬가지로 조건에 맞는 인덱스 요소를 찾을때 알맞고, 찾는 결과가 없을시 -1을 반환한다.     
find는 요소 자체를 찾을때 알맞는 기능이다. find는 검색 결과가 없을시 undefined를 반환한다.
예제를 살펴보자    

```javascript
const arr2 = [{id : 5, name : "Judith"}, {id : 7, name : "Francis"}];
arr2.findIndex(o => o.id === 5);                                //0
arr2.findIndex(o => {o.name === 3});                            //-1
arr2.findIndex(o=> o.name === "Francis");                       //1

arr2.find(o => o.id === 5);                                     //{id : 5, name : "Judith"}     
arr2.find(o => o.id === 13);                                    //undefined  
```  


마지막으로는 find를 이용한 ID를 조건으로 객체를 검색하는 방법의 예시이다.

```javascript
class Person{
    constructor(name){
        this.name = name;
        this.id = Person.nextId++;
    }
}
Person.nextId = 0;
const jamie = new Person("Jamie"),
juliet = new Person("Juliet"),
peter = new Person("Peter"),
jay = new Person("Jay");

const arr = [jamie, juliet, peter, jay];

//옵션 1: ID를 직접 비교하는 방법
arr.find(p => p.id === juliet.id);                               //juliet 객체

//옵션 2: "this" 매개변수를 이용하는 방법
arr.find(function (p) 
{
    return p.id === this.id
},juliet);

```


# some,every
some과 every는 조건을 만족하는 요소가 있는지 없는지만 알면 충분할때 사용되는 메서드들이다.  
some과 every는 요소가 있다면 true, 없다면 false를 반환한다.  

**some예제:**  

```javascript
const arr = [5,7,12,15,17];
arr.some(x=>x%2 === 0);                         //true, 12는 짝수
arr.some(x=> Number.isInteger(Math.sqrt(x)));  //false, 제곱수가 없다.
```  

**every예제:**  

```javascript
const arr = [4,6,16,36];
arr.every(x=>x%2 === 0)                         //true, 홀수가 없다
arr.every(x=>Number.isInteger(Math.sqrt(x)));  //false, 6은 제곱수가 아니다.
```  

# 참고문헌
- *러닝 자바스크립트: ES6로 제대로 입문하는 모던 자바스크립트 웹 개발*