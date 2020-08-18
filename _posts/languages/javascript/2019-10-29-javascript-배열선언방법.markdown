---
layout: article
title: "배열 선언 방법"
date: 2019-10-29 18:00:32 +0900
categories: [development, javascript]
# description: "웹 통신 프로토콜인 URL, HTTP, SMTP, MIME, FTP 을 정리"
excerpt: "배열 선언 방법"
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

# 배열 선언 및 사용법 코드
이 코드는 배열을 선언하고 사용하는 기초적인 여러 방법들을 적어놓은 코드이다.

```javascript
//배열 리터럴
const arr1 = [1,2,3];                                       //숫자로 구성된 배열
const arr2 = ["one", 2, "tree"];                            //비균질적 배열
const arr3 = [[1,2,3], ["one",2,"tree"]];                   //배열을 포함한 배열
const arr4 = [                                              //비균질적 배열
    {name : "Fred", type: "object", luckyNumbers:[5,7,13]}, //[0] 객체
    [                                                       //[1] 배열
        {name:"Susan", type:"object"},                      //[1][0] 객체
        {name:"Anthony", type:"object"},                    //[1][1] 객체
    ],
    1,                                                      //[2] 숫자
    function(){return "arrays can contain functions too";}, //[3] 함수
    "three",                                                //[4] 문자열
];

//배열 요소에 접근하기
console.log(arr1[0]);                                       //1
console.log(arr1[2]);                                      //3
console.log(arr3[1]);                                       //["one",2,"tree"]
console.log(arr4[1][0]);                                    //{name:"Susan", type:"object"}

//배열 길이
console.log(arr1.length);                                   //3
console.log(arr4.length);                                   //5
console.log(arr4[1].length);                                //2

//배열 길이 늘리기
arr1[4] =5;
arr1;                                                        //[1,2,3,undefined,5]
arr1.length;                                                 //3

//배열의 현재 길이보다 큰 인덱스에 접근하는 것만으로 배열의 길이가 늘어나지 않습니다.
arr2[10];                                                    //undefined
arr2.length;                                                 //5

//Array 생성자(거의 사용하지 않습니다.)
const arr5 = new Array();                                    //빈배열
const arr6 = new Array(1,2,3);                               //[1,2,3]
const arr7 = new Array(2);                                   //길이가 2인 배열, 요소는 모두 undefined입니다.
const arr8 = new Array("2");                                 //["2"]

```

# 참고문헌
- *러닝 자바스크립트: ES6로 제대로 입문하는 모던 자바스크립트 웹 개발*