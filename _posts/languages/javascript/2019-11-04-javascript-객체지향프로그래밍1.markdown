---
layout: article
title: "객체지향 프로그래밍(클래스, 프로토타입, 정적메서드)"
date: 2019-11-04 18:00:32 +0900
categories: [development, javascript]
# description: "웹 통신 프로토콜인 URL, HTTP, SMTP, MIME, FTP 을 정리"
excerpt: "객체지향 프로그래밍(클래스, 프로토타입, 정적메서드)"
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
# 객체지향 프로그래밍(클래스,프로토타입,정적메서드)
ES6에서는 자바스크립트에서 OOP 프로그래밍을 편리하게 하도록 문법을 추가하였다. OOP에서의 클래스, 생성자, 프로토타입, 정적메서드등 알아보자.

---

# 클래스와 인스턴스 생성
다음은 자바스크립트에서의 클래스 생성 문법과 인스턴스 생성 문법이다.  

```javascript
class Car{
    constructor(make,model){
        this.make = make;
        this.model = model;
        this.userGears = ['P','N','R','D'];
        this.userGear = this.userGears[0];
    }
    shift(gear){
        if(this.userGears.indexOf(gear) < 0)
            throw new Error(`Invalid gear : ${gear}`);
        this.userGear = gear;
    }
}

const car1 = new Car("Tesla","Model S");
const car2 = new Car("Mazda", "3i");
```

기존의 OOP프로그래밍과 비슷하다. 대신 생성자 이름이 클래스 이름이 아닌 constructor라고 적어야한다.  
위의 코드를 보면 알겠지만 car1,car2 두개의 인스턴스를 생성했다.  
그리고 Car클래스의 메서드인 shift도 만든걸 볼 수 있다.  

자바스크립트의 클래스 안의 메서드로 인해 잘못된 값이 저장되는걸 방지할 수 있는 메커니즘이 없다. 
즉, shift 메서드를 통해 userGear를 'X'로 변경할 시에 이걸 방지해줄 수 있는 메커니즘이 없다는 것이다.  
이러한 문제점은 자바스크립트에서의 OOP 한계라고 한다.  


# 프로토타입(Prototype)
클래스의 인스턴스에서 사용할 수 있는 메서드라고 하면 그건 프로토타입(Prototype) 메서드를 말하는 것이다.
예를 들어 Car의 인스턴스에서 사용할 수 있는 shift 메서드는 프로토타입 메서드입니다. 프로토타입 메서드는 Car.prototype.shift처럼 표기할 때가 많다.
Array의 forEach를 Array.prototype.forEach 혹은 Array#forEach라고 쓰는 것과 마찬가지로 말이다.  

함수의 Prototype이 중요해지는 시점은 new 키워드로 새 인스턴스를 만들었을 때이다. new 키워드로 만든 새 객체는 생성자의 Prototype 프로퍼티에 접근할 수 있다.
프로토타입에서 중요한 것은 `동적 디스패치` 라는 메커니즘이다.  

여기서 디스패치는 메서드 호출과 같은 의미이다. 객체의 프로퍼티나 메서드에 접근하려 할 때 그런 프로퍼티나 메서드가 존재하지 않으면 자바스크립트는 객체의 프로토타입에서 해당 프로퍼티나 메서드를 찾는다. 클래스의 인스턴스는 모두 같은 프로토타입을 공유하므로 프로토타입에 프로퍼티나 메서드가 있다면 해당 클래스의 인스턴스는 모두 그 프로퍼티나 메서드에 접근할 수 있다.  

인스턴스에서 메서드나 프로퍼티를 정의하면 프로토타입에 있는 것을 가리는 효과가 있다. 자바스크립트는 먼저 인스턴스를 체크하고 거기에 없으면 프로토타입을 체크한다.

```javascript
const car1 = new Car();
const car2 = new Car();
car1.shift === Car.prototype.shift;     //true
car1.shift('D');                        
car1.userGear;                          //D
car1.shift === car2.shift;              //true

car1.shift = function(gear){this.userGear = gear.toUpperCase();}
car1.shift === Car.prototype.shift;     //false;
car1.shift === car2.shift;              //false;
car1.shift('d');
car1.userGear;                          //D
```

위 예제는 동적 디스패치를 어떻게 구현했는지 잘 보여준다. car1 객체에는 shift 메서드가 없지만, car.shift('D')를 호출하면 자바스크립트는 car1의 프로토타입에서 그런 이름의 메서드를 검색한다. car1에 shift 메서드를 추가하면 car1과 프로토타입에 같은 이름의 메서드가 존재하게 된다. 이제 car1.shift('d')를 호출하면 car1의 메서드가 호출되고 프로토타입의 메서드는 호출되지 않는다.  


# 정적메서드(static method)
지금까지 우리는 인스턴스 메서드, 즉 인스턴스에서 사용하게끔 만든 메서드를 주로 살펴봤다. 메서드에는 인스턴스 메서드 뿐만 아니라 정적 메서드도 존재한다.
정적 메서드에서 this는 인스턴스가 아니라 클래스 자체에 묶인다.하지만 일반적으로 정적 메서드에서는 this 대신 클래스 이름을 사용하는게 좋은 습관이다.
예제를 보자.

```javascript
class Car{
    static getNextVin(){
        return Car.nextVin++;       //this.netVin이라 적어도 됨.
    }
    constructor(make,model){
        this.make = make;
        this.model = model;
        this.vin = Car.getNextVin();
    }
    static areSimilar(car1,car2){
        return car1.make===car2.make&&car1.model===car2.model;
    }
    static arSame(car1,car2){
        return car1.vin===car2.vin;
    }
    
}

Car.nextVin = 0;

const car1 = new Car("Tesla","S");
const car2 = new Car("Mazda", "3");

car1.vin;           //0
car2.vin;           //1

Car.arSimilar(car1,car2)        //false
Car.areSame(car1,ca2)           //false

```

# 참고문헌
- *러닝 자바스크립트: ES6로 제대로 입문하는 모던 자바스크립트 웹 개발*
