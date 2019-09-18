---
date: '2019-09-18 22:51:11'
layout: post
title: ES Next New Features and My Thought
subtitle: 배열과 객체 그리고 맵
description: >-
  ES2016 ~ ES2019 까지 이미 잘 쓰고 있는 새로운 기능들도 있지만, 다소 활용하지 못하고 있는 기능들도 있었다. 깃허브의 트렌트
  레포에 올라온 링크를 참고해서 내 생각들 기억해야할 것들을 정리해 보려 한다. 
image: /assets/img/uploads/screen-shot-2019-09-18-at-11.11.08-pm.png
category: code
tags:
  - ㅇ
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

* 맵을 배열로 변
