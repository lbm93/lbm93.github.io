---
layout: article
title: "Map"
date: 2019-11-06 18:00:32 +0900
categories: [development, javascript]
# description: "웹 통신 프로토콜인 URL, HTTP, SMTP, MIME, FTP 을 정리"
excerpt: "Map"
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

# Map 데이터구조
Map은 ES6에서 새로 도입한 데이터 구조이다. 이 구조에 대하여 알아보자.  

---

# 맵(Map)
ES6이전에는 키와 값을 연결하려면 객체를 사용해야 했다. 하지만 객체를 이런 목적으로 사용하면 여러가지 단점이 생긴다.  

>프로토타입 체인 때문에 의도하지 않은 연결선이 생길 수 있다.

>객체 안에 연결된 키와 값이 몇개나 되는지 쉽게 알 수 없다.

>키는 반드시 문자열이나 심볼이어야 하므로 객체를 키로 써서 값과 연결할 수 없습니다.

>객체는 프로퍼티 순서를 전혀 보장하지 않는다.  

이전의 ES5로 객체로 Key, Value는 다음과 같이 만들었다.

```javascript
const a = {
    name : "홍길동",
    age : 14
}
```


하지만 이제는 MAP으로 Key와 Value를 연결시켜 더욱 다양하게 사용할 수 있다. 이제 Map에있는 메서드들을 보자.

# set()
set()은 Map에 Key,value쌍으로 저장하는 메서드이다.  

```
Map.prototype.set(key, value)

```

# get()
get()은 Map에서 Key에 해당하는 Value를 가져오는 메서드이다.

```
Map.prototype.get(key)
```

# has()
has()는 Map에 해당하는 Key가 있는지 확인하는 메서드이다.

```
Map.prototype.has(key);
```

# size()
size()는 Map의 요소 숫자를 반환하는 메서드이다.

```
Map.prototype.size(MapInstance)
```

# keys()
keys()는 Key 값을 반환하는 메서드이다.

```
Map.prototype.key()
```

# values()
values()는 value를 반환하는 메서드이다.

```
Map.prototype.values()
```
   
```javascript
const u1 = {name : 'Cynthia'};
const u2 = {name : 'Jackson'};
const u3 = {name : 'Olive'};
const u4 = {name : 'James'};

//먼저 맵을 만든다
const UserRoles = new Map();

//맵의 set()메서드를 사용해 세팅한다
UserRoles.set(u1, 'User');  //Key Cynthia, Value User
UserRoles.set(u1, 'User');
UserRoles.set(u1, 'Admin');

//맵의 key에 해당하는 value를 얻어올땐 get()메서드를 사용
console.log(userRoles.get(u2));

//Map에서 has()메소드는 u1이라는 키가 있나 확인하는 메서드
console.log(userRoles.has(u1));
console.log(userRoles.get(u1));

//Map에서 size메소드는 Map의 요소 숫자를 반환
console.log(userRoles.size);

//Map에서 keys()는 키, values()는 값을 반환
for(let n of userRoles.keys()){
    console.log(n.name);
}
console.log('----');
for(let n of userRoles.values()){
    console.log(n);
}
console.log('----');

//Map에서 entrise()는 Key Value 다 가져옴
for(let ur of userRoles.entries()){
    console.log(`${ur[0].name}: ${ur[1]}`);
}

//Map 분해
for(let [u, r] of userRoles.entries())
    console.log(`${u.name}: ${r}`);

//entrise() 메서드는 맵의 기본 이터레이터이다
```

# 참고문헌
- *러닝 자바스크립트: ES6로 제대로 입문하는 모던 자바스크립트 웹 개발*