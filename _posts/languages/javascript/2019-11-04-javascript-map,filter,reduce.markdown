---
layout: article
title: "map, filter, reduce"
date: 2019-11-04 18:00:32 +0900
categories: [development, javascript]
# description: "웹 통신 프로토콜인 URL, HTTP, SMTP, MIME, FTP 을 정리"
excerpt: "map, filter, reduce"
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

# map(), filter(), reduce()
map과 filter, reduce는 배열 메서드 중 가장 유용한 메서드이다.  
이 유용한 메서드들에 대하여 간단하게 알아보자.  

---

# map()
map 메서드는 배열 요소를 숫자든 문자든 어떠한 것으로라도 변형을 시켜준다.  
그래서 일정한 형식의 배열을 다른 형식의 배열로 바꿔야 한다면 map 메서드를 사용하면된다.  
`map 메서드는 인자로 콜백함수를 요구하고, 그 콜백함수의 매개변수로는 요소 자체와 요소 인덱스, 배열 전체를 매개변수로 받는다.`  

```javascript
const cart = [{name : "Widget", price : 99.5}, {name : "Gadget", price : 22.95}];
const names = cart.map(x=>x.name);                  //[ 'Widget', 'Gadget' ]
const prices = cart.map(x=>x.price);                //[ 99.5, 22.95 ]
const discountPrices = prices.map(x => x*0.8);      //[ 79.60000000000001, 18.36 ]
```
  
이 때 가장 중요한 것은 위 소스코드에서 cart라는 배열은 변하지 않는다.  
즉, map 메서드를 수행했을때 결과 값은 사본이 전달된다는 것이다.  

위의 소스코드는 요소 자체를 매개변수로 받았을때고 밑의 소스코드를 보면 조금 더 map을 자세하게 알 수 있다.

```javascript
const items = ["Widget", "Gadget"];                                 
const price = [9.95, 22.95];                                        
const carts = items.map((x,i) => ({name : x, price : price[i]}));   //[ { name: 'Widget', price: 9.95 },{ name: 'Gadget', price: 22.95 } ]
```

위의 소스코드는 요소자체(x)와 인덱스(i)도 사용했다. 인데스를 사용한 이유는 items의 요소와 prices의 요소를 인덱스에 따라 결하하기 위해서이다.
여기서 map은 다른 배열에서 정보를 가져와서 문자열로 이루어진 배열을 객체 배열로 변형했다.  



# filter()
filter 메서드는 이름이 암시하듯이 배열에서 필요한 것들만 남길 목적으로 만들어졌다.  
map과 마찬가지로 사본을 반환하며 새 배열에는 필요한 요소만 냄겨둔다.  
이 필요한 요소를 남겨두는데는 어떤 요소를 남길지 판단하는 콜백함수를 넘깁니다.

```javascript
const cards = [];
for(let suit of ['H','C','D','S'])
    for(let value=1; value<=13; value++)
        cards.push({suit,value});

console.log(cards);

//value가 2인 카드
cards.filter(c => c.value === 2);
console.log(cards.filter(c => c.value === 2));

//다이아몬드
cards.filter(c => c.suit === 'D');
console.log(cards.filter(c => c.value === 2));

//킹,퀸,주니어
cards.filter(c => c.value > 10);
console.log(cards.filter(c => c.value === 2));
```



# reduce()
지금까지 map과 filter를 통해 배열의 각 요소를 변경했다면 reduce는 배열 자체를 변영하는 배열의 메서드이다. 이 메서드는 보통 배열을 값 하나로 줄이는데 쓰인다. reduce는 map이나 filter와 마찬가지로 콜백함수를 받는다.  그러나 map과 filter는 첫 번째 매개변수는 항상 현재 배열 요소였지만, reduce는 다르다. reduce는 첫 번째 매개변수로 배열이 줄어드는 대상인 누적값이다.
두 번째 매개변수부터는 현재 배열 요소, 현재 인덱스, 배열 자체이다. 또 reduce는 초깃값도 옵션으로 받을 수 있다.

```javascript
const arr = [5,7,2,4];
const sum = arr.reduce((a,x) => a+= x, 0);
```
위의 소스코드를 보며 이해를 하자. 먼저 reduce가 호출될때 화살표함수로 콜백함수를 호출한다. 이때 매개변수로는 누적값이 저장될 변수 a, 그리고 배열 요소 시작은 0번부터이니 5가 될 것이다.그리고 마지막으로 0은 a의 초기값을 말한다. reduce가 호출되는 과정은 다음과 같다.

1. 첫 번째 배열 요소 5에서 (익명) 함수를 호출, a의 초깃값은 0이고 x의 값은 5, 함수는 a와 x(5)의 합을 반환
2. 두 번째 배열 요소 7에서 (익명) 함수를 호출, a의 초깃값은 5이고 x의 값은 7, 함수는 a와 x(7)의 합(12)를 반환  

이런 식으로 진행이 된다.  

# 참고문헌
- *러닝 자바스크립트: ES6로 제대로 입문하는 모던 자바스크립트 웹 개발*