---
layout: article
title: "File 다루기"
date: 2019-11-07 18:00:32 +0900
categories: [development, nodejs]
# description: "웹 통신 프로토콜인 URL, HTTP, SMTP, MIME, FTP 을 정리"
excerpt: "File 다루기"
image:
  teaser: posts/nodejs/nodejs.PNG
  credit: nodejs
  creditlink: https://nodejs.org/en/
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
- nodejs
- node.js
---
{% include toc.html %}

# 파일다루기
노드의 파일 시스템은 파일을 다루는 기능과 디렉터리를 다루는 기능으로 구성되어 있으며, 동기식 I/O와 비동기식 I/O 기능을 함께 제공한다.
동기식I/O와 비동기식I/O를 구분하기 위하여 동기식I/O 메서드 뒤에는 Sync라는 단어를 붙인다.

---

# 파일에 Read/Write
파일에 Read, Write하는 메서드는 다음과 같다.

|메소드 이름|설명|
|:---|:---|
|readFile(filename, [encoding], [callback])| 비동기식 I/O로 파일을 읽어 들입니다.|
|readFileSync(filename,[encoding])|동기식 I/O로 파일을 읽어 들입니다.|
|writeFile(filename, data, encoding='utf-8', [callback])|비동기식 I/O로 파일을 씁니다.|
|wrtieFileSync(filename, data, encoding='utf-8')|동기식I/O로 파일을 습니다.|


동기식 I/O나 비동기식 I/O는 밑의 그림 처럼 동작하고 주로 Nodejs에서는 비동기식 I/O를 사용하는 편이다.  

![그림]({{ site.url }}/images/img/language-nodejs/file1.PNG) 

# 동기식 Read

```javascript
const fs = require('fs');

let data = fs.readFileSync('./README.md', 'utf-8');

console.log(`읽어 들인 데이터는 : ${data}`);

```

동기식으로 Read를 할 경우 결과는 당연히 읽어 들인 후 console을 찍는다.
  
# 비동기식 Read

```javascript
const fs = require('fs');

fs.readFile('./README.md', 'utf-8', (err,data) => {     //콜백함수 인자로 err와 data
    if(err === null){
        console.log(err);   //에러가 발생하면 err에 오류데이터가 들어가고 그렇지 않으면 err에 null이 들어감, 이 방식으로 에러를 방지함
        console.log(`파일을 읽어들임 : ${data}`);
    }
});
console.log('프로젝트 폴더 안의 README.md파일을 읽도록 요청함');
```
비동기식으로 파일을 읽을 경우 `파라미터로 콜백함수를 전달한다.` 이 때 `콜백함수에는 err와 data를 집어 넣으며 에러가 발생하면 err에 err 데이터가 들어가고 그렇지 않을경우 null이 들어간다.` 이 이점을 이용해 에러를 방지해 파일을 읽게 할 수 있다. 물론 실행 결과는 global 스코프에 있는 console이 먼저 찍히고 읽기가 완료 되면 callback으로 인해 callback function의 console이 수행된다.  


# 동기식 write

```javascript
//비동기식 write
const fs = require('fs'); 

let wrdata = '이것은 파일에 쓰여질 데이터';

fs.writeFileSync('./NEWFILE.md',wrdata,'utf-8');

console.log('file에 제대로 데이터가 쓰여짐');
```
당연히 파일이 다 써지고 global 스코프에 있는 console이 찍힌다.  

  
# 비동기식 write

```javascript
let wrdata = '이것은 파일에 쓰여질 데이터';

//비동기식 write
fs.writeFile('./NEWFILE.md',wrdata,'utf-8',(err)=>{ 
   if(err){     //쓰기 실패 할 경우 err를 검출
       console.log(`Error : ${err}`); 
   }
    console.log('file에 제대로 데이터가 쓰여짐');
    
   
});

//비동기식 read
fs.readFile('./NEWFILE.md', 'utf-8', (err,data)=>{
    if(err){    //읽기 실패 할 경우 err를 검출
        console.log(`Error : ${err}`); 
    }
        console.log(`NEWFILE을 제대로 읽어 들임 : ${data}`);
    
});
```

위 결과도 코드를 보면 이해할 것이다.  


# 파일을 직접 열고 닫으면서 읽거나 쓰기
지금까지는 정말 쉽게 파일에 열고 썼다. 그런데 실제로 파일을 읽거나 쓸 때는 한꺼번에 모든 데이터를 읽거나 쓰지 않고, 조금씩 읽거나 쓰는 방식을 사용하는 경우도 많다. 또 다른 곳에서 받아 온 데이터를 파일에 쓰는 경우도 있기 때문에 파일을 다루는 다양한 방식이 따로 정의되어 있다. 이러기 위해 사용되는 메서드들은 다음과 같다.  


|메소드 이름|설명|
|:---|:---|
|open(path,flags,[mode],[callback])|파일을 비동기식으로 연다|
|read(fd,buffer,offset,length,position,[callback])|지정한 부분의 파일 내용을 읽어 들인다.|
|write(fd,buffer,offset,length,position,[callback])|파일의 지정한 부분에 데이터를 쓴다.|
|close(fd,[callback])|파일을 닫아 줍니다.|

여기서는 파일을 파일디스크립터 번호로 관리하며 읽고 쓴다.  


# 비동기식 open
open 메서드는 콜백함수로 실행 결과를 받기 때문에 비동기식 open이라 작성하였다. open 메서드의 2번째 파마리터는 파일을 어떻게 사용할 것인지에 대한 플래그를 정한다. 플래그는 다음과 같다.

|플래그|설명|
|:---|:---|
|'r'|읽기에 사용하는 플래그이다. 파일이 없으면 예외가 발생한다.|
|'w'|쓰기에 사용하는 플래그이다. 파일이 없으면 만들어지고 파일이 있으면 이전 내용을 모두 삭제한다.|
|'w+|읽기와 쓰기에 모두 사용하는 플래그이다. 파일이 없으면 만들어지고 파일이 있으면 이전 내용을 모두 삭제한다.|
|'a+'|읽기와 추가에 모두 사용하는 플래그이다. 파일이 없으면 만들어지고 있으면 이전 내용에 새로운 내용을 추가한다.|



간단한 예제로 살펴보자.   

```javascript
const fs = require('fs');

//파일에 데이터 쓰기
fs.open('./output.txt', 'w', (err,fd)=>{
    if(err) throw err;

    var buf = new Buffer('안녕!\n');
    fs.write(fd,buf,0,buf.length, null, function(err,written, buffer){
        if(err) throw err;

        console.log(err,written.buffer);

        fs.close(fd,() =>{
            console.log('파일 열고 데이터 쓰고 파일 닫기 완료');
        });    
    });
});
//실행 종료 순서는 읽기 -> 쓰기 -> 닫기 순으로 ,  call stack 과 task que 를 이해하면 이해됨


fs.open('./output.txt', 'a+', (err,fd) =>{

    let buf2 = new Buffer('하이\n');

    fs.write(fd,buf2,0,buf2.length, null, function(err,written, buffer){
        if(err) throw err;

        console.log(err,written.buffer);

        fs.close(fd,() =>{
            console.log('파일 열고 새로운 데이터를 추가하고 종료');
        });    
    });

});
```

![그림]({{ site.url }}/images/img/language-nodejs/file2.PNG) 

다음 시간에는 위에서 파일을 직접 열고 닫을 때 사용되는 Buffer에 대하여 알아보자.

# 참고문헌
- *Do it! Node.js 프로그래밍*

