---
layout: article
title: "객체지향 프로그래밍(상속성, 다형성)"
date: 2019-11-04 18:00:32 +0900
categories: [development, javascript]
# description: "웹 통신 프로토콜인 URL, HTTP, SMTP, MIME, FTP 을 정리"
excerpt: "객체지향 프로그래밍(상속성, 다형성)"
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

# 객체지향 프로그래밍(상속성,다형성)
ES6에서는 자바스크립트에서 OOP 프로그래밍을 편리하게 하도록 문법을 추가하였다. OOP에서의 상속성,다형성에 대하여 알아보자.

---

# 상속
클래스의 인스턴스는 클래스의 기능을 모두 상속한다. 상속은 한 단계로 끝나지 않는다. 객체의 프로토타입에서 메서드를 찾지 못하면 자바스크립트는 프로토타입의 프로토타입을 검색한다.프로토타입 체인은 이렇게 만들어진다.바로 예제를 살펴보자.

```javascript
class Vehicle{
    constructor(){
        this.passengers = [];
        console.log("Vehicle created");
    }
    addPassenger(p){
        this.passengers.push(p);
    }
}

class Car extends Vehicle{
    constructor(){
        super();
        console.log("Car created");
    }
    deployAirbags(){
        console.log("BWOOSH!");
    }
}

const v = new Vehicle();
v.addPassengers("Frank");
v.addPassengers("Judy");
v.passengers;               //["Frank","Judy"];
const c = new Car;
c.addPassengers("Alice");   //["Alice"];
c.addPassengers("Cameron"); //["Alice","Cameron"]
c.passengers;
v.deployAirbags();          //ERROR
c.deployAirbags();          //"BWOOSH!"
```

위의 코드를 보면 Car 클래스는 Vechicle이라는 클래스를 상속받았다. 그 말은 Vechicle의 프로퍼티를 공유하게 되었다는 소리이다. 그래서 코드에서 보면 deployAirbags라는 메소드가 Vehicle에는 정의되어있지 않아 사용이 불가능하다. 그리고 Car 클래스의 `super()` 메소드는 슈퍼클래스(여기서 Vehicle이 슈퍼클래스)의 생성자를 호출하는 특별한 함수를 사용한다.
`서브클래스에서는 상속받으면 무조건 이 super()을 사용해야 코드가 제대로 동작한다.`


# 다형성
객체지향 언어에서 여러 슈퍼클래스의 멤버인 인스턴스를 가르킨다. 대부분 객체지향 언어에서 다형성은 특별한 경우에 속한다. 자바스크립트는 느슨한 타입을 사용하고 어디서든 객체를 쓸 수 있으므로, 어떤 면에서는 자바스크립트의 객체는 모두 다형성을 가지고 있다 할 수 있다. 자바스크립트 코드를 작성하다 보면 '이런 메서드가 있고 저런 메서드가 있으니 아마 그 클래스의 인스턴스일 것이다'처럼 짐작할 때가 많다. 자바스크립트에서는 객체가 클래스의 인스턴스인지 확인하는 insteadof 연산자가있다.

```javascript
class Motocycle extends Vehicle{}
const c = new Car();
const m = new Motocycle();
c instanceof Car;       //true
c instanceof Vehicle;   //true, Car는 Vehicle을 상속 받아서.
```

# 참고문헌
- *러닝 자바스크립트: ES6로 제대로 입문하는 모던 자바스크립트 웹 개발*