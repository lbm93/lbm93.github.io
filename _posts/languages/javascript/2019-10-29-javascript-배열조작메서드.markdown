---
layout: article
title: "배열 조작 메서드"
date: 2019-10-29 18:00:32 +0900
categories: [development, javascript]
# description: "웹 통신 프로토콜인 URL, HTTP, SMTP, MIME, FTP 을 정리"
excerpt: "배열 조작 메서드"
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

# 배열 조작 메서드
배열을 선언하고나면 그 배열에 프로퍼티로 여러가지 메서드들이 있다.  
그 메서드들에 대하여 사용방법을 알아보자.  

---

# push,pop
push와 pop은 많이 들어 봤을 것이다. 바로 stack의 메커니즘에서 push와 pop이 사용된다.  
작동 방법은 stack과 똑같다. push하면 맨끝요소 다음에 새로운 값을 집어넣고, pop하면 맨끝의 요소를 삭제한다.  

```javascript
//push,pop 뒤에서 추가 삭제
const arr1 = ["a","b","c"];
arr1.push("d");                                          //["a","b","c","d"]
arr1.pop();                                              //["a","b","c"]
```  


# unshift, shift
unshift,shift는 push,pop과 반대로다 unshift를 사용하면 맨 앞 요소에 새로운 값을 추가하고 shift를하면 맨 앞 요소를 삭제한다.  

```javascript
//unshift,shift 앞에서 추가 삭제
arr1.unshift("o");                                       //["o","a","b","c"]
arr1.shift();                                            //["a","b","c"]
```  


# concat
concat은 배열의 끝에 여러 요소를 추가하고 그 **사본**을 반환한다. `즉 원본은 그대로 유지된다.`

```javascript
const arr2 = [1,2,3];

arr2.concat(4,5,6);                                      //반환값 [1,2,3,4,5,6] , arr 값 [1,2,3]                       
arr2.concat([4,5,6]);                                    //반환값 [1,2,3,4,5,6] , arr 값 [1,2,3]  
arr2.concat([4,5],6);                                    //반환값 [1,2,3,4,5,6] , arr 값 [1,2,3]  
arr2.concat([4,[5,6]]);                                  //반환값 [1,2,3,4,[5,6]] , arr 값 [1,2,3]  
```  

# slice
slice는 배열 일부를 가져온다. 첫번째 매개변수는 어디서부터, 두번째 매개변수는 어디까지, 음수를 쓰면 반대로 가져온다.  
첫번째 매개변수만 사용 할 경우 끝까지 가져온다.  

```javascript
const arr3 = [1,2,3,4,5];

arr3.slice(3);                                           //[4,5], arr 값 [1,2,3,4,5]
arr3.slice(2,4);                                         //[3,4], arr 값 [1,2,3,4,5]
arr3.slice(-2);                                          //[4,5], arr 값 [1,2,3,4,5]
arr3.slice(1,-2);                                        //[2,3], arr 값 [1,2,3,4,5]
arr3.slice(-2,-1);                                       //[4], arr 값 [1,2,3,4,5]
```

# splice
splice는 배열을 자유롭게 수정한다. 첫번째 매개변수는 수정을 시작 할 인덱스, 두번째 매개변수는 제거할 요소 숫자(제거하지 않을때는 0을 기입)  

```javascript
const arr4 = [1,5,7];
arr4.splice(1,0,2,3,4);                                  //[] 제거 없음, arr의 값 [1,2,3,4,5,7]
arr4.splice(5,0,6);                                      //[] 제거 없음, arr의 값 [1,2,3,4,5,6,7]
arr4.splice(1,2);                                        //[2,3] 제거, arr의 값 [1,4,5,6,7]
arr4.splice(2,1,'a','b');                                //[5] 제거, arr의 값 [1,4,'a','b',7]
```  


# fill
fill은 ES6에 도입한 새로운 메서드이다. fill은 정해진 값으로 배열을 채운다. Array 생성자와 잘 어울린다.  

```javascript
const arr5 = new Array(5).fill(1);                       //[1,1,1,1,1]로 초기화
arr5.fill("a");                                          //["a","a","a","a","a"]
arr5.fill("b",1);                                        //["a","b","b","b","b"]
arr5.fill("c",2,4);                                      //["a","b","c","c","b"]
arr5.fill(5.5,-4);                                       //["a",5.5,5.5,5.5,5.5]
```  

# reverse 
이름 그대로 배열 요소의 순서를 반대로 바꿔 수정한다.

```javascript
const arr6 = [1,2,3,4,5];
arr6.reverse();                                          //[5,4,3,2,1]

```  


# sort
이름 그대로 배열 요소의 순서를 정렬한다.

```javascript
const arr7 = [5,3,2,4,1];
arr7.sort();                                              //[1,2,3,4,5]
```  

이러한 특징은 배열안에 객체들도 이름으로 정렬이 가능하다.  

```javascript
const arr8 =[{name:"Suzanne"},{name:"Jim"},{name:"Trevor"},{name:"Amanda"}];
arr8.sort();
arr8.sort((a,b) => a.name > b.name);                       //name의 프로퍼티의 알파벳 순으로 정렬
/*function f(){
    arr8.sort();
    return arr8.sort(function(a,b){return a.name > b.name;});
}*/
arr8.sort((a,b) => a.name[1] < b.name[1]);                 //name의 프로퍼티의 알파벳 두번째 알파벳 역순으로 정렬

```

# 참고문헌
- *러닝 자바스크립트: ES6로 제대로 입문하는 모던 자바스크립트 웹 개발*