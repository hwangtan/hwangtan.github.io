---
date: '2019-09-18 22:51:11'
layout: post
title: How Can I Change That? Map to Object OR Object to Map
subtitle: Map to Object OR Object to Map
description: >-
  ES2016 ~ ES2019 까지 이미 잘 쓰고 있는 새로운 문법도 있지만, 다소 활용하지 못하고 있는 문법들도 있었다. 깃허브의 트렌트
  레포에 올라온 링크를 참고해서 내 생각들 기억해야할 것들을 정리해 보려 한다. 
image: /assets/img/uploads/screen-shot-2019-09-19-at-12.48.12-am.png
category: code
tags:
  - fromEntries
  - Object.fromEntries
  - Object.entries
  - Object to Map
  - Map to Object
author: jacob
paginate: false
---
ES5 에서 ES6 로 넘어 오면서 문법적으로 정말 많은 혁신이 이루어졌다고 생각 한다. 

깃허브 트렌드를 보다가 [ECMAScript-new-features-list](https://github.com/daumann/ECMAScript-new-features-list) 가 눈에 들어 왔다. 

자바스크립트를 주 언어로 사용하고 있고, 여러 문법들을 다수 사용하고 있지만 몇가지 문법들은 사용하지 않는 것들도 있었다. 그래서 오늘은 사용해오지 않았던 문법들에 대해서 어떻게 활용하면 좋을지에 대한 기록을 남기고자 글을 작성해 본다.

## Object.fromEntries

2차원 배열을 객체로 변환시켜 준다.

```js
const array = [["A", 1], ["B", 2]];
Object.fromEntries(array); // { A: 1, B: 2 };
```

## Object.entries

객체를 2차원 배열로 변환시켜 준다.

```
const object = { A: 1, B: 2 };OBject.entries(object); // [["A", 1], ["B", 2]];
```

## Map

Map 은 key, value 가 쌍을 이루는 자료구조이다. Map 을 사용하는 것은 기존 Object 나 Array 를 다루면서 필요한 추가, 삭제, 존재유무 등을 표현할때 더 명시적인 이유도 있다고 생각 한다. 삭제를 예로 들어보겠다.

```
//delete// objectconst object = { A : 1 };delete object.A;// arrayconst array = ["A"]array.splice(array.indexOf("A"), 1);// mapconst map = new Map([["A", 1]]);map.delete("A");
```

Map 에 데이터를 설정하는 방법은 set 메소드를 이용하거나(Chaining 도 가능하다), 인스턴스 생성시에 2차원 배열을 인자로 주는 방법이 있다.

개발을 하다보면 데이터를 효율적으로 다루기 위해 배열, 객체, 맵(이터러블) 간에 변환이 필요한 경우가 꾀 빈번하게 생기게 된다. 경우를 생각해 보면 다음과 같다.

* 맵을 객체로 변환
* 객체를 맵으로 변환

두가지의 경우 앞서 소개한 문법을 사용했을때와 사용하지 않았을때 예를 들어보자.

## Map to Object

알파벳 A,B,C 를 key 로 이에 해당하는 ascii 코드를 value 로 가진 Map 이 존재 한다. 이를 객체로 변환하기 위해선 Map 에 있는 헬퍼 메소드를 이용 했을때 다음과 같이 구현해 볼수 있을것 같다.

```
const asciiOfAlphabetMap = new Map().set("A", "A".charCodeAt()).set("B", "B".charCodeAt()).set("C", "C".charCodeAt());// badconst asciiOfAlphabet = {};asciiOfAlphabetMap.forEach((value, key) => {  asciiOfAlphabet[key] = value;});// betterconst asciiOfAlphabet = Array.from(asciiOfAlphabetMap.entries()).reduce((accumulator, currentValue) => {  const [key, value] = currentValue;  accumulator[key] = value;  return accumulator;}, {});// goodconst asciiOfAlphabet = Object.fromEntries(asciiOfAlphabetMap);
```

Map 에 set 된 데이터는 이터러블 하지만, 생성자에 인자로 값을 할당 했을때 를 생각해보면 `Object.fromEntries  `를 이용해 쉽게 변환이 가능하다는 것을 알 수 있다.

## Object to Map

이번엔 반대로 객체를 맵으로 변환해 보겠다.

```
const asciiOfAlphabet = {  A: "A".charCodeAt(),  B: "B".charCodeAt(),  C: "C".charCodeAt(),}//badconst asciiOfAlphabetMap = new Map();Object.keys(asciiOfAlphabet).forEach(key => {  asciiOfAlphabetMap.set(key, asciiOfAlphabet[key]);});//betterconst twoDimensionalArray = Object.keys(asciiOfAlphabet).map(key => [key, asciiOfAlphabet[key]);const asciiOfAlphabetMap = new Map(twoDimensionalArray);//goodconst asciiOfAlphabetMap = new Map(Object.entries(asciiOfAlphabet));
```

이번에도 역시 `Object.entries` 를 이용해서 쉽게 변환 할 수 있었다.

## Conclusion

`Object.entries `와` Object.fromEntries `를 이용해서 객체를 맵으로 맵을 객체로 변환하는 문법에 대해서 알게된것이 나에게 도움이 된것 같다. 그 외에  이차원 배열을 평탄하게 해주는 flat, flatMap 이나
