---
layout: post
title: "JavaScript Generators와 Co"
description: "JavaScript Generators와 Co"
modified: 2014-07-27
category: articles
tags: [Javascript, Callback Hell, Promise, Generators, Coroutine, co]
comments: true
share: true
---

# 잘 가요, TJ

얼마 전 Node.js 생태계에 아주 큰 공헌을 한 개발자 TJ Holowaychuck(이하 TJ)이 [Node.js를 떠난다는 글](https://medium.com/code-adventures/farewell-node-js-4ba9e7f3e52b)을 포스팅 해서 Node.js 진영에 큰 이슈가 되었다.

TJ가 Node.js를 떠나는 이유는 요즘 자기가 관심있는 분야를 개발하는데는 Node.js보다 Go가 더 적합하기 때문이라고 한다.
그 외 다른 이유들도 언급하고 있지만, 그에 관해서는 이미 많이 회자되었고, 이 글에서 다루고자 하는 내용과는 거리가 있기 때문에 더 자세한 내용은 다양한 [번역 글](https://www.google.co.kr/search?q=Node.js를+떠나며&oq=Node.js를+떠나며)들을 참고하길 바란다.

TJ의 글에서 내가 주목 했던 부분은 두 곳이다.

* **Koa** is the one project I’ll continue to maintain (along with **Co** and friends).

**Koa**는 내가 계속 유지보수하는 하나의 프로젝트가 될 것이다 ( **Co**와 그 친구들 역시.)

* you may get duplicate **callbacks**
* you may not get a **callback** at all (lost in limbo)
* you may get out-of-band errors
* emitters may get multiple “error” events
* missing “error” events sends everything to hell
* often unsure what requires “error” handlers
* “error” handlers are very verbose
* **callbacks** suck

Node.js에서 **callback**과 **error handling** 겁나 구려!

내 생각에도 **callback**은 겁나 구린 것 맞아...그런데 **Koa**가 뭐지? 나는 바로 **Koa**에 대해서 찾아봤다. 

# Koa.js

[Koa.js](http://koajs.com/) : next generation web framework for node.js.

홈페이지에 들어가보니 Koa.js는 [Express.js](http://expressjs.com/) Node.js에서 가장 많이 쓰이는 web framework인 Express.js의 contributor들이 새로이 만들고 있는 web framework였다. 그런데 Express.js가 있는데 왜 새로운 web framework를 새로 만들고 있지? 그리고 왜 next generation일까? 홈페이지가 아닌 [Github](https://github.com/koajs/koa)에 방문해보면 그 의미가 더 명확해진다.

Expressive middleware for node.js using **generators** http://koajs.com  

**Generators**를 사용하는 더 표현적인 미들웨어, 라고 한다. 더 자세한 설명을 보면 

Expressive middleware for node.js using generators **via co** to make web applications and APIs more enjoyable to write.

**Co**를 이용하여 웹 어플리케이션과 API를 더 즐겁게 작성할 수 있게 해줄 것이라고 한다. 아니 **Co**는 또 뭐야? 팔수록 뭐가 계속 나온다. 우선은 Koa보다 **Generators**와 **Co**에 대해서 먼저 알아봐야 할 것 같다.

# Generators

**[Generators](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function*)**는 ES6(ECMA Script 6)에 추가될 새로운 스펙이다. Python과 Ruby와 같은 다른 동적언어에는 이미 존재하는 기능으로, 간단하게 설명하면 한번 실행할 때 마다 포함된 루프가 한번만 수행되는, 내부 상태를 유지하는 함수이다. Mozilla에서 제공하는 Generators에 대한 예제를 참고하면 이해에 도움이 될 것이다.

```javascript
function* idMaker(){
    var index = 0;
    while(true)
        yield index++;
}

var gen = idMaker();

console.log(gen.next().value); // 0
console.log(gen.next().value); // 1
console.log(gen.next().value); // 2
// ...
```

next()라는 키워드를 사용하는 것이, 이미 많이 사용하고 있는 Iterator와 유사해 보이지만, 호출이 될 때마다 **yield**가 선언된 블럭만 수행되며, 내부 변수의 상태가 유지된다는 것을 알 수 있다. 또한 Generators를 사용하기 위해서 function에 *을 덧붙혀 주고 있다는 것에 주목하자.(마치 포인터가 떠오르게 한다.) Generators는 특히 Python에서 많이 사용되고 있기 때문에 Python과 Generators에 대해서 찾아보면 더 자세한 설명들을 찾을 수 있다.

* [Python wiki : Generators](https://wiki.python.org/moin/Generators)
* [Pythons과 Generators](https://www.google.co.kr/search?q=파이썬+제너레이터) 

그렇다면 **Co**는 무엇일까?

# Co

**[Co](https://github.com/visionmedia/co)**는 TJ가 만든, 방금 설명한 Generators를 이용한 flow-control 모듈이다. Co라는 이름은 아마도 **Coroutine**에서 따온 게 아닌가 싶은데, Coroutine역시 Generators와 비슷한 개념이기 때문에 함께 찾아보면 Generators에 대해 이해하는데 도움이 될 것이다. (최근엔 Unity3D에서 많이 사용되는 것으로 보인다.)

* [Coroutine이란?](https://www.google.co.kr/search?q=coroutine)

그럼 Co는 Generators를 이용하여 어떻게 flow-control을 하겠다는 것일까? 오, 맙소사! 이건 마치 Callback Hell을 위한 은탄환(Siver Bullet)일지도 모르겠다.


# Callback Hell과 Co

예전에 [Callback Hell과 Promise Pattern](/articles/2013/12/26/callback-hell-promise-pattern/)이라는 글을 포스팅한 적이 있다. 비동기 개발에서 흔히 발생하는 Callback Hell과 Promise를 이용하여 Callback Hell을 빠져나오는 방법에 대해서 간단히 설명했었는데, 당시에는 Promise가 Callback Hell을 탈출할 수 있는 가장 합리적인 해결방안이라고 생각했었다. 하지만 Promise는 금방 습득해서 사용하기엔 어느정도 러닝커브가 있는 방법이었고, Promise를 추천했던 나조차 쉽게 손이 가지 않았다. 주위에서 Promise를 사용하는 사례를 찾아보았지만, 1~2번 정도의 콜백은 그냥 중첩해서 사용하고, 그 이상의 중첩이 필요하면 설계를 수정한다고 했다. 또한 인자로 함수를 전달하는 표현 자체가 정적 언어에서 동기식으로 개발하던 사람들에게는 낯설고 불편한 방법이기 때문에, async.js나 Promise를 이용해서 Callback 함수를 아무리 예쁘게 포장해도 표현에 대한 근본적인 불편함은 해결되지 않았다. TJ가 Callback Sucks를 외치면서 Node.js를 떠난 것도 충분히 이해가 되는 부분이다. Co 역시 Callback Hell을 해결하기 위한 솔루션이라고 볼 수 있는데, 기존의 솔루션들과는 달리, **표현**에 대한 불편함까지 해소해줄 수 있는 것으로 보인다.

가장 심플한 Co의 예제 코드를 보자.

```javascript
var co = require('..');
var fs = require('fs');

function read(file) {
  return function(fn){
    fs.readFile(file, 'utf8', fn);
  }
}

co(function *(){
  var a = yield read('.gitignore');
  var b = yield read('Makefile');
  var c = yield read('package.json');
  console.log(a);
  console.log(b);
  console.log(c);
})()
```
https://github.com/visionmedia/co/blob/master/examples/simple.js

fs.readFile()은 Node.js의 내장 모듈인 fs(FileSystem)이 파일을 읽는 메소드로서, 인자로 (읽을 파일, 인코딩, Callback)을 전달 받는다. 이 예제에서는 read()라는 함수가 fs.readFile()을 감싸고, read()가 받은 인자를 fs.readFile()로 전달하도록 했다. 그리고 이렇게 감싼 read는 Genarators 안에서 yield라는 키워드를 통해 호출하도록 했고, 그 Generators는 다시 co라는 function으로 감쌌다. 과연 이 코드는 어떻게 동작할까? 일단 fs.readFile()은 대표적인 **비동기** 메소드이다. 파일이 다 읽혀지면 먼저 읽혀지는 순서에 따라 세번째로 전달받은 fn, 즉 Callback함수가 실행되어 파일의 내용을 반환하게 된다. 하지만 이 예제에서는 fs.readFile()은 read()라는 함수로 감싸졌고, read()는 Callback함수를 전달 받지 않는다. 그럼 Callback함수는 어떻게 처리되는 것일까? 답은 co에 있다. read()에 어디에서 선언되어 있지 않은 fn에 대한 처리를 co()가 하게 된다. 즉 co()내부에 있는 yield 키워드는, 다음에 나오는 함수를 수행하여 Callback이 전달받는 결과 값을 왼쪽의 변수에 할당하라는 의미가 된다. 동기식 코드로 표현하면 다음과 비슷하게 된다.

```javascript
  //read(file) > file의 내용을 읽어서 return하는 메소드
  var a = read('.gitignore');
``` 

C/Java와 같은 정적언어에서 개발하던 개발자들에게 익숙한 표현 방식과 매우 비슷하다. 기존의 Callback형식의 함수를 read()와 같이 co()에서 사용할 수 있는 방식으로 감싸줘야하는 번거로움이 있지만, 익숙한 표현을 그대로 사용할 수 있다는 점은, 그런 번거로움을 감수하고도 남을 매력이 있다고 여겨진다. 이렇게 Callback 스타일의 함수를 co에서 사용할 수 있도록 감싼 함수를 thunk라고 하고, 그렇게 감싸는 작업을 thunkify라고 하는데, TJ가 친절하게도 [thunkify를 쉽게 할 수 있는 모듈](https://github.com/visionmedia/node-thunkify) 역시 만들어 두었다.

```javascript
var thunkify = require('thunkify');
var fs = require('fs');

var read = thunkify(fs.readFile);

co(function*(){
    var data = yield read('package.json', 'utf8');
    console.log(data);
});
```

thunkify()라는 메소드로 Callback을 전달할 함수를 감싸기만 하면, yield를 이용해 값을 전달 받을 수 있는 thunk가 만들어 진다. co()와 thunkify() 둘만 있으면 Callback없이 동기식과 같은 표현방법으로 결과값을 받을 수 있는 것이다! 정말 멋지지 않은가? 표현적인 부분만 봤을 땐, Callback Hell의 완벽한 해결책이라고 생각한다. Co를 이용하면 Callback Hell을 만들어 내는 주범인, Callback result를 이용하여 다시 Callback을 호출하는 문제를 아주 깔끔하게 해결할 수 있다. 다시 Co의 첫번째 예제 코드를 보자. yield가 연달아 3번 선언되어 있다. 이 코드들은 어떻게 동작하게 될까? 앞에서 설명했듯이 Generators 함수는 한번 수행시 yield가 선언된 곳까지만 실행된다. 그렇다면 yield가 한 함수 안에 여러번 선언되어 있다면? 호출될 때 마다 다음 yield를 수행하게 된다. Generators의 예제에서는 yield가 반복문 안에 선언되어 있었기 때문에, 첫번째 yield 다음에 나오는 yield는 반복문이 다음에 실행하는 yield가 되어 계속해서 다음 값을 반환하게 되는 것이다. Co에서도 그렇다. 첫번째 yield가 thunk를 실행하면 그 thunk는 본래 비동기 함수이기 때문에 비동기로 수행된다. 그리고 내부의 Callback이 결과 값을 반환하면 그 값을 변수에 할당하고 다음 yield까지 진행하게 된다. 다음 yield가 실행되는 시점은 이전 yield의 작업이 종료된 이후이기 때문에 이전 비동기 함수의 결과를 알 수 있고, 그 값을 이용해 새로운 비동기 함수를 수행할 수 있다. 그래서 예제코드의 console.log는 a,b,c 순차적으로 출력하게 된다. 이것은 마치 정적언어가 동기식으로 동작하는 것과 비슷하다. 하지만 그 동작은 비동기로 수행된다. 한마디로 Co는 비동기 코드를 동기식으로 작성하기 위한 모듈인 것이다.

# Co의 다른 기능들

Co는 이런 특성을 다른 방식으로도 사용할 수 있다.

### 병렬 호출

yield로 thunk의 배열을 호출하면 여러 thunk를 동시에 실행할 수 있다.

```javascript
co(function *(){
  var a = get('http://google.com');
  var b = get('http://yahoo.com');
  var c = get('http://cloudup.com');
  var res = yield [a, b, c];
  console.log(res);
})()
```

수행의 결과값은 모든 thunk의 실행이 끝나면 배열로 반환된다.

### Error Handling

일반적인 Node.js 모듈들의 Callback은 다음 형식을 지킨다.

```javascript
fs.readFile(file, encoding, callback) {
    ...
    callback(error, data);
 }
```

callback의 첫번째 인자는 error이다. 그래서 일반적인 Node.js에서 Error Handling은 다음과 같은 형태이다.

```javascript
function callback(error, data) {
    if(error) {
        console.err(error);
        throw error;
    }
    //Normal code
    ...
}
```

하지만 Co는 결과값만을 전달 받는다. 즉 일반적인 Callback함수의 두번째 인자만 변수에 전달한다. 그렇다면 첫번째 인자로 Error가 전달 되었을 땐 어떻게 될까? 무조건 Error를 throw하게 된다. 기존의 Node.js 모듈들에서는 Error가 전달되어도 무시할 수 있도록 선택권을 개발자에게 주었지만 Co는 그렇지 않다. Error가 전달되면 무조건 throw를 발생시켜 개발자가 적절하게 Error를 처리하여야 한다. 하지만 이것은 Java의 Exception Handling과 매우 유사하기 때문에, 오히려 정적 언어 개발자들에겐 익숙한 방법이라고 볼 수 있다.

```javascript
co(function*(){
    try {
        var data = yield read('package.json', 'utf8');
        console.log(data);
    } catch (Error e) {
        console.error(e);
    }
});
``` 

### Co 내부에서의 return

co()내부에서 연산한 값을 return하고 싶을 땐 어떻게 해야할까? co()와 Generators로 감싸져 있는데 외부로 값이 전달 가능 할까? 물론 가능하다. co() 내부에서 값을 return하게 되면 하면 일반적인 형태의 Callback을 인자로 받는 함수를 반환한다.

```javascript
var size = co(function *(){
  var a = yield read('.gitignore');
  return a.length;
});

size(function(err, res){
  console.log(res);
});
```

이것은 하위호환을 위한 것으로, co()를 이용한 함수가 return하는 값이 Co를 사용하지 않는 모듈에서 사용할 경우를 고려하여 일반적인 형태의 Callback을 반환한다. 하지만 이 리턴 값을 thunkify 시키면 동기적인 표현을 이어서 사용할 수 있다.

```javascript
var getLength = co(function *(){
  var a = yield read('.gitignore');
  return a.length;
});

var getSize = thunkify(getLength);

var size = yield getSize();
console.log(size);
```

이 예제에서는 이렇게 사용하는 이유를 이해하기 힘들지도 모르겠지만, co를 사용하는 메소드들이 개별적으로 모듈화가 되어있다면, 매우 유용하게 사용할 수 있는 패턴이다. 

### 여러 값을 반환하는 Callback

앞에서 설명했듯이, 일반적인 Callback의 첫번째 인자는 error, 두번째 인자는 결과값이다. 그렇다면 결과로 여러 값을 전달하는 Callback의 경우는 어떻게 될까?

```javascript
fs.readFile(file, encoding, callback) {
    ...
    //원래는 callback(error, data);
    callback(error, data, filesize);
}

var read = thunkify(fs.readFile);
co(function *(){
  var a = yield read('.gitignore');
  console.log(a);
})()
```

기존의 fs.readFile의 Callback은 file의 내용인 data만 반환하지만, callback을 수정하여 data와 filesize까지 반환하도록 하였다. 이렇게 Callback이 하나 이상의 값을 반환 할 경우에는 결과값이 배열로 전달된다. 즉 read의 결과값이 저장되는 a는 배열이 되어, a[0] = data, a[1] = filesize가 되는 것이다.

그 밖에 Co에 대한 더 많은 설명과 예제는 [Co의 Github Repository](https://github.com/visionmedia/co)를 참고하길 바란다. 

# 다시 Koa.js

그렇다면 다시 서두에 이야기 했던 Koa.js에 대한 이야기로 돌아와보자.

Expressive middleware for node.js using generators **via co** to make web applications and APIs more enjoyable to write.

Koa.js는 **Co**를 이용한 표현적인 미들웨어라고 한다. 그렇다. Koa.js는 Co를 이용하여 Callback으로 인해 복잡했던 Node.js 웹 개발을 더 단순하고 직관적으로, 즉 표현적으로 만들기 위한 미들웨어 인 것이다. 지금까지 살펴본 Co의 특징들이라면 충분히 가능할 것 같다. 그리고 TJ가 Node.js를 떠나면서도 왜 Koa.js와 Co의 유지보수는 이어서 하겠다고 한건지도 충분히 납득이 된다. 그가 외쳤던 **Callback Sucks**를 해결할 수 있는 방법이기 때문이다! 굳이 Koa.js까지 사용하지 않고, 기존 Node.js 모듈들에 Co만 적용하더라도 가독성과 생산성을 크게 높일 수 있을 것으로 보인다. 그리고 이미 많은 모듈들이 Co로 Wrapping 되어 제공되고 있다.

* [Co Libraries](https://github.com/visionmedia/co/wiki)   

# Co의 한계

그렇다면 Co는 은탄환인가? Generators를 쓸 수 있는 환경이라면 그렇게 보인다. 하지만 Generatros를 설명할 때 언급했던 것 처럼 Generators는 ES6에 도입 **될** 스펙이다. 아직 일반적인 환경에서는 Generators는 쓸 수 없다는 이야기다. Front End 환경에서는 물론이겠거니와, Node.js에서도 unstable 버전인 0.11에서 --harmony-generators 라는 옵션을 줘야만 사용할 수 있다. 아니면 [gnode](https://github.com/TooTallNate/gnode)와 같은 별도 모듈을 함께 설치해서 사용하거나, [regenerator](http://facebook.github.io/regenerator/)와 같은 도구를 이용해 Generators를 사용한 코드를 기존 환경에서 돌아갈 수 있도록 컨버팅을 해주어야 한다. Front End에서는 Grunt나 browserify를 사용하지 않는다면 거의 사용을 포기해야한다. 하지만 Node.js의 경우에는 gnode만 설치해준다면 stable버전인 0.10.x 대에서도 문제 없이 사용할 수 있으니 그나마 나은 편이다. 즉 지금은 서버환경에서만 사용할 수 있다는 한계가 있다. 그럼 또 다른 문제는 없는가? 기존 Callback 함수를 감싸서 사용 해아하는데, 성능적인 문제는 없는지? Co의 개발자 TJ는 이렇게 설명하고 있다.

On my machine 30,000 sequential stat()s takes an avg of 570ms, while the same number of sequential stat()s with co() takes 610ms, aka the overhead introduced by generators is extremely negligible.

기존의 함수와 co로 감싼 함수의 수행 시간의 차이는 ms 단위로 무시할 수 있는 수준이라고 한다. 그리고 이미 Co뿐만 아니라 기존의 Callback 처리 함수들에 대한 벤치마킹을 수행한 포스팅이 있으니 함께 참고하면 좋을 듯 하다. (성능 뿐만 아니라 다양한 장단점에 대해서 상세히 분석해 놓은 매우 좋은 포스팅이다.)

* [Analysis of generators and other async patterns in node](http://spion.github.io/posts/analysis-generators-and-other-async-patterns-node.html)

# Co-ooool!

Co는 정말 Cool한 모듈이다. 적어도 나의 짧은 지식 내에서는 Node.js에서 Callback Hell을 해결할 수 있는 방법들 중엔 가장 Cool하다고 할 수 있다. 기존에 Callback 형식으로 작성했었던 코드들을 Co를 이용하여 리팩토링 한 결과 훨씬 직관적이고 간결해졌으며, 그로 인해 흐름이 한눈에 들어오게 되었다. 개인적으로 마음에 들지 않던 callback(null, result), if(error) throw error; 과 같은 표현들을 보지 않아도 되는 것 역시 마음에 드는 부분이다. thunkify를 이용해 thunk로 감싸줘야하는 작업이 조금 번거롭긴 하지만, 그정도 번거로움은 Co를 통해서 얻을 수 있는 장점으로 모두 상쇄시키고도 남는다고 생각한다. 게다가 promise도 함께 지원하기 때문에, promise를 return하는 함수의 경우 바로 yield로 호출 하여 결과를 받을 수 있다. 최근엔 자체적으로 promise api를 지원하는 모듈들도 많고, 유명한 모듈들의 경우 이미 thunk 형식으로 제공되고 있는 것들도 많아서 적용에 큰 어려움이 없을 것이다. (ex: [co-mocha](https://github.com/ilkkao/co-mocha))

한동안 Callback을 이용한 비동기 개발의 한계 때문에 Node.js를 이용한 개발에 소홀해 있었는데, 많은 사람들이 Node.js에 대해 의문을 가지게 했던 TJ의 포스팅이 나에겐 오히려 Node.js로의 새로운 길을 제시해 준 통로가 되었다. 나뿐만 아니라 더 많은 사람들이 Co에 대해 알고, Callback 때문에 괴로워하지 않았으면 하는 바람으로 모자란 지식으로 Co에 대해서 정리해 보았다.

사용해 본 경험 안에서 기술 할 수 있는 것들은 최대한 기술하고자 했지만, 그래도 부족한 부분이 많을 것이라고 생각한다.
Generators, Co, Koa.js 등에 대해 더 자세히 알고 싶은 분들은 아래의 링크들을 참고하면 더 풍부한 내용들을 얻을 수 있을 것이다.   

##### 참고 Site :
* [Mozilla Developer Network : Generators](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function*)
* [Koa.js Github](https://github.com/koajs/koa)
* [Co Github](https://github.com/visionmedia/co)
* [Why coroutines won’t work on the web](http://calculist.org/blog/2011/12/14/why-coroutines-wont-work-on-the-web/)
* [Analysis of generators and other async patterns in node](http://spion.github.io/posts/analysis-generators-and-other-async-patterns-node.html)
