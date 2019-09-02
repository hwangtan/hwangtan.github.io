---
date: '2019-03-27 08:48:05'
layout: post
title: Mono Repository with Lerna
subtitle: ''
description: >-
 모노 레포는 모든 소스 코드를 단일 저장소에서 제어하는 패턴이다. 
image: /assets/img/uploads/1_obV0EGRGtftMYtM4_DTTQA.png
category: blog
tags:
  - mono repository
  - lerna
author: jacob
paginate: false
---

# monorepo with lerna

## 무엇

모든 소스 코드가 단일 저장소에 보관되는 소스 제어 패턴이다.

## 왜 사용 하는가

monorepo 를 사용하는 주된 이유 는 저장소 관리의 단순성과 팀 전체의 코드 재사용을 위함 이다.

### 문제점

여러 애플리케이션에서 코드를 공유해야하는 필요성이 느껴지면 저장소를 만들고  npm 패키지로 게시 할 수 있는데 애플리케이션 및 공유 구성 요소의 수가 증가함에 따라 고민 해야할 문제가 생긴다.

**테스트 :** 공유 패키지를 제대로 테스트하려면 여러 프로젝트를 로컬로 연결해야 하는데  상호 의존 패키지의 수가 늘어나고 npm 연결로 인해 예기치 않은 동작이 발생할 수 있기 때문에 어려운 작업이 될 수 있다

**게시** : 픽스를 전파하려면 서로 다른 상호 의존적 인 프로젝트를 올바른 순서로 업데이트하고 게시해야 하는 문제가 있다.

**검색 가능성** : 재사용 가능한 구성 요소를 찾기가 어려울 수 있다

**리팩토링 :** 리팩토링은 여러 패키지에 대한 업데이트로 이어져 위에서 언급 한 테스트 및 게시 문제를 야기 할 수 있다.

### 장점

**대규모 리펙토링**단일 API에서 API와 모든 호출자를 한 번 에 리팩터링 하거나 코드 [모드](https://github.com/facebook/jscodeshift) 를 활용 하여 전체 리포지토리의 코드를 리팩터링 한 다음 자동으로 추가 된 모든 코드 소유자와 함께 코드 검토를 할 수 있다.

**모든 것이 항상 통합되어 있음**

**여러 저장소로 인해 많은 반복이 발생.** 여러 저장소는 구성 및 프로젝트 설정에서 많은 반복을 초래할 수 있다

- 개발 환경
- 구성 작성
- 종속성
- 테스트 구성
- ESLint
- CI / CD

### Reference 

[https://itnext.io/guide-react-app-monorepo-with-lerna-d932afb2e875](https://itnext.io/guide-react-app-monorepo-with-lerna-d932afb2e875)

[https://marketplace.visualstudio.com/itemdetails?itemName=orepor.color-tabs-vscode-ext](https://marketplace.visualstudio.com/itemdetails?itemName=orepor.color-tabs-vscode-ext)
