---
layout: page
title: About
description: Some description.
permalink: /about/
---

<img itemprop="image" class="img-rounded" src="/assets/img/uploads/wj.png" alt="Your Name">

## About

<p>
자바스크립트를 주력 언어로 서비스를 만들고 있습니다. 1년전 부터 Typescript 를 주로 사용 하고 있으며 UI 를 그리는데 라이브러리는 Angular(ver.1) 6개월, React 2년 6개월 이상의 경험을 가지고 있습니다. RESTful API / Graphql  환경에서 데이터 연동 경험을 가지고 있으며 마크업은 css in js 방법론을 선호해서 styled-components 를 적극 활용 합니다. 요즘의 관심사는 마이크로 인터렉션입니다. 작은 변화로 사용자에게 큰 기쁨을 줄 수 있다고 믿기 때문입니다. 나와 우리가 사랑하는 서비스를 만드는데 기여하는것이 개발자로서 저의 지향점 입니다.
</p>

<div id="root" style="width:100%; height: 100%;"></div>
<script>
        ! function (f) {
            function e(e) {
                for (var r, t, n = e[0], o = e[1], u = e[2], l = 0, a = []; l < n.length; l++) t = n[l], Object
                    .prototype.hasOwnProperty.call(p, t) && p[t] && a.push(p[t][0]), p[t] = 0;
                for (r in o) Object.prototype.hasOwnProperty.call(o, r) && (f[r] = o[r]);
                for (s && s(e); a.length;) a.shift()();
                return c.push.apply(c, u || []), i()
            }

            function i() {
                for (var e, r = 0; r < c.length; r++) {
                    for (var t = c[r], n = !0, o = 1; o < t.length; o++) {
                        var u = t[o];
                        0 !== p[u] && (n = !1)
                    }
                    n && (c.splice(r--, 1), e = l(l.s = t[0]))
                }
                return e
            }
            var t = {},
                p = {
                    1: 0
                },
                c = [];

            function l(e) {
                if (t[e]) return t[e].exports;
                var r = t[e] = {
                    i: e,
                    l: !1,
                    exports: {}
                };
                return f[e].call(r.exports, r, r.exports, l), r.l = !0, r.exports
            }
            l.m = f, l.c = t, l.d = function (e, r, t) {
                l.o(e, r) || Object.defineProperty(e, r, {
                    enumerable: !0,
                    get: t
                })
            }, l.r = function (e) {
                "undefined" != typeof Symbol && Symbol.toStringTag && Object.defineProperty(e, Symbol.toStringTag, {
                    value: "Module"
                }), Object.defineProperty(e, "__esModule", {
                    value: !0
                })
            }, l.t = function (r, e) {
                if (1 & e && (r = l(r)), 8 & e) return r;
                if (4 & e && "object" == typeof r && r && r.__esModule) return r;
                var t = Object.create(null);
                if (l.r(t), Object.defineProperty(t, "default", {
                        enumerable: !0,
                        value: r
                    }), 2 & e && "string" != typeof r)
                    for (var n in r) l.d(t, n, function (e) {
                        return r[e]
                    }.bind(null, n));
                return t
            }, l.n = function (e) {
                var r = e && e.__esModule ? function () {
                    return e.default
                } : function () {
                    return e
                };
                return l.d(r, "a", r), r
            }, l.o = function (e, r) {
                return Object.prototype.hasOwnProperty.call(e, r)
            }, l.p = "/";
            var r = window["webpackJsonpabout-me"] = window["webpackJsonpabout-me"] || [],
                n = r.push.bind(r);
            r.push = e, r = r.slice();
            for (var o = 0; o < r.length; o++) e(r[o]);
            var s = n;
            i()
        }([])
    </script>
<script src="/assets/js/2.53703bf6.chunk.js"></script>
<script src="/assets/js/main.1067cece.chunk.js"></script>