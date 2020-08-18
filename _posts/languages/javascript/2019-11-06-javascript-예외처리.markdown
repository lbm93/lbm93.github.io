---
layout: article
title: "예외처리"
date: 2019-11-06 18:00:32 +0900
categories: [development, javascript]
# description: "웹 통신 프로토콜인 URL, HTTP, SMTP, MIME, FTP 을 정리"
excerpt: "예외처리"
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

# 예외처리
Javascript에서 클래스도 생성하고 객체도 생성하는데 예외처리는 없을까?  
당연히 예외처리가 있다. 우리는 예외처리를 통해 에러를 컨트롤한다.  
예외처리하는 방법에 대하여 알아보자.  

---

# Error 객체
자바스크립트에는 내장된 Error 객체가 있다. 이 객체를 통해 Error인스턴스를 만들어 에러 메시지를 저장 할 수 있다.

```javascript
const err = new Error('invalid email');
```

지금 위에서 만든 인스턴스는 에러와 통신하는 수단이다. 아래 코드는 예를 들어 이메일 주소의 유효성을 검사하는 함수가 있다 하자. 주소가 올바르면 주소를 반환하고, 바르지 않다면 Error 인스턴스를 반환하는 코드이다.

```javascript
//이메일 유효성을 검사하는 함수
function validateEmail(email){
    return remail.match(/@/)?
        email:
        new Error(`invalid email:${email}`);
}

const email = "jane@doe.em"

const validatedEmail = validateEmail(email);

if(validateEmail instanceof Error){
    console.error(`Error:${validatedEmail.message}`);
}else{
    console.log(`Valid email: ${validateEmail}`);
}
```

위 코드처럼 Error 인스턴스는 유효성을 검사하는 방법에도 많이 사용하지만 예외처리에 더 자주 사용된다.


# try/catch와 예외처리
try catch는 java에서도 나오는 개념이다 try는 예외가 발생할 코드를 집어 넣고 예외가 발생하면 catch로 잡는다. 바로 코드를 살펴보자. 위에서 사용했던 함수를 그대로 가져와 수정했다.

```javascript
const email = null;

try{
    const validatedEmail = validateEmail(email);
    //반환값이 Error 인스턴스인지 아닌지 확인
    if(validatedEmail instanceof Error){
        console.error(`Error:${validatedEmail.message}`);
    }else{
        console.log(`Valid email:${validatedEmail}`);
    }
}
catch(err){
    console.error(`Error:${err.message}`);
}
```

위 코드는 예외로 등록한 에러가 발생하면 catch를 수행하고 그렇지 않으면 catch는 수행되지 않고 넘어가게 된다.  


# 예외 일으키기
자바스크립트가 에러를 일으키기만을 기다릴 필요 없이 직접 에러를 일으켜서(Throw)예외처리 작업을 할 수도 있다.  

자바스크립트는 에러를 일으킬 때 꼭 객체만이 아니라 숫자나 문자열 등 어떤 값이든 catch 절에 넘길 수 있다. 하지만 Error 인스턴스를 넘기는게 가장 편리하다.  

아래 예제 코드는 은행 어플리케이션에 사용할 현금 인출 기능을 만든다고 생각하고 작성한 코드이다. 계좌의 잔고가 요청받은 금액보다 작으면 예외를 일으켜야한다.

```javascript
function billPay(amount, payee, account){
    if(amount > account.balance)
        throw new Error("insufficient funds");
    account.transfer(payee, amount);
}
```

위 코드에서는 계좌의 잔고가 요청받은 금액보다 작을 경우 Error 인스턴스를 생성해 throw한다. 즉, 예외를 일으킨다는 말이다. 그럼 다음 수행 문장인 account.transfer은 수행되지 않고 넘어가게 된다.



# 예외 처리와 호출 스택
자바스크립트는 프로그램이 함수를 호출하고, 그 함수는 다른 함수를 호출하고, 호출된 함수는 또 다른 함수를 호출하는 일이 반복된다. 자바스크립트 인터프리터는 이런 과정을 모두 추적하고 있어야 한다.  

예를 들어 함수 a가 함수 b를 호출하고 함수 b는 함수 c를 호출한다면, 함수 c가 실행을 마칠 때 실행 흐름은 함수b로 돌아간다. 그리고 b가 실행을 마칠 때 실행 흐름 함수 a로 돌아간다. 바꿔 말해, c가 실행 중일 때는 a와 b는 완료될 수 없다.
이렇게 완료되지 않은 함수가 쌓이는 것을 `호출 스택(call stack)`이라 한다.  

그럼 당연히 c에서 에러가 난다면 b가 c가 반환하는 값을 리턴한다면 b도 에러가나고 a도 에러가난다. 즉, 에러는 캐치될 때까지 호출 스택을 따라 올라간다.

하지만 `에러를 캐치하게되면 a가 함수 b를 호출하고 b가 호출한 c에서 에러가 발생한다면, 호출 스택은 c에서 일어난 에러를 보고하는데 그치지 않고 b가 c를 호출했으며 b는 a에서 호출했다는 것도 함께 알려준다.`

대부분의 자바스크립트 환경에서 Error 인스턴스에는 스택을 문자열로 표현한 stack 프로퍼티가 있다. 바로 코드를 보자.

```javascript
function a(){
    console.log('a:calling b');
    b();
    console.log('a: done');
}

function b(){
    console.log('b:calling c');
    c();
    console.log('b: done');
}

function c(){
    console.log('c:throwing error');
    //함수 c가 호출되면 error인스턴스를 던진다.
    throw new Error('c error');
    console.log('c:done');
}

function d(){
    console.log('d:calling c');
    c();
    console.log('d:done');
}

try{
    a();
}catch(err){
    console.log(err.stack);
}

try{
    d();
}catch(err){
    console.log(err.stack);
}

```

# 참고문헌
- *러닝 자바스크립트: ES6로 제대로 입문하는 모던 자바스크립트 웹 개발*