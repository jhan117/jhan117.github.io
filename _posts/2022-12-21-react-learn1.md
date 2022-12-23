---
title: "React 강의 정리 1"
categories:
  - "React"
toc: true
toc_label: "section 3"
toc_sticky: true
last_modified_at: 2022-12-21
---

## 함수 컴포넌트 정의

강의하시는 분은 화살표 함수가 더 간략해서 본인이 더 선호한다고 한다. 그러나 어떻게 쓰든 상관은 없다.

```js
// 일반 함수
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

// 화살표 함수
const Welcome = (props) => {
  return <h1>Hello, {props.name}</h1>;
};

// 모듈 내보내기
export default Welcome;
```

반환할 때는 하나의 루트를 가져야 한다.

```js
// GOOD
function Welcome(props) {
  return (
    <div>
      <h1>Hello, {props.name}</h1>
      <p>I hope you enjoy learning React!</p>
    </div>
  );
}
```

```js
// BAD
function Welcome(props) {
  return (
    <h1>Hello, {props.name}</h1>
    <p>I hope you enjoy learning React!</p>
    );
}
```

## props

얼마든지 프로퍼티를 넣어줄 수 있다.

```js
function Hello() {
  return <Welcome name="Sara" age="20" />;
}
```

`props.children`은 예약어이다.

```js
function Welcome(props) {
  return <div>{props.children}</div>;
}
```

```js
function Hello() {
  return <Welcome>this is props.children</Welcome>;
}
```

이렇게 사용하면 내용이 보인다. 텍스트말고 컴포넌트를 넣어도 그대로 렌더링된다.

## 컴포넌트 렌더링

```js
// DOM 태그로 만든 일반적인 컴포넌트
const element = <div />;
// 사용자 정의 컴포넌트
const element = <Welcome name="Sara" />;
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

프로젝트를 할 때 '사용자 정의 컴포넌트'의 경우 page명을 넘겨줘서 각각 스타일을 할당해 줬지만 이게 더 깔끔한 것 같아서 기억하고 싶다.

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
