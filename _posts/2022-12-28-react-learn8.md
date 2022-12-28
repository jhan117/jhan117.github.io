---
title: "React가 작동하는 방법과 최적화 테크닉"
categories:
  - "React"
toc: true
toc_label: "section 12"
toc_sticky: true
last_modified_at: 2022-12-28
---

## 학습 목표

- React는 어떻게 일하고 있을까? (How Does React Work Behind The Scenes?)
- Virtual DOM 과 DOM 업데이트에 대해서 이해하기(Understanding the Virtual DOM & DOM Updates)
- State와 State 업데이트 이해하기(Understanding State & State Updates)

## 리액트와 최적화 테그닉 살펴보기(A Look Behind The Scenes Of React & Optimization Techniques)

### How Does React Work?

React는 유저 인터페이스를 만드는 JS 라이브러리라고 하였다. React는 Components만 다룬다. Components에서는 우리가 배웠듯이 부모 컴포넌트에서 데이터를 전달하는 Props, 내부 데이터를 관리하는 State, Component-wide 데이터를 관리하는 Context 등의 기능을 한다. 그래서 이 내부 기능들이 변경되면 컴포넌트도 변경되어 재평가된다. React는 이 컴포넌트가 화면에 뭘 그리려고 하면 ReactDOM에게 알린다.

ReactDOM은 웹이랑 상호작용을 한다. 이는 유저가 보는 곳이다.

그렇다면 React와 ReactDOM은 서로 통신을 도대체 어떻게 하는 걸까?

React는 Virtual DOM이라는 개념을 사용한다. 이는 컴포넌트 트리가 "현재" 어떻게 보이는지 보여져야 하는 것이 무엇인지를 결정한다. 그리고 ReactDOM은 변경된 부분만 받고 나서 real DOM을 조작한다.

이때 컴포넌트가 재평가된다는 것이 DOM을 리렌더링이라고 생각하면 안된다. 왜냐하면 컴포넌트는 props, state, context가 변경되면 재평가되어 컴포넌트 함수가 실행되지만 Real DOM은 필요한 경우에만 변경되기 때문이다. 성능면에서 이는 중요하다. 이전과 현재의 상태를 가상으로 비교한다는 것은 간편하고 자원도 적게 들기 때문이다.

그런데 만약 소규모 변경이 여러곳에서 일어난다면 페이지가 느려질 수 있다.

결론적으로, React는 Virtual DOM과의 비교를 통해 최종 스냅샷과 현재의 스냅샷을 Real DOM에 전달하는 구조를 갖는다.

### 컴포넌트 업데이트와 재평가에 대해서 알아보기

앞서 React는 내부 기능이 업데이트 되면 재평가 되고 ReactDOM은 바뀐 경우에만 변경이 된다. 예를 들어 태그 추가하거나 텍스트 수정 등을 말한다.

그런데 React가 재평가를 할 때 자식 컴포넌트도 전부 재평가가 된다. 이는 낭비이다. 물론 간단한 작은 앱에서는 문제가 되지 않는다 React도 최적화가 어느정도 되어 있기 때문이다. 그러나 큰 앱에서는 최적화를 해주는 것이 좋다.

### `memo()`

우리는 위의 문제를 `memo()`를 이용하여 특정 상황에만 재평가하라고 지시할 수 있다. 이 함수는 props의 값이 바뀐 경우에만 재평가하라고 시킨다. 사용한 컴포넌트의 자식도 재평가 되지 않는다. 이는 함수형 컴포넌트에서만 사용할 수 있다.

```js
import { memo } from "react";

export default memo(DemoOutput);
```

그렇다면 모든 컴포넌트에 이렇게 하면 되는데 왜 안하냐고 묻는다면 바로 비용이 들기 때문이다. 비교하는 작업이 필요하다. 이는 개별적인 성능 비용이 필요하기 때문에 컴포넌트를 재평가하는 데 필요한 비용과 props를 비교하는 성능 비용을 고려해서 사용하는 것이 좋다.

memo에는 문제가 있다. 바로 객체나 배열이나 함수는 JS 특성상 비교할 때 각각 새로운 것이라고 판단하기 때문에 값이 변했다고 판단해 실제로는 같은 함수인데도 재평가가된다.

### `useCallback()`

이는 memo의 문제를 해결해준다. 함수를 저장할 수 있게 해준다. 그래서 실행할 때마다 함수를 재생성할 필요가 없다고 알려줄 수 있다. 의존성 배열을 두 번째 인자로 받는다.

```js
import { useCallback } from "react";

const func = useCallback(() => {
  // ... 생략
}, []);
```

이렇게 해주면 memo를 사용할 수 있다.

그렇다면 왜 의존성 배열이 필요한 것일까? 클로저 개념을 알고 있다면 이해하기 쉬울 것이다. 예를 들어 외부 변수인 allow가 true일 때 실행한다고 해보자. 의존성 배열에 추가하지 않으면 처음 실행되었을 때 클로저된 allow 값을 계속 들고 있을 것이다. 그래서 allow가 변경될 때 함수가 재생성되도록 하면 allow는 최신값을 가질 수 있다.

```js
const func = useCallback(() => {
  if (allow) {
    // ...
  }
}, [allow]);
```

### Components & State

State Management와 Components의 상호작용은 React가 관리하며 이는 React의 핵심적인 개념이다. 일반적으로 둘이 상호작용하는 방법은 useState hook을 사용하는 것이다.

React는 기본값에 대해서는 한 번만 처리하도록 한다. 처음 렌더링이 될 때 말이다. 그래서 DOM에 컴포넌트가 연결되고 유지되는 동안에는 최초의 초기화 이후에는 갱신만 된다. 예를 들어, DOM이 삭제된다면, 초기화는 한 번 더 이루어진다.

이는 Reducer에서도 동일하게 작동한다.

### State Updates & Scheduling

그렇다면 React는 State 업데이트를 어떻게 할까?

이전에 즉각적으로 업데이트 하지 않는다고 배웠다. State 업데이트 함수를 호출하면 데이터와 함께 예약을 한다. 이것이 Scheduled State Change이다. 보통 너무 빨라서 즉각적으로 처리되는 것처럼 보인다. 그러나 React는 이를 지연시킨다. 그래서 언젠간 예약된 함수가 실행이 되면 컴포넌트를 재평가하게된다.

이 Scheduling 때문에 다수의 Scheduled State Change가 있을 수 있다. 그렇기 때문에 State를 업데이트를 하고자 할 때 함수 형식으로 하는 것을 추천한다고 한 이유이다. 특히 이전 스냅샷에 의존하고 있다면 필수이다.

만약 함수 안에서 두 가지의 state 업데이트 함수가 있다고 가정해보자. Scheduled State Change가 두 개 존재하는 것이 아닌 하나의 Scheduled State Change에 존재한다.

### `useMemo()`

함수를 반환하기 위해서는 useCallback을 사용하였다. 값을 반환하기 위해서는 useMemo를 사용하면 된다. 만약 데이터를 처리하는 배열(sort된 배열)이 있다면, 매일 sort하고 불러오는 것은 너무 오래 걸리는 문제가 있다. 이를 해결하기 위해 useMemo를 사용할 수도 있다.

```js
const sortedList = useMemo(() => {
  return items.sort((a, b) => a - b);
}, [items]);
```

---

강의명

- Udemy : React 완벽 가이드
