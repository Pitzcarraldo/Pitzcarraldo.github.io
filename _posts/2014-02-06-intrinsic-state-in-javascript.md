---
layout: post
title: "[번역] JavaScript에서의 내부 상태"
description: "[번역] JavaScript에서의 내부 상태"
modified: 2014-02-06
category: articles
tags: [Javascript, Kent Beck, 켄트 벡, 내부 상태, 외부 상태]
comments: true
share: true
redirect: https://medium.com/@pitzcarraldo/%EB%B2%88%EC%97%AD-javascript%EC%97%90%EC%84%9C%EC%9D%98-%EB%82%B4%EB%B6%80-%EC%83%81%ED%83%9C-f6fd522fa33b
---

Smalltalk Best Practice Patterns ^[1](#fn:1) 에서는 객체를 상태와 연결 짓는 두 가지 방법을 정의합니다. 내부 상태, 외부 상태가 그것입니다. 내부 상태는 필드(속성, 인스턴스 변수, 멤버 변수 등)로서 저장되고, 외부 상태는 객체가 키, 벨류로 맵에 저장됩니다.
 
객체 네이밍을 예로 들면, 객체가 ID를 가질 경우, 그것을 내부에 저장하면,

```java
class Foo
  ID id
```

외부에 저장하면 다음과 같습니다.

```java
Map<ID, Foo> foos
``` 

명확한 사례들이지만 무엇이 더 옳다고 할 순 없습니다. 프로그램의 각 부분에서 같은 객체에 다른 ID들을 사용하는 경우에는 외부 상태가 낫고, 클래스의 모든 코드에서 상태의 비트 값을 사용한다면 내부에 저장하는 것이 낫습니다.
 
### Tradeoffs
 
내부 상태는 잠재적으로 응집도를 낮추고, 결합도를 키워서 객체의 추상적인 크기가 커집니다. 지금 당장 사용해야 하는 클래스가 필드들로 가득 차 있다면 읽기가 귀찮겠죠. 그 모든 것을 알고 있을 필요는 없지만, 무시하고도 안심이 될까요? 반면에, 내부 상태는 객체와 연관되어 있다는 것을 보장합니다. decayRate가 뭘까요? 객체가 decayRate라는 필드를 가지고 있다면, 그것은 값입니다. 당신이 객체에 접근할 수 있다면, decayRate에도 접근할 수 있습니다.
 
외부 상태는 반대의 Tradeoff를 제공합니다. 외부 상태는 사용하고자 할 때 접근할 수 있고, 객체와 생존 주기를 함께하는 새로운 구조(맵)를 도입하려고 할 때, 객체의 추상적인 사이즈를 줄여줍니다. (즉, 객체가 삭제되면 맵에서도 삭제되어야 할 때.)
 
### JavaScript
 
확실하게 구분하는 것을 좋아하는 사람으로서, 작은 객체들이 연관되어 있는 경우에는, 가능하면 외부 상태를 사용하는 것을 선호합니다. 그러나 이 성향은 내가 JavaScript에서 실수를 하게 만들었습니다. 나는 JavaScript에서는 내부 상태를 사용해야 한다는 것을 몇 번이나 깨닫고, 그것을 기억하기 위해 이 노트를 작성하고 있습니다.
첫째, JavaScript는 객체를 키로 사용하는 맵을 제공하지 않습니다. 물론 외부에서 제공하는 기능들을 사용하면 가능 하겠지만, 나는 언어를 기본 그대로 사용하는 것을 좋아합니다. Map<Object, Object> 없이는 외부 상태를 구현할 수 없습니다. 둘째, JavaScript객체는 유연합니다. 생성된 지 오래된 객체에 다른 속성을 추가하더라도 JavaScript에서는 괜찮다고 여겨집니다. 적어도 나쁘진 않습니다.
 
종합하면, JavaScript의 이런 특징들이 내가 내부 상태를 선호하도록 만듭니다. 예를 들어, 어제 나는 추상 구문 트리(AST) 노드의 해시 값을 생성해서 캐시하려고 했습니다. 내 생각에, 그 해시 값은 AST의 일부가 아니었습니다. 그 값은 AST들을 다루는 다른 모든 코드에서는 사용되지 않았고, 오직 내가 작성한 프로그램에서만 쓰이고 있었습니다. 외부 상태가 내 머리를 강타하고 한 시간 후, 나는 내 모국이 아닌 곳에서 원어민 처럼 행동하고 있다는 것을 알았습니다. 난 node.hash = ... 를 집어 던져 버렸고, 모든 게 해결됐습니다.
 
### 한 줄 요약
 
자바스크립트에서는, 객체와 연결된 데이터를 그 객체의 속성으로 저장하세요.

원문 : [Kent Beck's Facebook](http://www.facebook.com/notes/kent-beck/intrinsic-state-in-javascript/709152922450908)

* * *
1. <a name="fn:1"></a>[Kent Beck의 저서, Smalltalk Best Practice Patterns](http://book.naver.com/bookdb/book_detail.nhn?bid=231103)