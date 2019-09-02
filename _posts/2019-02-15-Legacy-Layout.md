---
date: '2019-02-15 08:48:05'
layout: post
title: Legacy Layout
subtitle: ''
description: >-
 현재는 flex 를 통해 쉽게 레이아웃을 그리고 있지만, 전통적인 웹에서 레이아웃을 그릴 때 position 과 float 을 이용해 화면을 그리고 있고 아직도 많은 시스템들이 이 전통적인 레이아웃 작성 방식을 따르고 있다.(GRID, FLEX 등에 이전세대를 legacy layout 이라 부른다.)
image: /assets/img/uploads/old-bannack-townsite-4027297_640.jpg
category: blog
tags:
  - welcome
  - blog
author: jacob
paginate: false
---

# CSS: Legacy layout

현재는 flex 를 통해 쉽게 레이아웃을 그리고 있지만, 전통적인 웹에서 레이아웃을 그릴 때 position 과 float 을 이용해 화면을 그리고 있고 아직도 많은 시스템들이 이 전통적인 레이아웃 작성 방식을 따르고 있다.(GRID, FLEX 등에 이전세대를 legacy layout 이라 부른다.)

보다 잘 그리기 위해선 과정을 이해 하는 것이 중요하다. 그래서 왜 이렇게 그려 지는가 에 대한 고찰을 해보려 한다.

## **GRAPHICS SYSTEM**

> x, y, width, height, color, left, top

그래픽 시스템은 고정된 값 을 통해서 화면을 그린다.

고전적인 그래픽 시스템은 고정된 숫자 기반에 화면을 그리기 때문에

가변 되는 화면에 대응 할 수 없다. 

그래서 가변 되는 숫자 대신 메타포를 사용 한다.

> %, left, block, inline, float...

## RENDERING SYSTEM

브라우저는 영역을 나눈(geometry calculate)뒤에 영역을 칠(fragment fill)한다.

영역을 나누기 위해 계산 하는 것을 reflow 라 하고, 나뉜 영역을 칠 하는것을 repainting 이라 한다.

## NORMAL FLOW

> position: static | relative

position: 어떠한 지오메트리 영역에 위치를 결정하게 만드는 추상적인 시스템을 의미한다.

BFC: block formatting contexts

IFC: inline formatting contexts

- relative 는 static 으로 **그린뒤**에 상대적으로 위치를 이동 시킨다.
- html 은 공백문자가 없는 문자열을 하나의 인라인으로 본다.
```html
    <div style="width: 500px; border: 1px solid #ccc;">
      **
      <span>
        HELLO
        <span>WORLD
          <div style="background-color:red;">&nbsp;</div>
        </span>
        !!
        <div style="background-color:blue;">&nbsp;</div>
      </span>
      **
    </div>
```
## FLOAT

> float: left | right

float 는 **라인 박스 공식**으로 그려 진다.

- 기존 BFC 박스를 파기하고 새로운 BFC 박스가 생성 되고, 위로 뜬다.
```html
    <div style="width: 500px; border: 1px solid #ccc;">
      <div style="height: 50px; background:red;"></div>
      <div style="width: 200px; height: 150px; float:left; background:rgba(0,255,0,0.5);"></div>
      HELLO
      <div style="height: 50px; background: skyblue">WORLD</div>
    </div>
```
- 인라인 가드가 생긴다.
```html
    <div style="width: 500px; border: 1px solid #ccc;">
      <div style="float:left; width: 200px; height:150px; background:green; opacity: .8;">1</div>
      <div style="float:right; width: 50px; height:150px; background:red; opacity: .8;">2</div>
      <div style="float:right; width: 50px; height:100px; background:orange; opacity: .8;">3</div>
      <div style="float:left; width: 150px; height:50px; background:azure; opacity: .8;">4</div>
      <div style="float:right; width: 150px; height:70px; background:beige;">5</div>
      <div style="float:left; width: 150px; height:50px; background:cadetblue;">6</div>
      <div style="float:left; width: 150px; height:50px; background:cyan;">7</div>
      <div style="height: 30px; background: gold;">ABC1 ABC2 ABC3 ABC4 ABC5 ABC6 ABC7 ABC8</div>
      <div style="height: 30px; background: gold; overflow:hidden;">A</div>
      <div style="height: 15px; background: gainsboro; overflow:hidden;">B</div>
      <div style="height: 30px; background: black;"></div>
    </div>
```
원래 컨텐츠가 크면 BFC 박스를 밀어낼때 인라인 글자는 늘어난다. 근데 라인박스때문에 인라인요소가 밀려서 가드작동이 발생하면 늘어나지 않는다.(중요!)

- overflow: hidden 또는 scroll 을 가질때 만 새로운 박스가 생긴다.
```html
    <div style="width: 500px; border: 1px solid #ccc;">
      <div style="float:left; width: 200px; height:150px; background:green; opacity: .8;">1</div>
      <div style="float:right; width: 50px; height:150px; background:red; opacity: .8;">2</div>
      <div style="float:right; width: 50px; height:100px; background:orange; opacity: .8;">3</div>
      <div style="float:left; width: 150px; height:50px; background:azure; opacity: .8;">4</div>
      <div style="float:right; width: 150px; height:70px; background:beige;">5</div>
      <div style="float:left; width: 150px; height:50px; background:cadetblue;">6</div>
      <div style="float:left; width: 150px; height:50px; background:cyan;">7</div>
      <div style="height: 30px; background: gold; overflow:hidden;">A</div>
      <div style="height: 15px; background: gainsboro; overflow:hidden;">B</div>
      <div style="height: 30px; background: black;"></div>
      <div style="height: 30px; background: brown; overflow:hidden;">C</div>
      <div style="height: 20px; background: hotpink; overflow:hidden;">D</div>
      <div style="height: 30px; background: black;"></div>
      <div style="height: 20px; background: mediumblue; overflow:hidden;">E</div>
      <div style="height: 30px; background: black;"></div>
      <div style="height: 20px; background: indigo; overflow:hidden;">F</div>
      <div style="height: 30px; background: black;"></div>
    </div>
```
float 가 normal flow 랑 어떠한 관계가 있는지, float 가 overflow hidden 이랑 어떤 관계가 있는지 이해 할 수 있었다.

## Reference

코드스피츠: [CSS Rendering](https://www.youtube.com/watch?v=_o1zsrBkZyg)-1 [CSS Rendering](https://www.youtube.com/watch?v=ybNH1a14vQY)-2
