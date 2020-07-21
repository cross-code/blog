---
title: 화면에 보이는 요소를 감지하는 IntersectionObserver 알아보기
tags:
  - IntersectionObserver
  - LasyLoading
  - codeJS
date: 2020-07-16 23:11:31
---

## IntersectionObserver API란

<strong>IntersectionObserver API는 타겟 엘리멘트가 화면(viewport) 내에 보이고 있는지 관찰하는 API입니다.</strong>
크롬 51버전부터 사용 가능하며, 현재 대부분의 모던 브라우저에서 지원합니다.
IntersectionObserver 이전엔 화면에 보이는 요소를 감지하려면 Scroll 이벤트를 사용하여 스크롤의 좌표와 화면의 크기를 더해서 관찰하려는 요소의 offset 좌표가 포함되는지를 계산해야 하는 번거로움뿐만 아니라 반복적인 스크롤 연산 처리 과정의 성능 이슈가 따릅니다. 그래서 아래와 같은 상황에 이 API를 활용하는 것을 권장합니다.

![IntersectionObserver](./intersection-observer.png)

## 어떻게 활용하면 좋을까요?

- Image Lasy loading 구현할 때
- Content Lasy loading 구현할 때
- Infinite scrolling 구현할 때
- 화면에 보이는 요소 에니메이션 처리할 때

## 주요 API 알아보기

### 구문

```javascript
const observer = new IntersectionObserver(callback, options);
```

### 속성

필수값이 아닌 Optional이며, 옵저버를 조정할 수 있는 옵션 객체로 위 구문의 options에 기입합니다.
지금은 이런게 있다 정도만 보고, 자세히 다뤄보고자 할 때 다시 봐도 늦지 않는다.

- root
  - 대상 요소 (element) 를 감시할 상위 요소.
  - default: null (document가 뷰포트로 사용된다)
- rootMargin
  - 바깥 여백(Margin)을 이용해 Root 범위를 확장하거나 축소할 수 있습니다.
  - default: 0px 0px 0px 0px
- threshold
  - 관측 대상이 화면에 어느 정도 노출될때 보인다고 판단할지 비율을 지정할 수 있다. (0.0 ~ 1.0 지정 가능하고, 0.0은 관측 대상이 1픽셀이라도 보이면, 1.0은 요소가 전부 보일때를 의미한다)
  - default: 0.0

### 메서드

## 간단한 예제로 동작 원리 알아보기

화면에 보이는 블럭은 배경색이 차오르고, 블럭이 화면 밖으로 나가면 다시 배경색이 사라지는 예제입니다.

```javascript
const observer = new IntersectionObserver(() => {});
```

## 브라우저 지원 현황

[Polyfill](https://github.com/w3c/IntersectionObserver/tree/master/polyfill)을 사용하여 IE와 더 많은 하위 브라우저들도 지원할 수 있습니다.
![Browser compatibility](./browser-compatibility.png)

## 참고

- [IntersectionObserver MDN](https://developer.mozilla.org/ko/docs/Web/API/IntersectionObserver/IntersectionObserver)
- [IntersectionObserver Polyfill](https://github.com/w3c/IntersectionObserver/tree/master/polyfill)
- [react-intersection-observer](https://www.npmjs.com/package/react-intersection-observer)
- [react-visibility-sensor](https://github.com/joshwnj/react-visibility-sensor)
- [react-in-viewport](https://www.npmjs.com/package/react-in-viewport)

## 작성자

<img src="https://avatars2.githubusercontent.com/u/1393664?s=200&v=4" width="100" align="left" style="margin-right: 10px">

**codeJS 🐘**<br>https://github.com/dodortus<br>`simple is best`
