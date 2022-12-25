---
title: "React 강의 정리 3"
categories:
  - "React"
toc: true
toc_label: "section 5 ~ 7"
toc_sticky: true
last_modified_at: 2022-12-23
---

## 학습 목표

**렌더링 리스트 및 조건부 Content**

- Content의 동적 목록 출력하기 (Outputting Dynamic Lists Of Content)
- 조건부 Content 렌더링하기 (Rendering Content Under Certain Conditions)

**리액트 컴포넌트 스타일링**

- 조건부 & 동적 스타일 (Conditional & Dynamic Styles)
- Styled Components
- CSS Modules

**리액트 앱 디버깅하기**

- 에러 메시지 이해하기 (Understanding Error Messages)
- 디버깅 & React 앱 분석하기 (Debugging & Analyzing React Apps)
- React devTools 사용하기 (Using React DevTools)

## 렌더링 리스트 및 조건부 Content

### 동적으로 목록 출력하기

기존에는 아이템을 복붙해서 나열하는 식으로 작성했다. 그러나 이 방법으로는 몇 개의 아이템이 필요할지 모르고 아이템이 추가되면 업데이트 되지 않기 때문에 정적 목록이었다. 이제 동적으로 map 함수를 이용해서 작성할 수 있다.

key props를 추가하지 않으면 react는 모든 아이템을 업데이트 하는 과정을 거친다. 이렇게 되면 성능면에서 좋지 않고 버그가 생길 가능성이 있으므로 react가 아이템을 구분할 수 있도록 key값을 넘겨주는 것을 선호한다. 또한 인덱스로 부여하는 것도 좋지 않다. 직접적으로 아이템에 붙인 이름도 아니며, 버그를 발생시킬수도 있다. 똑같은 id를 가진 다른 아이템이 있을 수도 있기 때문이다.

```js
// 정적 목록
<Card>
  <ExpenseItem
    title={props.items[0].title}
    amount={props.items[0].amount}
    date={props.items[0].date}
  />
  <ExpenseItem
    title={props.items[1].title}
    amount={props.items[1].amount}
    date={props.items[1].date}
  />
</Card>;

// 동적 목록
{
  props.items.map((expense) => (
    <ExpenseItem
      key={expense.id}
      title={expense.title}
      amount={expense.amount}
      date={expense.date}
    />
  ));
}
```

### 조건부 내용 출력하기

만약 목록에 아이템이 하나도 없다면 알려줘야 한다. 그렇지만 jsx에 중괄호 안에 for이나 if같은 긴 문법은 사용할 수 없다. 그대신 삼항 연산자를 사용하여 조건문을 생성할 수 있다. 또한 논리 연산자를 이용해 두 줄로 쓸 수 있다. 그러나 이들도 jsx에 쓰면 과해보일 수 있다. 그래서 변수를 이용하는 방법도 있다. 가독성이 좋은 건 변수를 이용하는 것이지만 사람들마다 선호하는 방법이 다 다르기에 본인이 선호하는 것을 사용하면 된다.

```js
// 삼항 연산자 사용
// 조건식 ? true : false
{
  items.length === 0 ? (
    <p>No items found.</p>
  ) : (
    items.map((item) => <Item key={item.id} />)
  );
}

// 논리 연산자 사용
// 조건식 && true
// 조건식이 true이면 다음 식을 반환함을 이용한 것
{
  items.length === 0 && <p>No items found.</p>;
}
{
  items.length > 0 && items.map((item) => <Item key={item.id} />);
}

// 변수 이용
let content = <p>No items found.</p>; // 초기화

if (items.length > 0) {
  content = items.map((item) => <Item key={item.id} />);
}

return <div>{content}</div>;
```

만약 조건부로 반환하고자 하는 컴포넌트가 전부라면 이런 방식도 사용할 수 있다. 보여주고 싶은 부분을 바로 return해서 변수를 저장하지 않고도 깔끔하게 작성할 수 있다.

```js
// 조건 명령문 반환
const list = (props) => {
  if (props.items.length === 0) {
    return <p>No items found.</p>;
  }

  return (
    <ul>
      {props.items.map((item) => (
        <Item key={item.id} />
      ))}
    </ul>
  );
};

export default list;
```

## 리액트 컴포넌트 스타일링

### css에서 동적 스타일 추가하기

만약 값에 따라 변하는 스타일을 추가하고 싶다면 style로 넘겨주면 된다. 대신 객체 상태로 넘겨줘야 하며 key는 css 프로퍼티 이름을 작성하고 value에는 value를 작성해주면 된다. 만약 "-"가 들어간 프로퍼티를 주려면 카멜케이스로 적어서 넘겨주면 된다. 예를 들면, 'background-color'는 'backgroundColor'로 사용된다. 이를 inline 스타일이라고 한다. 그렇지만 이는 css보다 최우선으로 적용되기 때문에 선호하지 않는다고 한다.

```js
// 변수가 변하는 경우
let barHeight = props.height + "%";

return <div style={{ height: barHeight, backgroundColor: "black" }}></div>;

// 조건이 있는 경우
<div style={{ backgroundColor: !isValid ? "red" : "black" }}></div>;
```

위의 inline 스타일 대신 쓸 수 있는 방법은 동적 클래스를 추가하는 것이다.

```js
<div className={`default ${!isValid ? "invalid" : ""}`}></div>
```

### css 작성하는 법

기존 css 파일을 작성하면 전역으로 적용되기 때문에 이름이 겹치면 스타일이 중복 적용되는 문제점이 있다. 이를 해결하기 위해 크게 두가지 방법을 사용할 수 있는데 하나는 Styled Components 라이브러리를 사용하는 것이고 두번째는 css Module을 사용할 수 있다. 이 둘은 모두 작성한 파일에서만 클래스가 유효하다.

3가지 방법 중 어떤 것을 사용해도 본인의 취향이라고 한다. 각각의 장단점이 있으니 선호하는 것을 고르자.

#### Styled Components

[공식 사이트](https://styled-components.com/) | [공식 문서](https://styled-components.com/docs)

- 설치법 with npm : `npm install --save styled-components`

사용법은 일단 라이브러리를 import 한 뒤 styled.{html tag 이름}뒤에 백틱 두개로 구성되어 있다.

```js
import styled from "styled-components";

const Button = styled.button`
  color: white;

  &:hover {
    color: red;
  }
`;
```

만약 동적 클래스를 추가하고 싶다면 두 가지 방법이 있다.

```js
// className으로 넘겨주는 방법
const Form = styled.form`
  color: black;

  & label {
    font-size: 1rem;
  }

  &.invalid {
    color: red;
  }
`;

return (
  <div>
    <Form className={!isValid && "invalid"}>
      <label />
    </Form>
  </div>
);
```

위의 방법도 괜찮지만 styled-components가 제공하는 기능은 props를 이용하는 것이다.

```js
// props 이용하는 방법
const Form = styled.form`
  border: 1px solid ${(props) => (props.invalid ? "red" : "#ccc")};
  color: ${(props) => (props.invalid ? "red" : "black")};

  & label {
    font-size: 1rem;
  }
`;

const Hello = () => {
  return (
    <div>
      <Form invalid={!isValid}>
        <label />
      </Form>
    </div>
  );
};
```

미디어 쿼리도 사용할 수 있다.

```js
const Button = styled.button`
  @media (min-width: 768px) {
  }
`;
```

#### CSS Modules

어떤 방법을 선택하든 상관없지만 강의하시는 분은 js에서 css 파일을 분리하는 것을 좋아해서 css module을 사용한다고 한다.

css 모듈은 그 기능을 지원하도록 설정된 프로젝트에서만 사용가능한데 코드 실행 전에 변환이 필요하기 때문이다. 다행히 React는 지원한다고 한다.

[Create React App - Adding a CSS Modules Stylesheet](https://create-react-app.dev/docs/adding-a-css-modules-stylesheet)

1. module.css 파일로 생성한다.
2. classes 또는 styles로 import 한다. (이름은 자유이지만 저 둘을 많이 쓴다.)

```js
import classes from "./Button.module.css";
import styles from "./Button.module.css";
```

3. 클래스네임에 문자열 대신 classes 또는 styles를 적는다.

classes 또는 styles는 객체로 클래스 이름들을 프로퍼티로 갖는다.

```js
// 예를 들어 .button 클래스가 있다면
<div className={classes.button}></div>

// 만약 '-'를 넣고 싶다면
<div className={classes['button-control']}></div>
```

만약 동적 클래스를 추가하고 싶다면 다음과 같은 방법을 쓸 수 있다.

```js
<div
  className={`${styles["form-control"]} ${!isValid && styles.invalid}`}
></div>
```

미디어 쿼리는 기존의 CSS 처럼 module.css에 사용하면 된다.

## 리액트 앱 디버깅하기

### 에러 메시지 이해하기

`Parsing error: Adjacent JSX elements must be wrapped in an enclosing tag. Did you want a JSX fragment <>...</>?` -> 한 개의 루트 요소로 반환해야 하는데 그렇지 못한 경우에 나타나는 에러 메시지이다. 원인은 React.createElement가 하나의 요소만 반환하기 때문이다.

```js
// 감싸주면 해결되는 문제였다
return (
  <div>
    <section />
    <section />
  </div>
);
```

`'adHandler' is not defined` -> 오타거나 선언하지 않았거나 등등 함수를 찾을 수 없을 때 나타나는 에러 메시지이다. 오타이면 수정해주면 된다. 선언한 부분으로 가 원인을 찾아야 한다.

### 디버깅 & React 앱 분석하기

만약 논리적 오류를 만난다면

- Console 창에 뜬 Warning 메시지를 읽고 오류를 해결할 수 있다.
- Warning 메시지가 없거나 breakpoint를 이용해 작업하고 싶다면 breakpoint를 이용하면 된다.
  1.  브라우저의 Sources 탭에서 원본 소스 파일을 찾는다.
  2.  breakpoint를 설정하고 하나씩 실행하면서 저장된 값을 보면서 파악하면 된다.

### React devTools 사용하기

크롬에서 설치할 수 있다. React devTools라고 치면 React Developer Tools 라는 이름의 확장 프로그램을 설치할 수 있다.

그러면 Components와 Profiler 탭을 추가로 갖게 될 것이다.

Components 탭에서는 오직 리액트 컴포넌트만 트리 형태로 볼 수 있다. 클릭하면 더 상세한 정보를 볼 수 있다. props을 볼 수 있으며 hooks을 볼 수 있는데 값을 임의로 수정해서 브라우저상에서 확인해볼수도 있다. 수정하면 UI에 영향을 미친다.

![React devTools](https://lh3.googleusercontent.com/XWuZGqIrIsaoKHUqqQ2rs_GhS5JaH1p5pPBIUpj22mjNRNdR3Ana8FKz4B7JwsA6HIFVXGuU7pa4ELiW6iUNhs0Iyg=w640-h400-e365-rj-sc0x00ffffff)

---

강의명

- Udemy : React 완벽 가이드
