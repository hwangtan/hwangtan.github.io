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
```
