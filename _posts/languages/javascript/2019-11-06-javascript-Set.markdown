---
layout: article
title: "Set"
date: 2019-11-06 18:00:32 +0900
categories: [development, javascript]
# description: "웹 통신 프로토콜인 URL, HTTP, SMTP, MIME, FTP 을 정리"
excerpt: "Set"
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

# Set 데이터구조
셋은 중복을 허용하지 않는 데이터 집합이다.


# 셋(Set)
Set은 데이터 타입 중의 하나인데, 중복되는 값을 가지지 않는 값들의 리스트입니다. 그리고 이 때 값은 순서가 존재하지 않는다.



# add()
add()메서드는 Set 데이터 구조에 데이터를 추가하는 것이다.

```
Set.prototype.add(Data)
```

# size()
size()메서드는 Set 데이터 구조에 요소의 갯수를 가져온다.

```
Set.prototype.size(SetInstance)
```

# has()
has()메서드는 Set 데이터 구조에 해당 데이터가 있는지 확인한다.

```
Set.prototype.has(Data)
```

# delete()
delete()메서드는 Set 데이터 구조에 해당 데이터를 지울 때 사용한다.

```
Set.prototype.delete(Data)
```


```javascript
const roles = new Set();

roles.add("User");      //Set["User"]
roles.add("Admin");     //Set["User","Admin"]
console.log(roles);
roles.size;             //2

//셋은 추가하려는것이 셋에 이미 있는지 확인할 필요가 없다 있다면 아무 일도 일어나지 않기 떄문이다

roles.add("User");      //Set["User","Admin"]
roles.size;             //2

//제거는 delete 메서드를 사용 반환값 true, false
roles.delete("Admin");  //Set["User"]
roles.has("Admin")      //false

```

# 참고문헌
- *러닝 자바스크립트: ES6로 제대로 입문하는 모던 자바스크립트 웹 개발*