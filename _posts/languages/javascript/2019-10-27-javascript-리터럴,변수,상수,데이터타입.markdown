---
layout: article
title: "리터럴과 변수, 상수, 데이터 타입"
date: 2019-10-27 18:00:32 +0900
categories: [development, javascript]
# description: "웹 통신 프로토콜인 URL, HTTP, SMTP, MIME, FTP 을 정리"
excerpt: "리터럴과 변수, 상수, 데이터 타입"
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


# 리터럴과 변수, 상수, 데이터 타입
자바스크립트가 데이터를 보관하는 메커니즘에는 변수(Variable), 상수(Constant), 리터럴(Literal)이 있다.

---

# 변수와 상수

변수(Variable)은 이름이 붙은 값이다. 변수라는 말 그대로 값이 언제든지 바뀔수 있다.

```
let currentTempC = 22; //let키워드는 ES6부터 사용
currentTempC = 25;     //정상적으로 값이 바뀜
```

상수(Constant)는 ES6에서 새로 생겼다. 상수도 마찬가지로 값을 할당받을 수 있지만, 한 번 할당한 값을 바꿀수는 없다.

```
const ROOT_TEMP_C = 21.5;   //상수 ROOT_TEMP_C 값 고정
ROOT_TEMP_C = 24.5;         //컴파일 에러
```


# 변수와 상수 중 어떤 것을 써야 할까?

될 수 있으면 변수보다 상수를 써야한다. 데이터의 값이 아무 때나 막 바뀌는 것보다는, 고정된 값이 이해하기 쉽다.
상수를 사용하면 값을 바꾸지 말아야 할 데이터에서 실수로 값을 바꿀일이 줄어든다.



# 리터럴

리터럴(Literal)은 값을 프로그램 안에서 직접 지정한다는 의미이다. 즉, 리터를은 값을 만드는 방법이다.
쉽게 생각해 우리는 변수 상수를 배울때 Rvalue와 Lvalue를 배운다

```
let room1 = "conference_room_a;     //"conference_room_a"(따움표 안)은 리터럴입니다.
let currentRoom = room1;            //이제 currentRoom의 값은 room1의 값
```

# 참고문헌
- *러닝 자바스크립트: ES6로 제대로 입문하는 모던 자바스크립트 웹 개발*
