---
layout: post
title: "Callback Hell 과 Promise Pattern"
description: "Callback Hell 과 Promise Pattern"
modified: 2013-12-26
category: articles
tags: [Javascript, Callback Hell, Promise]
comments: true
share: true
---

Node.js 등을 통해서 Javascript 개발을 하다보면, 비동기 처리시 사용되는 Callback 패턴으로 인해 중첩 코드가 늘어나 코드의 복잡성이 능가하고 가독성이 떨어지는 경우가 종종 생긴다. 이르자면 다음과 같은 경우이다.

```javascript
fs.readdir(source, function(err, files) { 
  if (err) { 
    console.log('Error finding files: ' + err) 
  } else { 
    files.forEach(function(filename, fileIndex) { 
      console.log(filename) 
      gm(source + filename).size(function(err, values) { 
        if (err) { 
          console.log('Error identifying file size: ' + err)
        } else { 
          console.log(filename + ' : ' + values) 
          aspect = (values.width / values.height) 
          widths.forEach(function(width, widthIndex) { 
            height = Math.round(width / aspect) 
            console.log('resizing ' + filename + 'to ' + height + 'x' + height)
            this.resize(width, height).write(destination + 'w' + width + '_' + filename, function(err) { 
              if (err) console.log('Error writing file: ' + err)
            }) 
          }.bind(this)) 
        } 
      }) 
    }) 
  } 
})
```
[코드 출처 : [http://callbackhell.com](http://callbackhell.com)]
 
흔히 Callback Hell이라고 부르는 현상인데, Callback 함수가 다시 그 결과 값을 가지고 Callback을 부르고, 또다시 Callback을 부르면서 처리하게 되면 발생한다. Callback 함수의 경우 인라인으로 처리하는 경우가 많아서, 이렇게 Callback이 다시 Callback을 호출하게 되면, 여러 함수가 중첩 되면서 코드를 눈으로 따라가기 어려운 상황에 처하게 되고, 유지보수가 어려운 코드가 양산되게 된다. 말 그대로 정말 HELL 이다.
 
위 예제 코드를 가지고 온 http://callbackhell.com/ 사이트를 방문해 보면 이런 Callback Hell 현상을 해결할 수 있는 여러가지 방안들을 제시하고 있는데, 그 방법들은 다음과 같다.

* 인라인 함수에 이름을 붙여라.
* 코드를 간결하게 작성하라.(라인수를 줄여라.)
* 모듈화 하라.
* Promise 패턴을 고려하라.

해결 방안 중 앞의 3개는 Callback Hell이 발생 하더라도 코드를 좀 더 명시적이고 간결하게 작성하여 가독성을 개선하는 방법이기 때문에 근본적인 해결 방법이 되지 못한다. 하지만 마지막에 이야기하고 있는 Promise 패턴을 주목해보자. Promise 패턴이 도대체 무엇일까? Callback Hell을 해결 할 수 있는 근본적인 해결 방법일까?

[http://callbackhell.com](http://callbackhell.com) 에서는 Promise 패턴을 다음과 같이 설명하고 있다.

> **Promises** are a more abstract pattern of working with async code in JavaScript.

> **"Javascript 에서 비동기 코드를 동작시키는 더 추상적인 패턴."**

Pormise 패턴은 Callback 패턴과 같이 비동기 처리를 하기 위한 패턴이지만, 다른 형태를 가지고 있는 패턴이다. 우리는 이 Promise 패턴을 통해서 Callback 패턴 사용시 생길 수 있는 Callback Hell 현상을 피할 수 있게 된다. 그럼 Promise 패턴은 어떻게 사용하며, 어떻게 Callback Hell을 해결 할 수 있는지 알아보자.

Promise 패턴의 스펙은 다음 사이트에서 정의하고 있다.

[http://promises-aplus.github.io/promises-spec](http://promises-aplus.github.io/promises-spec)

Promise 패턴은 스펙일 뿐이며 실제 사용을 위해서는 그것을 Javascript로 구현한 구현체가 있어야하는데, 가장 많이 사용되고 있는 Promise 패턴의 구현체로는 [q.js](https://github.com/kriskowal/q)  와 [when.js](https://github.com/cujojs/when) 가 있다. 가장 널리 사용되고 있는 것은 q.js 이고, 많은 기능을 제공 하지만 무겁다. 반면 when.js의 경우 경량화 된 Promise 구현체로서 q.js보다 가볍기 때문에, FrontEnd, BackEnd 양쪽에서 부담없이 사용할 수 있다. 이 포스팅에서는 when.js를 기반으로 Promise 패턴을 사용하는 방법에 대해 설명하기로 한다.

Promise에 대해 알게 된 건, Mongoose를 이용해 Node.js - MongoDB 연동 기능을 개발하던 중, Callback Hell로 인해 점점 복잡해져가는 코드를 보며, Javascript에서 Callback 처리를 효율적으로 할 수 있는 방법들을 찾아보던 중이었는데, 수 많은 Node.js 개발자들이 Callback Hell로 개발에 어려움을 가지고 있었고, 여러가지 해결 방법들이 존재하고 있었다. 그 중 하나는 [async.js](https://github.com/caolan/async) 이라는 Step Library 활용하는 방법이었는데, 기존 Callback 구조를 가지고 있는 여러 함수들을 사용할 수 있는 장점이 있지만, 인라인 함수 사용으로 인해 생기는 중첩 문제를 근본적으로 해결하는 방법은 아니었고, 분기 처리에 어려움이 있어서 적용을 시도하다가 포기했다. 그 다음으로 시도 해본 것이 Promise 패턴이었고, 기존 Callback 형식 함수를 사용하려면 Promise 패턴으로 Wrapping 을 해서 사용해야하는 번거로움이 있지만, 제대로 적용만 된다면 Callback Hell이 생기지 않는 간결한 코드가 작성이 가능해지기 때문에 async.js 사용시 보다 더 유지보수성이 좋을 것이라고 생각하고 적용을 해보았다.

Promise 패턴은 Promise Object를 기반으로 동작한다. when.js를 사용하면 Promise Object를 다음과 같이 생성할 수 있다.

```javascript
var promise = when.promise(function(resolve, reject, notify) { 
    resolve(onSuccess); 
    reject(onError); 
}); 
var deferred = when.defer(); 
deferred.resolve(onSuccess);  
deferred.reject(onError); 
var promise = deferred.promise;
```

when.promise(), when.defer() 두가지 함수를 통해 각각 다른 방법으로 Promise Object를 생성할 수 있다. Promise Object에 비동기 처리가 성공했을 경우 반환할 값을 resolve에 넘기고, 실패했을 때 반환할 값을 reject에 넘기면 된다. 개인적으로는 인라인 함수를 통해서 resolve, reject를 지정해줘야 하는 when.promise() 보다 흐름에 따라 resolve와 reject를 다른 시기에 지정할 수 있는 when.defer()를 사용하는 것이 더 좋은 방법으로 여겨진다.

추가적으로 비동기 처리가 진행 되는 동안 수행 상황을 알려 줄 수 있는 nofify 함수 역시 지정할 수 있는데, resolve를 제외한 나머지 함수들은 선택 사항이기 때문에 지정하지 않아도 사용에는 문제가 없다. (하지만 reject는 Error Handling를 위해 지정해주는 것이 좋다.)

이렇게 Promise Object를 생성했으면 then()이란 함수로 Callback 처리를 할 수 있다.

```javascript
promise.then(funcOnSuccess, funcOnError);
```

funcOnSuccess에 비동기 처리가 완료되었을 때 수행할 함수(=Promise Object 생성시 지정해준 resolve), funcOnError는 수행 중 에러가 발생했을 때 수행할 함수(=Promise Object 생성시 지정해준 reject)를 넘겨주면 된다. then()은 Promise Object를 반환하기 때문에 Chaining을 통해 앞서 수행한 Promise의 결과를 받아 작업을 순차적으로 처리할 수 있도록 then()을 추가로 연결할 수 있다.

```javascript
promise.then(funcOnSuccess, funcOnError).then(funcOnSuccess2, funcOnError2);
```

여기서 funcOnSuccess2는 funcOnSuccess의 return 값을 전달 받는 함수가 들어가게 된다.
이렇게 then을 통해서 비동기 처리가 필요한 함수들을 Callback 처럼 중첩되지 않고, 선형적으로 표기가 가능하기 때문에 코드에서 작업의 흐름을 파악하기 쉽고, 작업 수행 결과에 따라 resolve, reject로 적절히 분기도 가능하게 된다.

실제로 작업하던 코드에 when.js 를 적용하여 Callback 구조의 함수를 Promise로 개선해 보았다.

##### AS-IS

```javascript
getFacebookAccount: function (profile, accessToken, callback) {
        var self = this; 
        if(typeof profile.username === 'undefined') {
            throw Error('username is empty');
        } 
        repository.findByFacebookId(profile.id, function (account) { 
            if (account) { 
                callback(account); 
            } else { 
                repository.findById(profile.name, function (account) { 
                    if (account) { 
                        if (account.id === profile.providers.facebook.id) {
                            repository.updateById(profile.name, self.convertToFb(profile, accessToken),
                            function (result) {
                                callback(account); 
                            }); 
                        } else { 
                            throw Error('already exist same id');
                        } 
                    } 
                }); 
            } 
        }) 
    }
```

##### TO-BE

```javascript
getFacebookAccount: function (profile, accessToken) { 
        var self = this; 
        var deferred = when.defer(); 

        if (typeof profile.username === 'undefined') {
            deferred.reject(new Error('username is empty'));
            return deferred.promise; 
        } 

        repository.findByFacebookId(profile.id) 
            .then(function (account) { 
                if (account) { 
                    deferred.resolve(account); 
                } else { 
                    return repository.findById(profile.username); 
                } 
            }) 
            .then(function (account) { 
                if (account.providers.facebook.id === profile.id) { 
                    return repository.updateById(profile.name, self.convertToFb(profile, accessToken));
                } else { 
                    deferred.reject(new Error('already exist same id'));
                } 
            }) 
            .then(function (resultCount) { 
                deferred.resolve(resultCount); 
            }); 

        return deferred.promise; 
    }
```

두 코드를 비교해보면 Callback 패턴을 이용하여 작성한 쪽은 분기에 따라 중첩의 깊이가 3단계까지 들어가게 되고, 코드의 흐름을 한눈에 파악하기가 어렵다. 하지만 Promise.then()을 이용하여 구현한 쪽은 작업의 흐름에 따라 then() 을 순차적으로 따라가기만 하면, if/else 로만 구현된 간단한 resolve 함수들만이 존재하기 때문에 코드를 이해하는데 큰 어려움이 없을 것이다. 그리고 return으로 Promise Object 반환했기 때문에 getFacebookAccount 함수 역시 then()으로 결과 값을 처리할  있다.또한 when.all() 에 Promise Object들을 배열로 넘기게 되면 여러 비동기 처리를 병렬로 수행이 가능하다.

```javascript
var when = require('when');
var someWork = function(callback) {
 when.all([collection1.find(query1).exec(),
  collection2.find(query2).exec() ])
  .spread(callback)
  .otherwise(function(err) {// something went wrong});
  };
```
 
이 밖에도 when.js에서는 Promise 스펙에 정의된 다양한 기능들을 제공하고 있지만, 가장 핵심은 then()을 통한 Callback 처리이다. Promise 패턴이 앞에서 언급했던 async.js 를 이용할 때 보다 더 뛰어난 점은 Error Handling이다. async.js 역시 순차적 또는 병렬로 Callback 형식의 함수들을 수행할 수는 있지만, 결과에 따라 내부에서 분기가 생길 경우에 대한 처리가 어렵다. 하지만 then()을 사용할 경우 resolve(), reject() 두가지 상황에 따른 구현이 가능하기 때문에 여러가지 상황에도 대응하기가 훨씬 용이하다.
 
지금까지 간단히 Javascript의 Promise 패턴과, 그 구현체인 when.js 에 대해 알아보았다. 아직도 수많은 Javascript 코드들이 인라인 함수를 이용한 Callback 패턴을 사용하고 있는데, Javascript의 강점이라고 할 수 있는 인라인 함수 처리가 Javascript 코드들을 더 복잡하게 만들었고, 그것이 오히려 가독성에 독이 되어 발목을 붙잡고 있는 건 아닐까 생각한다. V8과 Node.js 의 출현으로 Javascript는 지금 탄생 이래 최고의 중흥기를 맞이하고 있고, 앞으로도 더 발전할 것이라고 기대가 된다. 하지만 Javascript로 뛰어든 많은 개발자들이 Callbak Hell에 빠져 도중에 좌절하고 포기하게 되진 않을까 걱정이 되기도 한다. Promise 패턴은 개발자들이 Callback Hell에서 빠져나올 수 있는 적절한 대안이라고 생각한다. 이미 많은 Javascript 및 Node.js Module 들이 이미 Promise 패턴을 통해 API를 제공하고 있다. jQuery는 [when()](http://api.jquery.com/jquery.when)이란 API를 통해서 Promise 패턴을 제공하고 있고, Mongoose 역시 자체적으로 Promise 패턴 사용을 위한 [Mongoose Promise API](http://mongoosejs.com/docs/api.html#index_Mongoose-Promise)를 제공 중이다. 이렇게 제공되는 API들과 직접 구현한 Promise 패턴들을 적절히 활용한다면, 앞으로는 쉽게 Callback Hell을 피할 수 있을 것이다.
 
개발자들이여, Callback을 두려워 하지 말자.
Promise가 더 나은 코드를 약속할 것이다.
 
 
*** 
**여담**
Promise 패턴은 은탄환이 아니다. 그래도 수 많은 Javascript Library들이 Callback 패턴을 통해 API를 제공하고 있고, Callback 구조를 완벽히 버리기는 힘들다. 하지만 Callback과 Promise를 적절히 활용한다면, Callback만 사용하는 코드보다는 훨씬 깔끔한 코드를 작성할 수 있을 것이다.
 
##### 참고 Site :

* [http://callbackhell.com](http://callbackhell.com)
* [http://promises-aplus.github.io/promises-spec](http://promises-aplus.github.io/promises-spec)
* [https://github.com/cujojs/when/blob/master/docs/api.md#api](https://github.com/cujojs/when/blob/master/docs/api.md#api)
* [http://stackoverflow.com/questions/17564974/to-to-do-a-parallel-query-in-mongoose/17565327#17565327](http://stackoverflow.com/questions/17564974/to-to-do-a-parallel-query-in-mongoose/17565327#17565327)
* [http://net.tutsplus.com/tutorials/javascript-ajax/managing-the-asynchronous-nature-of-node-js](http://net.tutsplus.com/tutorials/javascript-ajax/managing-the-asynchronous-nature-of-node-js)