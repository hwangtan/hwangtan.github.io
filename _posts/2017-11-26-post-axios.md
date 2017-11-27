---
layout: post
title: "axios 를 이용한 HTTP 요청과 개선될수 있는 코드들"
date: 2017-11-26 01:07:22
image: 'https://res.cloudinary.com/jacob-dev/image/upload/c_scale,w_786/v1511688592/axios-cover.png'
description:
category: 'HTTP'
tags: 
 - axios
 - http
twitter_text:
introduction: 서버의 요청과 응답을 fetch 인터페이스를 이용해 처리하면서 다양한 상황을 핸들링 하다보니 핸들러가 너무 커지게 되었습니다. axios 로 대안을 찾게 되었고 fetch 에서 axios 로 갈아타면서 얻게된 이점에 대해서 설명하고자 합니다. 
---
서버에 비동기 요청을 보내고 응답 결과를 받아서 처리할때 다양한 처리 방법이 있습니다.    
라이브러리를 이용하지 않는다면 XMLHttpRequest(XHR) 객체를 생성하여 처리할수 있을것 이구요.
제이쿼리를 이용해 $.ajax() 나 $.get() 등으로 처리했을수도 있을것 같습니다.   

Promise 패턴이 Callback Hell을 어느정도 완화 시켜주면서 HTTP Client 라이브러리들 역시 Promise 기반 함수들을 제공하기 시작했습니다. 여러 라이브러리들 중에 저의 경우 whatwg 에서 기준으로 정한 fetch api 를 사용해 왔습니다.

IE에서 사용하려면 폴리필을 해야하지만, 인터페이스가 간단하고, 이에 따른 응답과 요청 처리 핸들링이 자유로워서 선택 하였습니다.    
 서비스가 확장되면서 구현한 요청과 응답 핸들러의 크기가 커짐에 따라 조금더 다양한 인터페이스가 구현된 라이브러리 도입에 필요성을 느끼게 되었습니다.
기존에 fech로 된 요청과 응답을 axios 로 바꾸면서 어떤 이점이 있었는지 이야기를 하고자 합니다.

## GET

```js
//fetch
import 'whatwg-fetch';
import ax from 'axios';
const SERVICE_URI = process.env.SERVICE_URI;
const axios = ax.create({
    baseURL: SERVICE_URI,
    withCredentials: true,
});
const queryString = (params) => new URLSearchParams(params).toString();

// fetch
const getUsers = (params = {}) =>
 fetch(`${SERVICE_URI}/user?${queryString(params)}`, {
    method: 'GET',
    credentials: 'include',
  }).then(fetchHandler);
// axios
const getUsers = (params = {}) =>
  axios.get('/users', { params: params }).then(fetchHandler);
```
axios 는 기본옵션을 설정하여 인스턴스를 생성 할수 있습니다.
ax.create 를 통해 기본URL 과 credentials 를 지정해준것을 볼수 있습니다. 또한 axios 의 옵션 프로퍼티중에
params 의 벨류로 객체를 넘기게 되면 키벨류 쌍으로 쿼리스트링을 생성해주는것을 볼수 있습니다.
axios 의 옵션 프로퍼티에도 method 가 있지만, axios 의 메서드명을 이용한 방식이 더 명시적이라고 생각 합니다.

## Promise All

동시요청을 처리하기히위해서 Promise.all 을 이용할수 있습니다. 
카테고리와 태그를 호출한다는 가정으로 예로 들어 보겠습니다.
```js
import ax from 'axios';
const SERVICE_URI = process.env.SERVICE_URI;
const axios = ax.create({
    baseURL: SERVICE_URI,
    withCredentials: true,
});
const getCategories = () => axios.get('/categories').then(fetchHandler);
const getTags = () => axios.get('/tags').then(fetchHandler);

Promise.all([
    getCategories(),
    getTags(),
]).then((response) => {
    const categoryList = response[0].list;
    const tagList = response[1].list;
    // anything else...
}).catch(exceptionHandler);

axios.all([
    getCategories(),
    getTags(),
]).then(axios.spread((categories, tags) => {
    const categoryList = categories.list;
    const tagList = tags.list;
    // anything else...
})).catch(exceptionHandler);
```
Promise.all 은 요청을 배열로 받고, 응답을 배열로 리턴합니다.
응답을 배열의 인덱스번호로 접근하는것은 보기 좋은 코드가 아니라고 생각 합니다.
axios 에 spread 를 이용하면 요청한 배열의 순서대로 함수의 인자가 되어서 응답이 리턴 됩니다.
인자명을 명시적으로 작성하고 접근하여 다른 사람이 요청에 대한 응답의 결과를 명확히 알수 있도록 하는것이 좋습니다.


...작성중 금주내로 12월 3일 이내로 작성완료할 예정입니다.