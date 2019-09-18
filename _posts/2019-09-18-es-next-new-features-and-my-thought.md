---
date: '2019-09-18 22:51:11'
layout: post
title: How Can I Change That? Map to Object OR Object to Map
subtitle: Map to Object OR Object to Map
description: >-
  ES2016 ~ ES2019 까지 이미 잘 쓰고 있는 새로운 문법도 있지만, 다소 활용하지 못하고 있는 문법들도 있었다. 깃허브의 트렌트
  레포에 올라온 링크를 참고해서 내 생각들 기억해야할 것들을 정리해 보려 한다. 
image: /assets/img/uploads/screen-shot-2019-09-19-at-12.48.12-am.png
category: blog
tags:
  - fromEntries
  - Object.fromEntries
  - Object.entries
  - Object to Map
  - Map to Object
author: jacob
paginate: false
---
ES5 에서 ES6 로 넘어 오면서 문법적으로 많은 변화가 이루어 졌다고 생각 한다. 

깃허브 트렌드 저장에서 [ECMAScript-new-features-list](https://github.com/daumann/ECMAScript-new-features-list) 를 보고 글을 작성해야 겠다고 생각 했다.

여러 문법들이 소개 되고 있지만 여기서 전부 소개 하는것 보단 나에게 당장 도움이 될수 있는 것을 기록으로 남기고자 한다.

## Object.fromEntries

2차원 배열을 객체로 변환시켜 준다.

```js
const array = [["A", 1], ["B", 2]];
Object.fromEntries(array); // { A: 1, B: 2 };
```

## Object.entries

객체를 2차원 배열로 변환시켜 준다.

```js
const object = { A: 1, B: 2 };
OBject.entries(object); // [["A", 1], ["B", 2]];
```

## Map

Map 은 key, value 가 쌍을 이루는 자료구조이다. Map 을 사용하는 것은 기존 Object 나 Array 를 다루면서 필요한 추가, 삭제, 존재유무 등을 표현할때 더 명시적인 이유도 있다고 생각 한다. 삭제를 예로 들어보겠다.

```js
// object
const object = { A : 1 };
delete object.A;

// array
const array = ["A"]
array.splice(array.indexOf("A"), 1);

// map
const map = new Map([["A", 1]]);map.delete("A");
```

Map 에 데이터를 설정하는 방법은 set 메소드를 이용하거나(Chaining 도 가능하다), 인스턴스 생성시에 2차원 배열을 인자로 주는 방법이 있다.

개발을 하다보면 데이터를 효율적으로 다루기 위해 배열, 객체, 맵(이터러블) 간에 변환이 필요한 경우가 꾀 빈번하게 생기게 된다. 경우를 생각해 보면 다음과 같다.

* 맵을 객체로 변환
* 객체를 맵으로 변환

두가지의 경우 앞서 소개한 문법을 사용했을때와 사용하지 않았을때 예를 들어보자.

## Map to Object

알파벳 A,B,C 를 key 로 이에 해당하는 ascii 코드를 value 로 가진 Map 이 존재 한다. 이를 객체로 변환하기 위해선 Map 에 있는 헬퍼 메소드를 이용 했을때 다음과 같이 구현해 볼수 있을것 같다.

```js
const asciiOfAlphabetMap = new Map()
.set("A", "A".charCodeAt())
.set("B", "B".charCodeAt())
.set("C", "C".charCodeAt());

// bad
const asciiOfAlphabet = {};
asciiOfAlphabetMap.forEach((value, key) => { 
 asciiOfAlphabet[key] = value;
});

// better
const asciiOfAlphabet = Array.from(asciiOfAlphabetMap.entries()).reduce((accumulator, currentValue) => {  
const [key, value] = currentValue; 
accumulator[key] = value;
return accumulator;
}, {});

// good
const asciiOfAlphabet = Object.fromEntries(asciiOfAlphabetMap);
```

Map 에 set 된 데이터는 이터러블 하지만, 생성자에 인자로 값을 할당 했을때 를 생각해보면 `Object.fromEntries`를 이용해 쉽게 변환이 가능하다는 것을 알 수 있다.

## Object to Map

이번엔 반대로 객체를 맵으로 변환해 보겠다.

```js
const asciiOfAlphabet = {  
A: "A".charCodeAt(),
B: "B".charCodeAt(),
C: "C".charCodeAt(),
}

//bad
const asciiOfAlphabetMap = new Map();
Object.keys(asciiOfAlphabet).forEach(key => {  asciiOfAlphabetMap.set(key, asciiOfAlphabet[key]);
});

//better
const twoDimensionalArray = Object.keys(asciiOfAlphabet).map(key => [key, asciiOfAlphabet[key]);
const asciiOfAlphabetMap = new Map(twoDimensionalArray);

//good
const asciiOfAlphabetMap = new Map(Object.entries(asciiOfAlphabet));
```

## Conclusion

`Object.entries`와`Object.fromEntries`를 이용해서 객체를 맵으로 맵을 객체로 쉽게 변환 할수있었다.