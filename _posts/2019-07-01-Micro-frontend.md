---
date: '2019-07-01 08:48:05'
layout: post
title: Micro Frontends
subtitle: ''
description: >-
 복잡한 문제에 직면 했을때 우리는 어떻게 해야 할까? 문제를 작게 나누고 작은 문제를 해결 한다. 하나씩 해결 하다 보면 그 문제를 해결 할 수 있게 된다. 이 생각을 웹에 적용해 보자. 웹 서비스는 크고 복잡해 질 수 있고 지금도 충분히 그러 하다. 동의 하는가? 이것은 모두가 다 아는 사실 이다.
image: /assets/img/uploads/mf.jpg
category: blog
tags:
  - Frontend
  - Architecture
author: jacob
paginate: false
---
### Micro Frontends

가끔 들어가는 페이스북 커뮤니티를 통해서 해당 개념을 최근에 접하게 되었다.

복잡한 문제에 직면 했을때 우리는 어떻게 해야 할까? 문제를 작게 나누고 작은 문제를 해결 한다. 하나씩 해결 하다 보면 그 문제를 해결 할 수 있게 된다. 이 생각을 웹에 적용해 보자. 웹 서비스는 크고 복잡해 질 수 있고 지금도 충분히 그러 하다. 동의 하는가? 이것은 모두가 다 아는 사실 이다.

그럼 웹에서 생길수 있는 복잡한 문제를 어떻게 해결 해야 할까? 데이터 단위로 작게 나누고 작은 문제를 하나씩 해결한다. 이것이 우리가 알고 있는 마이크로 서비스 의 개념 이다.

그리고 이 개념을 프론트 엔드에 접목 시킨 것이 마이크로 프론트 엔드 이다.

예상 했겠지만 이것은 전혀 새로운 개념이 아니다. 어떠한 문제를 해결 하기 위한 하나의 방법론 이다.

### Conclusion

앞서 나는 이것을 방법론이라고 이야기 했다. 그렇다면 이 방법론이 내가 있는 회사에 필요할까? 라는 질문을 먼저 해 볼 수 있을것이다. 결론을 말 하자면 지금 상황에선 필요가 없다고 생각 한다.

Micro Frontends 줄여서 MF 라 부르겠다. MF 의 핵심 아이디어를 읽고 느낀바 이다.

[Micro Frontends - extending the microservice idea to frontend development](https://micro-frontends.org/#serverside-rendering--universal-rendering)

- 우리팀은 3명이 같은 (React)스택을 선택해서 성장 하고 있다. 단일 팀이고 다른 팀과 스택을 가지고 조정할 필요가 없다.
- global css 의 오염은 styled-components 를 통해서 해결 하고 있다.
- React Native 의 컴포넌트를 온전히 활용 하고 있다. 즉, 단순 웹뷰로 이용할 경우 유용할지 모르겠으나. 웹뷰 영역이 극히 적기 때문에 효용 가치가 없다.

자체적인 결론은 내렸지만 이것은 어디까지나 현재의 상황에서 내려진 결론이다. 가능성은 충분히 열려 있다.

- 백엔드 서비스는 이미 AWS 기반의 마이크로 서비스 아키텍쳐로 되어 있다.
- 조직이 린디벨롭 관점에서 지금 보다 빠른 실행을 원한다면 RN 을 버리고 혹은 대부분의 뷰를 웹뷰로 전환 하고 웹 중심에 환경으로 방향을 전환 할 수 있다.

그래서 이야기를 좀더 풀어가 보겠다.

### What problem do you solve?

Monolithic Frontend 가 가진 문제를 완화 시켜 준다.

백엔드에서는 프론트 엔드에 있는 UI 코드를 업데이트 하지 않고 비즈니스 가치를 제공 하기 어렵다.

UI코드가 있는 HTML 을 백엔드가 돌려 줄 수 있지만 대부분이 저장소에 있는 데이터를 돌려주는 API 이기 때문이다. 

서비스가 폭풍 성장 하는 상황을 가정해 보자. 사용자 요구에 맞추기 위해 백엔드에서 수많은 기능이 개발 된다. 이에 대응해서 프론트엔드 UI 를 개발 해야 한다. 하나의 기능을 추가해서 발생되는 사이드 이펙트를 테스트하는 시간과 빌드 시간이 늘어난다.

팀 전체의 관점으로 봤을때 유연하지 못한 상황이다. 여기서 프론트 엔드를 백엔드와 같은 단위에 도메인으로 수직분할을 하게 되면 늘어나는 시간을 절약 할 수 있게 된다.

하지만 이는 어디까지나 서비스가 잘 됬을때 이야기 이다.  단일 팀에 의해 유지 될만큼 작은 서비스의 경우에 이것은 오버 엔지니어링 이라고 생각 한다.

아키텍쳐의 변경은 문제를 완화.해결 하기 위한 수단이지 목표가 되어서는 안되는것 같다.

### **Disadvantages**

단점으로 무엇이 있을까? 

- 페이로드가 증가.
- 프로비저닝과 자동화에 필요한 시간.
- 팀내 가치 충돌
- 코드 품질과 일관성 또는 거버넌스.

결국엔 기술적,조직적 성숙도 를 고려 해야 한다. 

### Let's do it

개념적으로 어느정도 이해가 갔다고 생각 했지만, 코드를 통해서 보면 조금더 이해 할 수 있을거란 기대를 하게 되었다.

그래서 single-spa 를 이용해서 그대로 코드를 작성해 봤다.

single-spa 는 front-end microservices 를 구축하기 위한 라이브러리 이다. 

이 라이브러리의 핵심 개념은 최상위 라우터에서 동적 로딩을 통해 각 SPA 의 엔드포인트를 랜더링 하게 되는데 인기 있는 대중적인 라이브러리들과 함께 구현 할 수 있다.

자습서만 따라해 봐도 개념을 이해하는데 큰 도움이  될것 같다.

### **Test Example**

[https://github.com/hwangtan/micro-frontends](https://github.com/hwangtan/micro-frontends)

### Reference

- [https://single-spa.js.org/](https://single-spa.js.org/)
- [https://www.youtube.com/watch?v=BuRB3djraeM](https://www.youtube.com/watch?v=BuRB3djraeM)
- [https://www.martinfowler.com/articles/micro-frontends.html](https://www.martinfowler.com/articles/micro-frontends.html)
- [https://micro-frontends.org](https://micro-frontends.org/#)
