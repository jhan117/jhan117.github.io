---
title: "React 강의 정리 5"
categories:
  - "React"
toc: true
toc_label: "section 9"
toc_sticky: true
last_modified_at: 2022-12-26
---

## 학습 목표

- JSX 제한사항 & Fragments (JSX Limitations & Fragments)
- Portals를 이용해서 깨끗한 DOM을 얻기 (Getting a Cleaner DOM with Portals)
- Refs 다루기 (Working with Refs)

## 심화 학습 더 깊이 다이빙 : Fragments, Portals & "Refs"

### JSX 제한사항 및 Fragments

루트 JSX 요소를 하나 이상 return해서는 안된다. 이유는 JS 문법이 그렇기 때문이다. 하나만 반환해야 한다. 물론 배열을 반환할 수 있겠지만 이것도 결국엔 하나만 반환한다는 뜻이다.

이 문제를 해결하기 위해 세가지 방법이 있다.

1. 하나의 태그(예를 들어 `<div>`)로 감싸준다.
   "`<div>` Soup"가 발생한다. 불필요한 div가 생성되고 이 많은 div를 렌더링 하려면 결국엔 브라우저는 느려질 것이라는 단점이 있다.
2. 리스트로 반환한다.
   항상 key 값을 넣어줘야 하는 번거로움이 있다.
3. Wrapper를 만든다.
   이렇게 해도 작동한다. div없이 사용할 수 있는 좋은 속임수라고 한다.

   ```js
   // Helpers/Wrapper.js
   const Wrapper = (props) => {
     return props.children;
   };
   ```

   ```js
   // Hello.js
   <Wrapper>
     <div />
     <div />
   </Wrapper>
   ```

4. `<Fragment></Fragment>`를 사용한다.
   3번도 좋지만 굳이 우리가 만들 필요가 없다. import 해서 사용하면 된다. 내부적으로는 동일하게 동작한다.

   ```js
   // 일반적인 경우
   import React from 'react';

   <React.Fragment>
    <div/>
    <div/>
   </React.Fragment>

   // import Fragment
   import { Fragment } from 'react';

   <Fragment>
    <div/>
    <div/>
   </Fragment>

   // 빌드 워크플로가 지원하는 경우 이렇게 사용할 수도 있다.
   <>
    <div/>
    <div/>
   </>
   ```

### Portals

Fragments와 비슷한 기능을 한다. 이는 오버레이 컴포넌트 위치를 조정하고 싶을 때 사용한다.

예를 들어 폼에서 제출하면 모달 창이 뜨는 기능이라고 가정을 해보자. 이 때, 보통 폼 컴포넌트 상단에 모달 컴포넌트를 배치할 것이다.

```js
<Fragment>
  <Modal />
  <Form />
</Fragment>
```

구현하는데에는 문제 없으나 Real DOM에서 렌더링 될 때 구조적인 관점이나 의미적인 관점이나 HTML 코드의 안의 깊은 곳에 자리잡고 있다. 이는 스크린 리더가 오버레이라고 인식하지 못할 가능성이 있다. 그도 그럴 것이 오버레이는 최상단에서 작동하기 때문이다. css 스타일링으로 기능은 구현했어도 오버레이인지 구별하지 못한다. 이해가 안될 수도 있는데, 버튼을 만들 때 `<button>`대신 `<div>`로 버튼 기능을 만드는 것이나 마찬가지라고 한다. 즉, 의미를 알기 어렵다는 뜻이다.

```html
<!--  Good  -->
<div class="modal"></div>
<section>
  <form></form>
</section>

<!-- Bad -->
<section>
  <div class="modal"></div>
  <form></form>
</section>
```

이럴 때 Portals를 사용할 수 있다. Portal은 두가지가 필요하다. 하나는 컴포넌트를 이동시킬 장소. 다른 하나는 그곳에 포털을 가져야 한다고 알려주는 것이다.

먼저 이동시킬 루트를 생성한다.

```html
<!-- public/index.html -->
<body>
  <div id="backdrop-root"></div>
  <div id="overlay-root"></div>
  <div id="root"></div>
</body>
```

backdrop과 overlay 컴포넌트를 분리한다. 강사분은 한 번만 사용할 것이기 때문에 파일로 따로 분리하지 않았다. 그러나 파일로 분리해도 괜찮다.

중괄호를 치고 ReactDOM에서 `createPortal()` 함수를 불러온다.

`ReactDOM.createPortal(child, container)` 첫 번째 인자(child)는 엘리먼트, 문자열, 혹은 fragment와 같은 어떤 종류이든 렌더링할 수 있는 React 자식이다. 두 번째 인자(container)는 DOM 엘리먼트이다.

```js
// ErrorModal.js
// react 18에서는 /client 추가
import ReactDOM from "react-dom/client";

// ... Backdrop, ModalOverlay 컴포넌트 선언 생략

return (
  <Fragment>
    {ReactDOM.createPortal(
      <Backdrop onConfirm={props.onConfirm} />,
      document.getElementById("backdrop-root")
    )}
    {ReactDOM.createPortal(
      <ModalOverlay
        title={props.title}
        message={props.message}
        onConfirm={props.onConfirm}
      />,
      document.getElementById("overlay-root")
    )}
  </Fragment>
);
```

전부 쓰기 전이랑 정확히 같은 방식으로 동작하지만 이러한 기능으로 인해 의미적으로 더 정확한 HTML 코드를 쓸 수 있게 됐다. 접근성을 높이니 좋은 것!

### Refs

기본적인 기능은 다른 DOM 요소에 접근해서 그것들도 작업할 수 있게 해주는 것이다. 가장 많이 쓰이는 곳은 input 태그이지만 여러 요소에 다 쓸 수 있다. ref의 값은 항상 객체이며, current 속성을 가지고 있다. 그 current는 연결된 ref의 실제 값을 갖는다. 기본값은 정의되지 않은 값이다. 실제 값은 real DOM 노드이다. 그래서 여러 가지 작업을 할 수 있다. 그러나 DOM은 리액트가 작동하게 만드는 것이 좋다. 즉, 업데이트 하려고 하는 것은 비효율적이다. 편하게 쓰려고 리액트를 접한게 아닌가? 그러니 우리는 그저 읽는 것만 해보는 것이 좋겠다.

만약 input 태그에 ref를 사용한다면 우리는 기존의 키가 입력될 때마다 작용하는 state들은 전부 필요 없어졌다. 그러면 다 삭제함으로써 코드 양을 줄일 수 있다. 그런데 이렇게 하면 입력 값이 제출될 때 초기화가 되지 않는 문제가 생긴다. 어떻게 초기화를 다시 시킬까? 두 가지 방법이 있다. state 방식을 다시 사용하는 것으로 돌아가거나 아니면 아까는 DOM을 작동시키지 말라고 했지만 이 경우에는 빈 문자열로 할당해주는 방법이 있다. 그러나 흔히 사용되는 방법은 아니다.

```js
import { useRef } from "react";

const Hello = () => {
  const inputRef = useRef();

  const addHandler = () => {
    const enteredInput = inputRef.current.value;

    inputRef.current.value = ""; // 초기화
  };

  return <input ref={inputRef} />; // value, onChange는 필요 없어졌다.
};
```

그렇다면 state와 ref 무엇을 사용하는 게 좋을까?

만약 값을 빠르게 읽고 싶을 때가 있을 것이다. 바꿀 생각 없이 오직 읽는 용도로만 사용한다면 ref를 사용하는 것이 좋겠다. 예를 들어 본인은 이전 프로젝트에서 디자인을 위해 댓글 창의 높이를 가져와야 했는데 ref를 이용해 컴포넌트의 높이를 가져와 계산하는 식으로 사용했다.

그리고 흔히 Input 태그의 경우는 선호하는 대로 사용하면 된다. state를 사용하면 깔끔하지만 코드를 좀 더 많이 써야 하는 단점이 있고 ref는 코드가 적지만 DOM을 예외적으로 동작해야 한다는 단점이 있다.

편한대로 사용하면 되지만 리액트 프로젝트에서는 ref를 많이 사용한다고 한다. 요소에 접근하기 쉽고 아까 DOM 노드가 값이라고 했는데 이 때문에 더 많은 기능이 있기 때문이다.

이렇게 DOM 요소, 특히 입력 요소와 상호작용하는 이 방법에는 특별한 이름이 있다. 바로 제어되지 않는 컴포넌트라고 한다. 즉, ref로 접근하는 경우를 말한다.

왜 제어되지 않는다고 할까?

그것은 바로 리액트에 의해 제어되지 않기 때문이다. 아까 우리가 DOM을 직접 업데이트 한 걸 기억하는가? 그것은 리액트로 제어한 것이 아닌 직접 내부에 제어한 것이기 때문에 이를 제어되지 않는 컴포넌트라고 한다. 즉, 입력 요소의 state를 제어하지 않기 때문이라고 이해하면 되겠다.

리액트 세계에서 자주 등장하는 용어들이니 기억해두는 게 좋겠다!

---

강의명

- Udemy : React 완벽 가이드
