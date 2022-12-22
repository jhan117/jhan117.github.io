---
title: "React 강의 정리 2"
categories:
  - "React"
toc: true
toc_label: "section 4 : 리액트 State 및 이벤트 다루기"
toc_sticky: true
last_modified_at: 2022-12-22
---

## 학습 목표

- 이벤트 처리하기 (Handling Events)
- UI를 업데이트하고 "State" 사용해보기 (Updating the UI & Working with "State")
- Components와 State에 대해 자세히 살펴보기 (A Closer Look At Components & State)

### Handling Events

기존에는 addEventListener를 호출하는 방식이었다. 이는 명령형으로 React에서는 JSX에 on으로 시작하는 이벤트 props를 선언형으로 넘겨주면 이벤트를 처리할 수 있다.

이벤트 함수 이름은 자유지만 보통 이벤트 + Handler == 'clickHandler'로 짓는다고 한다. onClick 안에 함수를 작성할 수 있겠지만 가독성을 위해서 JSX구문보다는 위에다가 작성하는 것을 추천한다.

```js
const Hello = () => {
  const clickHandler = () => {
    console.log("Clicked!!!");
  };

  return <button onClick={clickHandler}>click button</button>;
};
```

### Updating the UI & Working with "State"

이벤트 핸들러 함수에 특정 변수를 변경해도 값은 변경되지만 화면에는 구성이 안되는 것을 확인할 수 있다. 이는 React가 한 번 불러오면 다시 실행하지 않기 때문인데 다시 실행하더라도 초기화된 값만 평생 가질 것이다. React는 이를 위해 "State"라는 개념을 도입했다. 물론 React에만 있는 개념은 아니다.

react의 훅은 use로 시작해서 알아차리기 쉽다. 그리고 컴포넌트 함수 안에서 호출해야 한다. 중첩된 함수나 함수 밖은 허용하지 않는다. useState는 초기값을 넘겨줄 수 있으며 배열을 반환하는데 첫 번째는 읽기 전용이고 두 번째는 쓰기 전용이다. react는 state가 변경된 컴포넌트만 재평가한다.

```js
import { useState } from "react";

const Hello = () => {
  const [text, setText] = useState("click");

  const clickHandler = () => {
    setText("update");
    console.log("Clicked!!!");
  };

  return <button onClick={clickHandler}>{text}</button>;
};
```

#### 여러가지 useState 호출

State는 독립적이기에 여러번 선언해도 상관없다. 합쳐서 한 줄로해도 되고 다수의 줄로 생성해도 된다.

```js
// 독립적
const [enteredTitle, setEnteredTitle] = useState("");
const [enteredAmount, setEnteredAmount] = useState("");
const [enteredDate, setEnteredDate] = useState("");

// 한 줄로
const [userInput, setUserInput] = useState({
  enteredTitle: "",
  enteredAmount: "",
  enteredDate: "",
});
```

만약 한줄로 객체로 초기화를 했다면 state를 업데이트 하는 방법에는 두 가지가 있다. 만약, 상태 업데이트가 이전 상태에 의존하고 있다면 항상 최신 상태를 가져오는 2번째 방법을 추천한다. 예를 들어, 카운트를 할 경우가 있겠다.

```js
// 객체로 업데이트 : 오래된 상태를 가져올 수도 있음에 주의한다.
setUserInput({
  ...userInput,
  enteredTitle: event.target.value,
});

// 함수로 업데이트 : 항상 이전의 최신 상태를 가져온다.
setUserInput((prevState) => {
  return { ...prevState, enteredTitle: event.target.value };
});
```

#### 양방향 바인팅

양방향 바인딩이 되어 있는데 업데이트를 할 수 있고 그 상태를 다시 보내주기도 한다. 무한루프처럼 들리겠지만 그렇진 않으니 문제는 없다. 이는 폼 전송을 할 때 유용하다.

```js
<input
  value={enteredTitle} // state 값 보내주기
  onChange={(e) => {
    setEnteredTitle(e.target.value); // state 값 업데이트하기
  }}
/>
```

#### 자식 컴포넌트 → 부모 컴포넌트로 데이터 전달하는 법 (상향식)

관례적으로 on으로 시작해서 넘겨주는 걸 선호한다고한다. 핵심은 부모에서 함수를 props로 넘겨주고 자식에서 props로 함수를 실행하는 것이다. state 끌어올린다고도 한다.

만약 자식1 -> 자식2으로 전달하고 싶으면 그 자식들의 부모를 통해 끌어올린 후 전달하는 식으로 해야 한다.

```js
// 부모 컴포넌트
const Parent = () => {
  const saveDataHandler = (data) => {
    console.log(data);
  };

  return (
    <div>
      <Child onSaveData={saveDataHandler} />
    </div>
  );
};
```

```js
// 자식 컴포넌트
const Child = (props) => {
  return (
    <button
      onSubmit={() => {
        const originData = "this is data";

        props.onSaveData(originData);
      }}
    ></button>
  );
};
```

### A Closer Look At Components & State

#### State

컴포넌트별 인스턴트에 대해서 독립된 state를 가진다. 즉 state가 적용된 똑같은 컴포넌트를 재사용해도 독립적으로 이벤트가 진행된다는 장점이 있다.

---

강의명

- Udemy : React 완벽 가이드
