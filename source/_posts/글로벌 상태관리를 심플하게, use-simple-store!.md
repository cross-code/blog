---
title: 글로벌 상태관리를 심플하게, use-simple-store!
date: 2019-05-16 00:00:00
tags: 
- react
- state
- hook
- redux
---

`react`의 대표적인 상태관리 도구인 `redux`는 **해야 할 게 너무 많습니다.** 
redux로 카운터를 만들 때를 상상해 보죠. 

## `redux`로 Counter 만들기
먼저 액션에 따라 카운터 숫자를 바꾸는 `reducer`를 작성하고 `store`를 생성합니다. 

```js
const counterReducer = (state = {count: 0}, action) => {
    switch (action.type) {
        case 'INCREMENT':
            return {...state, count: state.count + 1}
        case 'DECREMENT':
            return {...state, count: state.count - 1}
        default:
            return state
    }
}

export const store = createStore(counterReducer)
```

생성한 store를 아래와 같이 `<App/>`을 감싼 `Provider`에 인자로 넘깁니다.

```jsx
import { Provider } from 'react-redux'
import { store } from './store'

render(
    <Provider store={store}>
        <App />
    </Provider>,
    document.getElementById('root')
)
```

리듀서로 전달될, `action`을 생성하는 함수를 작성합니다.

```js
const increment = () => {
    return {type : 'INCREMENT'}
}
const decrement = () => {
    return {type : 'DECREMENT'}
}
```

이제 리액트 컴포넌트에 스토어의 상태를 props로 전달하기 위한 `mapStateToProps`, 그리고 액션을 `dispatch`하기 위한 `mapDispatchToProps`를 만들어 컴포넌트와 `connect`합니다.

```js
const mapStateToProps = state => {
    return { count: state.count }
}

const mapDispatchToProps = dispatch => {
    return {
        increment: () => dispatch(increment()), // 👈 아까 만들어둔 액션 생성함수를 사용합니다.
        decrement: () => dispatch(decrement())
    }
}

export default connect(mapStateToProps, mapDispatchToProps)(Counter)
```
그럼 끝? 아니, 아직 안 끝났을 수 있습니다.

지금은 액션타입을 문자열 그대로 작성했지만 이것을 상수로 분리해 관리하기도 합니다. 또 리덕스 커뮤니티는 리덕스와 리액트 컴포넌트 사이에 `Container component`라고 불리는 중간 레이어를 두는 것을 권장합니다. 위에서 connect한 Counter는 다시 `CounterContainer`와 `Counter`로 분리될 수 있을 겁니다.


컴포넌트가 많아질수록 이런 작업은 정말 피곤합니다. 만약 타입스크립트라도 사용한다면... 😱

이것들을 정말 줄일 수는 없을까요?

# use-simple-store 🏬
[use-simple-store](https://github.com/skt-t1-byungi/use-simple-store)는 이런 리덕스 장황한 작업들을 피하고, 보다 심플하게 리액트에서 글로벌 상태를 관리하기 위한 라이브러리입니다. 이번엔 use-simple-store를 사용해 카운터를 만들어 보면서 리덕스와 어떤 차이가 있는지 살펴봅시다.

## `use-simple-store`로 Counter 만들기

```js
import createStore from 'use-simple-store'

const {useStore, update} = createStore({count: 0})

const increment = () => update(state => {
    state.count = state.count + 1
})
 
const decrement = () => update(state => {
    state.count = state.count - 1
})
```
초기 상태 값으로 즉시 store를 생성합니다. store에는 `useStore`와 `update` 함수가 있습니다. `update`함수를 사용해 카운터 숫자를 변경할 수 있습니다.

```js
function Counter() {
    const {count} = useStore()
    return (
        <div>
            <span>{count}</span>
            <button onClick={decrement}> - </button>
            <button onClick={increment}> + </button>
        </div>))
}
```

`Provider`로 `<App/>`을 감싸거나 컴포넌트와 `connect`하는 과정이 없습니다. 리액트 16.8 부터 사용할 수 있는 `hook`을 사용해 스토어 상태를 즉시 컴포넌트로 가져옵니다.

끝? 네 이게 전부입니다.

## What?
use-simple-store는 리덕스와 달리 상태변경을 처리하는 프로세스를 액션, 리듀서로 분리하지 않습니다. 상태를 직접 변경하는 `mutate`함수를 `update(mutate)` 인자로 넘기면 됩니다. 리덕스에서 요구되던 아래 과정들을 하나로 통합할 수 있습니다.

1. 리듀서 작성
2. 액션생성자 정의
3. 액션 타입을 상수로 분리해서 별도로 관리
4. mapDispatchToProps

또 컴포넌트에 hook으로 스토어 상태를 즉시 가져옵니다. 아래 내용을 생략할 수 있습니다.

1. Provider로 `<App/>`을 감싸는 것.
2. mapStateToProps, mapDispatchToProps를 컴포넌트와 `connect`

## 다른 특징
### 더 나은 상태변경 코드
만약 객체 깊은(nested) 값을 변경해야 한다면, 리덕스는 불변성(immutable)을 위해 객체확산(object spread) 연산자를 남발(spread hell)합니다.
```js
const reducer = (state = INITIAL_STATE, action) => {
    switch (action.type) {
        case 'ADD_USER':
            return {
                ...state, 
                team: {
                    ...state.team,
                    users: [
                        ...state.team.users, 
                        action.newUser
                    ]
                }
            }
        /* ... */
        default: return state
    }
}
```

반면 use-simple-store는 state를 직접 바꿀 수 있어서 보다 깔끔한 코드를 작성할 수 있습니다. (내부적으로 immutable 라이브러리인 [immer](https://github.com/immerjs/immer)을 사용합니다.)
```js
const addUser = newUser => update(state => {
    state.team.users.push(newUser)
})
```

### 비동기 액션
update함수는 promise나 비동기함수(async function)을 직접 지원하지 않습니다. 대신 아래와 같이 작성할 수 있습니다.
```js
async function fetchTodos() {
    update(state => {
        state.fetching = true
    })

    const todos = await fetchTodosAsync()

    update(state => {
        state.fetching = false
        state.todos = todos
    })
}
```

### Hook - `useStore([selector[, deps]])`
```js
const counter = useStore(state => state.counter)
```
`useStore(selector)`에 `selector`함수를 인자로 넘겨 `mapStateToProps` 처럼 원하는 값만 고를 수 있습니다.
```js
const value = useStore(state => state[condition ? 'v1' : 'v2'], [condition])
```
selector 함수가 외부 변수에 의존한다면, 다른 리액트 훅처럼 `dependencies`를 훅에 알려줘야 합니다.

## Trade-off
심플함을 위해 포기한 것들이 있습니다. 

### Provider 부재
use-simple-store는 `Context API`의 `Provider`로 `<App/>`을 감싸지 않습니다. 컴포넌트와 싱글톤에 가까운 스토어가 맞바로 결합합니다. 컴포넌트의 유닛테스트를 힘들고 컴포넌트 재사용을 어렵게 할 수 있습니다.

### 사용자 액션과 스토어 변경함수가 서로 강하게 결합합니다.
사용자의 액션과 각 스토어(또는 리듀서)의 변경은 1:1 관계가 아닐 수 있습니다. 포스트(post)에 코멘트를 작성하는 임의의 사용자 액션(`WRITE_COMMENT`)을 생각해 봅시다. 이 액션이 실행되면 포스트(post) 스토어는 `포스트에 달린 총 댓글 수`를 +1 합니다. 동시에 화면에는 내가 단 댓글 수를 표시하는 곳도 존재합니다. 내 정보(myProfile) 스토어의 `내가 단 댓글 수`도 +1 해야 합니다. `포스트 스토어의 포스트에 달린 총 댓글 수` 와 `내 정보 스토어의 내가 단 댓글 수`는 의미도 다르고 다른 스토어에 위치하지만 같은 액션(`WRITE_COMMENT`)에 반응해야 합니다.

```js
const postReducer = (state, action) => {
    switch (action.type) {
        case 'WRITE_COMMENT':
            return {...state, commentCount: state.commentCount + 1}
        /* ... */
        default: return state
    }
}

const myProfileReducer = (state, action) => {
    switch (action.type) {
        case 'WRITE_COMMENT':
            return {...state, commentCount: state.commentCount + 1}
        /* ... */
        default: return state
    }
}
```

이러한 이유로 리덕스나 더 오래된 플럭스(flux)는 사용자 액션과 스토어(리듀서)를 분리하고 스토어가 사용자 액션을 관찰 가능하도록 설계해 느슨한 관계를 가지도록 하였습니다. 

use-simple-store는 이런 분리를 다시 통합했기 때문에, 만약 동일한 액션에 반응해야 하는 2개 이상의 스토어가 있을 경우, 아래같이 명시적인 함수 호출이 필요합니다.

```js
function writeComment(){
    addPostCommentCount()
    addMyProfileCommentCount()
}
```

스토어의 로직이 변경됐을 때, 코드수정이 스토어 뿐 아니라 여러 군데서 이뤄져야 할 수도 있습니다. 그러나 대개는 특수하고 빈번하지 않습니다. 

## 마치며
저는 만족하며 사용하고 있답니다. 😎 

만약 다른 의견이나 제안이 있다면 [https://github.com/skt-t1-byungi/use-simple-store/issues](https://github.com/skt-t1-byungi/use-simple-store/issues)에 이슈를 남겨주세요. 답변드리겠습니다. 감사합니다.


## 작성자
<img src="https://avatars2.githubusercontent.com/u/16894698?v=4" width="110" align="left" style="margin-right: 10px"> 

**BYUNGI**<br>https://github.com/skt-t1-byungi<br>`write less, do more`
 