---
date: '2019-03-27 08:48:05'
layout: post
title: Do you know async hell?
subtitle: ''
description: >-
 자바스크립트에 지옥이 있다면 그것은 바로 비동기 지옥일 것이다.
 오늘은 비동기 지옥을 함께 투어해 보자.
image: /assets/img/uploads/demon-161049_1280.png
category: blog
tags:
  - async
  - callback
author: jacob
paginate: false
---

# Do you know async hell? - part1

> **Better to reign in hell than to serve in heaven. - John Milton**

자바스크립트에 지옥이 있다면 그것은 바로 비동기 지옥일 것이다.

오늘은 비동기 지옥을 함께 투어해 보자.

## Callback Hell

가장 먼저 생겨난것은 콜백 지옥이다.

이 지옥이 생겨난 배경을 이해 하려면 자바스크립트 엔진에 대한 이해가 어느정도 있어야 한다.

그래서 자바스크립트 엔진에 대한 설명을 조금 해야 할 것 같다.

자바스크립트 하면 바로 떠올릴수 있는 키워드가 **'단일 스레드**'  이다.

스레드가 하나 라는것은 동시에 하나의 작업을 처리 할 수 있다는 것이다.

이러한 단일 스레드에는 **힙 메모리**와 **콜 스택** 영역이 있는데 

힙 메모리에는 변수와 객체에 대한 메모리 할당이 발생하는 영역이다. 

콜 스택은 함수 호출을 기록 하는 데이터 구조이다.
```js
    function foo(b) {
      var a = 5;
      return a * b + 10;
    }
    
    function bar(x) {
      var y = 3;
      return foo(x * y);
    }
    
    console.log(bar(6));
```
다음과 같은 코드가 있을때 콜스택은 다음과 같이 쌓인다. 그리고 후입선출에 따라 실행된다.

`main() → console.log(bar6)) → bar(6) → foo(18)`

이를 실행해 보면 실행 과정을 알수 있다.
```js
    function foo() { throw new Error('Oops!'); };
    function bar() { foo(); };
    function baz() {bar(); };
    baz();
```
콜스택의 크기는 브라우저 마다 다르지만, 콜스택의 최대 크기에 도달하면 다음과 같은 에러를 우리는 흔히 볼수 있는데 보통은 재귀 함수를 잘못 구현한 경우에 볼 수 있을 것 이다.

`RangeError: Maximum call stack size exceeded`

이것이 대부분 자바스크립트 엔진의 모습 이다. 

자바스크립트 엔진은 독립적으로 실행 되지 않고 항상 호스팅 환경(Global, Window) 에서 실행된다. 

호스팅 환경이 왜 필요한 것일까? 우리가 흔히 사용하는 DOM, setTimeout, AJAX,  노드라면 C++ 모듈등은 자바스크립트 엔진에 들어가 있지 않은것 들이다.

본질적으로 이러한 API들을 사용하여 개발을 하게 되는데 호스팅환경이 필요한 것이다. 이러한 API 들의 스레드는 접근할수없고 여기서 동시성 문제가 발생 된다.

동시성 문제를  해결하기 위해 호스팅 환경은 Event Loop 의 메커니즘을 가지게 되는데 동작 원리는 매우 간단하다.

Event Loop 는 콜백 큐와 콜 스택을 지켜보고 있다가 콜 스택이 비어 있으면 콜백 큐의 첫번째 이벤트를 콜 스택에 전달하는 방식을 가지고 있다.

- [x]  그림

이러한 반복을 이벤트 루프의 틱 (Tick) 이라 부르며 틱은 단순한 콜백 함수를 의미 한다.  

정리하자면 자바스크립트에서 비동기 작업의 동시성을 처리 하기위해 콜백 함수를 사용해야 했고 이런 메커니즘이 지옥을 만들게 되었다.

그럼 콜백 지옥을 감상해 보자.

- 이 코드는 자바스크립트 패턴과 테스트 콜백 패턴 예제를 가져왔다.
```js
    CallbackArrow = CallbackArrow || {};
    
    CallbackArrow.rootFunction = function(arg) {
      CallbackArrow.firstFunction(function(arg) {
       CallbackArrow.secondFunction(function(arg) {
        CallbackArrow.thirdFunction(function(arg) {
         CallbackArrow.fourthFunction(function(arg) {
          ....
         }); 
        });
       });
      }); 
    }
    
    CallbackArrow.firstFunction = function(callback1) {
      callback1(arg);
    };
    CallbackArrow.secondFunction = function(callback2) {
      callback2(arg);
    };
    CallbackArrow.thirdFunction = function(callback3) {
      callback3(arg);
    };
    CallbackArrow.fourthFunction = function(callback4) {
      callback4(arg);
    };
```
콜백 눌러펴기 라는 기법을 통해 해당 코드를 평탄하게 할수 있지만 더 좋은 방법이 있기때문에 여기서 설명은 생략 하도록 한다.

## Promise Hell

콜백 지옥을 완화 하기 위해 등장한 개념이 프로미스(Promise) 이다.

비동기 연산이 종료된 이후의 결과값이나 실패 이유를 처리하기 위해 사용 된다.

그러나 이또한 중첩된 지옥을 만들어내기에 충분 했다.

비동기 데이터를 요청하는 상황을 가정해 보겠다.
```js
    connectDatabase()
      .then((database) => {
        return findAllBooks(database)
          .then((books) => {
            return getCurrentUser(database)
              .then((user) => {
                return pickTopRecommendation(books, user);
              });
          });
      });
```
그리고 프로미스는 실패를 자동으로 처리 해주지 않는다. 이로 인해 개발자가 실수 할 수 있는 상황을 만들 수 있다.
```js
    const pending = (num) => new Promise((resolve, reject) => {
       if (num < 0) {
           return; //?
       }
    
       if (num > 10) {
          reject('fail');
       }
       resolve('success');
    });
    
    pending(-1).then(console.log).catch(console.log); //??
```
## Async Await Hell

Generator 는 생략하고 바로 async 에 대해서 이야기 하겠다. C# 에서 넘어온 이 방식은 콜백 지옥을 완화 시켜주었던 프로미스보다 더 진보되어서 콜백 지옥을 해결해 주는듯 했지만 이를 남용함으로 인해서 또다른 지옥을 만들어 냈다.

그럼에도 불구하고 현재 자바스크립트에서 비동기 처리를 할때 가장 선호 하는 방식이라고 생각하기 때문에 이 지옥에서 빠져 나갈수  있는 대안도 이야기 해 보겠다.  
```js
    // bad
    (async () => {
      const pizzaData = await getPizzaData()    // async call
      const drinkData = await getDrinkData()    // async call
      const chosenPizza = choosePizza()    // sync call
      const chosenDrink = chooseDrink()    // sync call
      await addPizzaToCart(chosenPizza)    // async call
      await addDrinkToCart(chosenDrink)    // async call
      orderItems()    // async call
    })()
    
    
    // good
    async function selectPizza() {
      const pizzaData = await getPizzaData()    // async call
      const chosenPizza = choosePizza()    // sync call
      await addPizzaToCart(chosenPizza)    // async call
    }
    
    async function selectDrink() {
      const drinkData = await getDrinkData()    // async call
      const chosenDrink = chooseDrink()    // sync call
      await addDrinkToCart(chosenDrink)    // async call
    }
    
    Promise.all([selectPizza(), selectDrink()]).then(orderItems)   // async call
```
장바구니를 가져와서 주문을 하는경우, 요청을 반복 루프에 포함해서 하나 하나 보낼 필요가 있을까?
```js
    // bad
    async function orderItems() {
      const items = await getCartItems()    // async call
      const noOfItems = items.length
      for(var i = 0; i < noOfItems; i++) {
        await sendRequest(items[i])    // async call
      }
    }
    
    // good
    async function orderItems() {
      const items = await getCartItems()    // async call
      const promises = items.map((item) => sendRequest(item))
      await Promise.all(promises)    // async call
    }
```
Promise.all 을 사용해 병렬적인 처리를 하는 이러한 방법에도 문제점이 있다.

만약 피자를 10개 주문했는데 6번째에서 실패를 하게 된다면 어떻게 될까?
```js
    let p1 = new Promise((resolve, reject) => {
      setTimeout(resolve, 1000, "Promise 1");
    });
    let p2 = new Promise((resolve, reject) => {
      setTimeout(resolve, 2000, "Promise 2");
    });
    let p3 = new Promise((resolve, reject) => {
      reject("Promise 3");
    });
    
    Promise.all([p1, p2, p3]).then(value => {
      console.log(value);
    }, reason => {
      console.log(reason)
    });
    // result : Promise 3.
```
Promise.all 은 실패를 빠르게 처리 하는 방식때문에 reject3 이 출력 된다. 

이러한 특성을 피해 가고자 에러를 리턴해서 에러의 인스턴스를 찾는 방식에 코드를 작성 하게 된다.
```js
    let p1 = new Promise((resolve, reject) => {
      setTimeout(resolve, 1000, "Promise 1");
    });
    let p2 = new Promise((resolve, reject) => {
      setTimeout(resolve, 2000, "Promise 2");
    });
    let p3 = new Promise((resolve, reject) => {
      resolve(new Error("Something went wrong in Promise 3!"));
    });
    
    Promise.all([p1, p2, p3]).then((values) => {
      for (let i = 0; i < values.length; ++i) {
        if (values[i] instanceof Error) {
          console.log("ERR: " + values[i].message);
        } else {
          console.log(values[i]);
        }
      }
    });
```
이 코드를 조금더 개선한 방식은 다음과 같다.
```js
    let p1 = new Promise((resolve, reject) => {
      setTimeout(resolve, 1000, "Everything OK in Promise 1");
    });
    let p2 = new Promise((resolve, reject) => {
      setTimeout(resolve, 2000, "Everything OK in Promise 2");
    });
    let p3 = new Promise((resolve, reject) => {
      reject(new Error("Something went wrong in Promise 3!"));
    });
    
    const toResultObject = (promise) => {
        return promise
        .then(result => ({ success: true, result }))
        .catch(error => ({ success: false, error }));
    };
    
    Promise.all([p1, p2, p3].map(toResultObject)).then((values) => {
      for (let i = 0; i < values.length; ++i) {
        if (!values[i].success) {
          console.log("ERR: " + values[i].error);
        } else {
          console.log(values[i].result);
        }
      }
    });
```
이렇게 Promise.all 의 코드를 확장하면 실패에 대한 허점도 개선 할 수 있게 된다.

## Conclusion

part1 에서는 콜백 지옥의 탄생과 간단한 지옥을 여행해 봤다.

아직 몇가지 풀리지 않은 떡밥이 있다. 그것은 다음시간에 다루도록 하겠다.

- Promise 와 async
- 그래서 Async / await  를 항상 사용 해야 하는가?
- 왜 Generator 를 다루지  않았는가?
- 오류 처리는?
```js
    // es2018
    async function getData() {
        const promises = [
            getPizzaData(),
            getDrinksData(),
        ];
        for await (const resolvedPromise of promises) {
            // Use the resolved promise
        }
    }
```