---
title: "React 기초"
categories:
  - "React"
toc: true
toc_label: "section 3"
toc_sticky: true
last_modified_at: 2023-01-05
---

## JSX

표현식을 사용할 때는 중괄호로 감싸서 사용할 수 있으며 태그를 사용하면 된다.

```js
const name = "kaye";
const element = <h1>Hello, {name}</h1>;

if (name) {
  return <h1>Hello, {name}</h1>;
}
return <Welcome />;
```

## 함수 컴포넌트 정의

반환할 때는 하나의 루트를 가져야 한다.

```js
// 일반 함수
function Welcome(props) {
  return (
    <div>
      <h1>Hello, {props.name}</h1>
      <p>I hope you enjoy learning React!</p>
    </div>
  );
}

// 화살표 함수
const Welcome = (props) => {
  return (
    <div>
      <h1>Hello, {props.name}</h1>
      <p>I hope you enjoy learning React!</p>
    </div>
  );
};

// 모듈 내보내기
export default Welcome;
```

## props

얼마든지 프로퍼티를 넣어줄 수 있다. `props.children`은 예약어이고 자식 컴포넌트 및 텍스트를 렌더링 한다.

```js
function Hello(props) {
  return (
    <div>
      <Welcome name="Sara" age="20" />
      <p>{props.children}</p>
    </div>
  );
}
```

## css 파일 적용하기

모듈로 불러오는 방법도 있다.

```js
import "./Welcome.css";

function Welcome(props) {
  return <h1 className="header">Hello, {props.name}</h1>;
}
```

### 사용자 정의 컴포넌트에 클래스명 지정하는 법

프로젝트를 할 때 '사용자 정의 컴포넌트'의 경우 페이지 명을 넘겨줘서 각각 스타일을 할당해 줬지만 이게 더 깔끔한 것 같아서 기억하고 싶다.

```js
// 일반 css 파일
function Card(props) {
  const classes = "card" + props.className;

  return <div className={classes}>{props.children}</div>;
}

// module.css 파일
import classes from "./Card.module.css";

function Card(props) {
  const customClass = "card" + props.className;

  return <div className={classes[customClass]}>{props.children}</div>;
}
```
