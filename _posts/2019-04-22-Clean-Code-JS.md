---
date: '2019-03-27 08:48:05'
layout: post
title: Clean Code JS Version
subtitle: ''
description: >-
 즐겨 찾기 해놨던 클린코드 저장소에 있는 글을 이해 하면서 공부를 위해 작성해본 아티클 입니다.
image: /assets/img/uploads/800x0.jpg
category: blog
tags:
  - welcome
  - blog
author: jacob
paginate: false
---


# Clean Code

[https://github.com/ryanmcdermott/clean-code-javascript](https://github.com/ryanmcdermott/clean-code-javascript)

로버트 C.마틴의 클린 코드를 자바스크립트 버전으로 설명 합니다.

## Variables

- 의미 있는 변수 이름을 사용 하라. (Good)
```js
    //bad
    const yyyymmdstr = moment().format("YYYY/MM/DD");
    //good
    const currentDate = moment().format("YYYY/MM/DD");
```
- 검색 가능한 이름을 사용 해라. (Good)
```js
    //bad
    setTimeout(func, 86400000);
    //good
    const MILLISECONDS_IN_A_DAY = 86400000;
    setTimeout(func, MILLISECONDS_IN_A_DAY);
```
- 배열에 대한 결과 값을 명시적으로 선언 해라. (Good)
```js
    const address = "One Infinite Loop, Cupertino 95014";
    const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
    //bad
    saveCityZipCode(
      address.match(cityZipCodeRegex)[1],
      address.match(cityZipCodeRegex)[2]
    );
    //good
    const [, city, zipCode] = address.match(cityZipCodeRegex) || [];
    saveCityZipCode(city, zipCode);
```
- 암시적인 것보다 암묵적인것이 좋다. (Good)
```js
    //bad
    const locations = ["Austin", "New York", "San Francisco"];
    locations.forEach(l => {
      doStuff();
      doSomeOtherStuff();
      dispatch(l);
    });
    //good
    locations.forEach(location => {
      doStuff();
      doSomeOtherStuff();
      dispatch(location);
    });
```
- 불필요한 컨텍스트를 추가 하지 말자 (Normal.)

    클래스나 객체명을 변수 이름에 반복하지 말자.
```js
    // bad
    const Car = {
      carMake: "Honda",
      carModel: "Accord",
      carColor: "Blue"
    };
    
    function paintCar(car) {
      car.carColor = "Red";
    }
    // good
    const Car = {
      make: "Honda",
      model: "Accord",
      color: "Blue"
    };
    
    function paintCar(car) {
      car.color = "Red";
    }
```
- 단락 조건 보단 기본 인수 사용 (Good) 이것은 한번더 확인.

   기본값을 표시하는게 코드가 깔끔해짐, 설명상으론 falsy 한 값을 모두 대응 못한다고 하는데    크롬에선 다 잘되는것을 확인 했습니다.("", null,  undefined, NaN, 0, false);
```js
    // bad
    function createMicrobrewery(name) {
      const breweryName = name || "Hipster Brew Co.";
    }
    // good
    function createMicrobrewery(name = "Hipster Brew Co.") {
    }
```
## Functions

- 매개 변수의 양을 최소화 해라( 2개이하) (잘 안된다고 생각함)

넘어갈 경우엔 객체를 활용하고, ES6 Destructuring 를 적극 활용 하라.

Destructuring은 함수에 전달 된 인수 객체의 지정된 원시값을 복제 하기 때문에 부수효과를 예방하는데 매우 효과 적이다.
```js
    //bad
    function  createMenu ( title , body , buttonText , cancellable ) { 
    }
    
    //good
    function createMenu({ title, body, buttonText, cancellable }) {
    }
    
    createMenu({
      title: "Foo",
      body: "Bar",
      buttonText: "Baz",
      cancellable: true
    });
```
- 함수는 한가지 작업을 수행해야 한다.(노력하는부분)

단일책임의 원칙과 같은 내용
```js
    // bad
    function emailClients(clients) {
      clients.forEach(client => {
        const clientRecord = database.lookup(client);
        if (clientRecord.isActive()) {
          email(client);
        }
      });
    }
    
    // good
    function emailActiveClients(clients) {
      clients.filter(isActiveClient).forEach(email);
    }
    
    function isActiveClient(client) {
      const clientRecord = database.lookup(client);
      return clientRecord.isActive();
    }
```
- 한 함수에서 한단계의 추상화만 하자(Good)
```js
    //bad
    function parseBetterJSAlternative(code) {
      const REGEXES = [
        // ...
      ];
    
      const statements = code.split(" ");
      const tokens = [];
      REGEXES.forEach(REGEX => {
        statements.forEach(statement => {
          // ...
        });
      });
    
      const ast = [];
      tokens.forEach(token => {
        // lex...
      });
    
      ast.forEach(node => {
        // parse...
      });
    }
    
    //good
    function parseBetterJSAlternative(code) {
      const tokens = tokenize(code);
      const syntaxTree = parse(tokens);
      syntaxTree.forEach(node => {
        // parse...
      });
    }
    
    function tokenize(code) {
      const REGEXES = [
        // ...
      ];
    
      const statements = code.split(" ");
      const tokens = [];
      REGEXES.forEach(REGEX => {
        statements.forEach(statement => {
          tokens.push(/* ... */);
        });
      });
    
      return tokens;
    }
    
    function parse(tokens) {
      const syntaxTree = [];
      tokens.forEach(token => {
        syntaxTree.push(/* ... */);
      });
    
      return syntaxTree;
    }
```
- 함수가 외부 값을 참조하는 것을 지양 하자.
```js
    //bad
    let name = "Ryan McDermott";
    
    function splitIntoFirstAndLastName() {
      name = name.split(" ");
    }
    
    splitIntoFirstAndLastName();
    
    console.log(name); // ['Ryan', 'McDermott'];
    
    //good
    function splitIntoFirstAndLastName(name) {
      return name.split(" ");
    }
    
    const name = "Ryan McDermott";
    const newName = splitIntoFirstAndLastName(name);
    
    console.log(name); // 'Ryan McDermott';
    console.log(newName); // ['Ryan', 'McDermott'];
```
- 참조형태는 불변형을 만들어 새로운 값을 반환 한다.
```js
    //bad
    const addItemToCart = (cart, item) => {
      cart.push({ item, date: Date.now() });
    };
    
    //good
    const addItemToCart = (cart, item) => {
      return [...cart, { item, date: Date.now() }];
    };
```
- 함수의 이름에서 그 함수의 역할을 알수 있어야 한다.(노력하는부분)
```js
    //bad
    function addToDate(date, month) {
    }
    
    const date = new Date();
    
    // It's hard to tell from the function name what is added
    addToDate(date, 1);
    
    //good
    function addMonthToDate(month, date) {
      // ...
    }
    
    const date = new Date();
    addMonthToDate(1, date);
```
- 객체의 디폴트 값을 설정할때는 Object.assign 을 사용해 보자.(OH!)
```js
    //bad
    const menuConfig = {
      title: null,
      body: "Bar",
      buttonText: null,
      cancellable: true
    };
    
    function createMenu(config) {
      config.title = config.title || "Foo";
      config.body = config.body || "Bar";
      config.buttonText = config.buttonText || "Baz";
      config.cancellable =
        config.cancellable !== undefined ? config.cancellable : true;
    }
    
    createMenu(menuConfig);
    
    //good
    const menuConfig = {
      title: "Order",
      // User did not include 'body' key
      buttonText: "Send",
      cancellable: true
    };
    
    function createMenu(config) {
      return Object.assign(
        {
          title: "Foo",
          body: "Bar",
          buttonText: "Baz",
          cancellable: true
        },
        config
      );
    }
    
    createMenu(menuConfig);
```
- 함수 매개 변수로 플래그를 사용하지 말자 (Humm)!!! **우리 회사 코드에서 조금 더 사례를 찾아보자!**

함수는 한 가지 일만 해야 한다. 그런 관점에서 플래그는 이 함수가 두 가지 이상의 일을한다고 사용자에게 알려준다. 이것이 나쁘다.
```js
    //bad
    function createFile(name, temp) {
      if (temp) {
        fs.create(`./temp/${name}`);
      } else {
        fs.create(name);
      }
    }
    
    //good
    function createFile(name) {
      fs.create(name);
    }
    
    function createTempFile(name) {
      createFile(`./temp/${name}`);
    }
```
- 전역 함수를 사용하지 말자(Good)
```js
    //bad
    Array.prototype.diff = function diff(comparisonArray) {
      const hash = new Set(comparisonArray);
      return this.filter(elem => !hash.has(elem));
    };
    
    //good
    class SuperArray extends Array {
      diff(comparisonArray) {
        const hash = new Set(comparisonArray);
        return this.filter(elem => !hash.has(elem));
      }
    }
```
- 명령형 코드 보단 함수형 프로그래밍을 지향 하자.(Good)
```js
    const programmerOutput = [
      {
        name: "Uncle Bobby",
        linesOfCode: 500
      },
      {
        name: "Suzie Q",
        linesOfCode: 1500
      },
      {
        name: "Jimmy Gosling",
        linesOfCode: 150
      },
      {
        name: "Gracie Hopper",
        linesOfCode: 1000
      }
    ];
    
    //bad
    let totalOutput = 0;
    for (let i = 0; i < programmerOutput.length; i++) {
      totalOutput += programmerOutput[i].linesOfCode;
    }
    //good
    const totalOutput = programmerOutput.reduce(
      (totalLines, output) => totalLines + output.linesOfCode,
      0
    );
```    

- 조건문 캡슐화(Good)
```js
    //bad
    if (fsm.state === "fetching" && isEmpty(listNode)) {
      // ...
    }
    
    //good
    function shouldShowSpinner(fsm, listNode) {
      return fsm.state === "fetching" && isEmpty(listNode);
    }
    
    if (shouldShowSpinner(fsmInstance, listNodeInstance)) {
      // ...
    }
```
- 부정 조건문을 피하자(Good?)
```js
    //bad
    function isDOMNodeNotPresent(node) {
      // ...
    }
    
    if (!isDOMNodeNotPresent(node)) {
      // ...
    }
    
    //good
    function isDOMNodePresent(node) {
      // ...
    }
    
    if (isDOMNodePresent(node)) {
      // ...
    }
```
- 조건문은 다형성을 이용해서 피하자.(?)

다형성을 이용해 동일한 작업을 수행할수있다. if 를 피하는 이유는 클린 코드의 원칙 때문이다. 함수는 한번에 한가지의 기능만 수행 해야 한다.! 즉, 함수의 단일책임 원칙을 지키자.
```js
    //bad
    class Airplane {
      // ...
      getCruisingAltitude() {
        switch (this.type) {
          case "777":
            return this.getMaxAltitude() - this.getPassengerCount();
          case "Air Force One":
            return this.getMaxAltitude();
          case "Cessna":
            return this.getMaxAltitude() - this.getFuelExpenditure();
        }
      }
    }
    
    //good
    class Airplane {
      // ...
    }
    
    class Boeing777 extends Airplane {
      // ...
      getCruisingAltitude() {
        return this.getMaxAltitude() - this.getPassengerCount();
      }
    }
    
    class AirForceOne extends Airplane {
      // ...
      getCruisingAltitude() {
        return this.getMaxAltitude();
      }
    }
    
    class Cessna extends Airplane {
      // ...
      getCruisingAltitude() {
        return this.getMaxAltitude() - this.getFuelExpenditure();
      }
    }
```
- 지나치게 최적화 하지 마라.(미지의 영역)

브라우저는 런타임에서 많은 최적화 작업을 하고 있기때문에 막연한 최적화를 하고 있다면 그것은 시간낭비 니까 정말 필요한 부분만 실용적으로 최적화 하자. **[여기](https://github.com/petkaantonov/bluebird/wiki/Optimization-killers)**에 최적화가 부족한 부분이 잘 나와 있다.

- 중복 코드를 피하도록 노력 하자.
- 사용하지 않는 코드는 제거 해라. (우린 버전 관리 하고 있으니까 너무 당연한!)

### Object

- getter 와 setter 를 이용
- make objects have private members with closures

### Class

- ES5 함수보다 ES6 의 클래스를 사용하자(상속이 편해짐)
- 메서드 채이닝을 이용
```js
    class Car {
      constructor(make, model, color) {
        this.make = make;
        this.model = model;
        this.color = color;
      }
    
      setMake(make) {
        this.make = make;
        // NOTE: Returning this for chaining
        return this;
      }
    
      setModel(model) {
        this.model = model;
        // NOTE: Returning this for chaining
        return this;
      }
    
      setColor(color) {
        this.color = color;
        // NOTE: Returning this for chaining
        return this;
      }
    
      save() {
        console.log(this.make, this.model, this.color);
        // NOTE: Returning this for chaining
        return this;
      }
    }
    
    const car = new Car("Ford", "F-150", "red").setColor("pink").save();
```
- 상속보단 조합을 이용

대부분의 경우 상속보단 조합이 좋은데 상속이 좋은 몇가지 경우가 있다.

1. 당신의 상속관계가 "has-a" 관계가 아니라 "is-a" 관계일 때 (사람->동물 vs. 유저->유저정보)
2. 기반 클래스의 코드를 다시 사용할 수 있을 때 (인간은 모든 동물처럼 움직일 수 있습니다.)
3. 기반 클래스를 수정하여 파생된 클래스 모두를 수정하고 싶을 때 (이동시 모든 동물이 소비하는 칼로리를 변경하고 싶을 때)
```js
    //bad
    class Employee {
      constructor(name, email) {
        this.name = name;
        this.email = email;
      }
    
      // ...
    }
    
    // 이 코드가 안좋은 이유는 Employees가 tax data를 "가지고" 있기 때문입니다.
    // EmployeeTaxData는 Employee 타입이 아닙니다.
    class EmployeeTaxData extends Employee {
      constructor(ssn, salary) {
        super();
        this.ssn = ssn;
        this.salary = salary;
      }
    
      // ...
    }
    
    //good
    class EmployeeTaxData {
      constructor(ssn, salary) {
        this.ssn = ssn;
        this.salary = salary;
      }
      
      // ...
    }
    
    class Employee {
      constructor(name, email) {
        this.name = name;
        this.email = email;
      }
    
      setTaxData(ssn, salary) {
        this.taxData = new EmployeeTaxData(ssn, salary);
      }
      // ...
    }
```
### Concurrency

콜백 보단 프로미스. 프로미스 보단 어싱크 어웨이트를 사용 하자.

### ErrorHandle

단순히 콘솔로그만 되어선 안된다. 에러를 표현하겠다는것은 에러에 대한 계획이나 장치가 있어야 한다는것
```js
    //good
    
    getdata()
      .then(data => {
        functionThatMightThrow(data);
      })
      .catch(error => {
        // One option (more noisy than console.log):
        console.error(error);
        // Another option:
        notifyUserOfError(error);
        // Another option:
        reportErrorToService(error);
        // OR do all three!
      });
```
### Formatting

- 일관된 대소문자를 사용 하자.
- 함수 호출자와 수신자는 가까이 있어야 한다. (코드를 위에서 아래로 읽는 경향이 있기 때문)
    - **public static private  등에 키워드가 있을 경우엔 어떤 배치 순서를 가져야 하는지가 의문이다JH**
```js
    //good
    class PerformanceReview {
      constructor(employee) {
        this.employee = employee;
      }
    
      perfReview() {
        this.getPeerReviews();
        this.getManagerReview();
        this.getSelfReview();
      }
    
      getPeerReviews() {
        const peers = this.lookupPeers();
        // ...
      }
    
      lookupPeers() {
        return db.lookup(this.employee, 'peers');
      }
    
      getManagerReview() {
        const manager = this.lookupManager();
      }
    
      lookupManager() {
        return db.lookup(this.employee, 'manager');
      }
    
      getSelfReview() {
        // ...
      }
    }
    
    const review = new PerformanceReview(employee);
    review.perfReview();
```
### Comments

버전 관리를 하고 있다면 작성 하지 말하야 한다. 코드 그 자체로 설명이 되어야 하기 때문

### SOLID

### Single Responsibility Prinsiple

클래스는 개념적으로 응집도가 낮아야 하고 수정 해야하는 이유가 2개 이상 있으면 안됩니다.
```js
    // bad
    class UserSettings {
      constructor(user) {
        this.user = user;
      }
    
      changeSettings(settings) {
        if (this.verifyCredentials()) {
          // ...
        }
      }
    
      verifyCredentials() {
        // ...
      }
    }
    
    // good
    class UserAuth {
      constructor(user) {
        this.user = user;
      }
    
      verifyCredentials() {
        // ...
      }
    }
    
    
    class UserSettings {
      constructor(user) {
        this.user = user;
        this.auth = new UserAuth(user);
      }
    
      changeSettings(settings) {
        if (this.auth.verifyCredentials()) {
          // ...
        }
      }
    }
```