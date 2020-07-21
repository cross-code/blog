---
title: 화면에 보이는 요소를 감지하는 IntersectionObserver 알아보기
tags:
  - IntersectionObserver
  - LasyLoading
  - codeJS
date: 2020-07-16 23:11:31
---

<strong>IntersectionObserver API는 타겟 엘리멘트가 화면(viewport) 내에 보이고 있는지 관찰하는 API입니다.</strong>
크롬은 51버전 부터 사용가능하며, 현재 대부분의 모던 브라우저에서 동작합니다.
IntersectionObserver 이전엔 화면에 보이는 요소를 감지하려면 Scroll 이벤트를 감지하여 스크롤의 좌표와 화면의 크기를 더해서 관찰하려는 요소의 offset 좌표가 포함되는지 계산해야 하는 번거로움이 있었는데 이 API를 사용하면 간단합니다.

![IntersectionObserver](./intersection-observer.png)

## 주요 API 알아보기

## 어떻게 활용하면 좋을까요?

- Image Lasy loading 구현할 때
- Content Lasy loading 구현할 때
- Infinite scrolling 구현할 때
- 화면에 보이는 요소 에니메이션 처리할 때

## 간단한 예제로 동작 원리 알아보기

## 브라우저 지원 현황

[Polyfill](https://github.com/w3c/IntersectionObserver/tree/master/polyfill)을 사용하면 IE와 하위 브라우저도 지원할 수 있습니다.

## 참고

- [IntersectionObserver MDN](https://developer.mozilla.org/en-US/docs/Web/API/IntersectionObserver)
- [IntersectionObserver Polyfill](https://github.com/w3c/IntersectionObserver/tree/master/polyfill)

## 작성자

<img src="https://avatars2.githubusercontent.com/u/1393664?s=200&v=4" width="100" align="left" style="margin-right: 10px">

**codeJS 🐘**<br>https://github.com/dodortus<br>`simple is best`
