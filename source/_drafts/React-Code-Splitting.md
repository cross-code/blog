---
title: React Code Splitting
tags:
  - react
  - code splitting
  - codeJS
date: 2019-09-18 23:54:31
---

## 코드 분할

번들링은 훌륭하지만 앱이 커지면 번들도 커집니다. 프로젝트 초기에는 괜찮지만, 프로젝트 규모가 커지고 최적화가 필요한 단계에서 Code Splitting을 한다면 웹서비스 퍼포먼스가 좋아지기 때문에 더 좋은 사용자 경험을 줄 수 있습니다.

## 방법 #1. React.lazy
React.lazy 함수를 사용하면 동적 import를 사용해서 컴포넌트를 렌더링 할 수 있습니다.
React.lazy는 React 16.6.0 버전 부터 지원합니다. 

### Before
``` javascript
import OtherComponent from './OtherComponent';
```

### After
``` javascript
const OtherComponent = import React.lazy(() => import('./OtherComponent'));
```

### With Suspense
lazy 컴포넌트는 Suspense 컴포넌트 하위에서 렌더링되어야 하며, Suspense는 lazy 컴포넌트가 로드되길 기다리는 동안 로딩 화면과 같은 예비 컨텐츠를 보여줄 수 있게 해줍니다.
``` javascript
const OtherComponent = React.lazy(() => import('./OtherComponent'));

function MyComponent() {
  return (
    <div>
      <Suspense fallback={<div>Loading...</div>}>
        <OtherComponent />
      </Suspense>
    </div>
  );
}
```

하나의 Suspense 컴포넌트로 여러 lazy 컴포넌트를 감쌀 수도 있습니다.
``` javascript
const OtherComponent = React.lazy(() => import('./OtherComponent'));
const AnotherComponent = React.lazy(() => import('./AnotherComponent'));

function MyComponent() {
  return (
    <div>
      <Suspense fallback={<div>Loading...</div>}>
        <section>
          <OtherComponent />
          <AnotherComponent />
        </section>
      </Suspense>
    </div>
  );
}
```

### Route-based code splitting
앱 코드를 분리하는 단위는 선택이지만, 좋은 방법 중에 하나는 라우트 단위로 분할 하는 방법입니다.
``` javascript
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';
import React, { Suspense, lazy } from 'react';

const Home = lazy(() => import('./routes/Home'));
const About = lazy(() => import('./routes/About'));

const App = () => (
  <Router>
    <Suspense fallback={<div>Loading...</div>}>
      <Switch>
        <Route exact path="/" component={Home}/>
        <Route path="/about" component={About}/>
      </Switch>
    </Suspense>
  </Router>
);
```

### 코드 Build 결과 
``` javascript
react-app
 dist/
  - index.html
  - main.b1234.js (contains Appcomponent and bootstrap code)
  - home.bc4567.js (contains Home)
  - about.bc4567.js (contains About)
  
/** index.html **/
<head>
  <div id="root"></div>
  <script src="main.b1234.js"></script>
</head>
```

 
###

## 방법 #2. Loadable Components
React.lazy 가 서버 사이드 렌더링을 지원하지 않습니다. 코드 분할을 클라이언트, 서버사이드 모두 가능하도록 만들고 싶다면 Loadable Components를 사용하면됩니다. (React 공식 문서에서 권장하는 방법)





## 번외, React-loadable
React-loadable 모듈은 React 공식 문서에서도 오랫동안 권장하던 방법이었습니다. 그러나 현재는 더 이상 유지 관리되지 않으며 Webpack v4 + 및 Babel v7 +와 호환되지 않는 부분들이 있습니다.
그래서 React.lazy 또는 Loadable Components로 변경하는것을 추천합니다.

## 참고

prefetch: resource is probably needed for some navigation in the future
preload: resource might be needed during the current navigation

- [webpack code splitting](https://webpack.js.org/guides/code-splitting/)

## 작성자

<img src="https://avatars2.githubusercontent.com/u/1393664?s=200&v=4" width="100" align="left" style="margin-right: 10px">

**codeJS 🐘**<br>https://github.com/dodortus<br>`simple is best`
