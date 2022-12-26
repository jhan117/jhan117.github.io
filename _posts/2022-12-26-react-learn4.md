---
title: "React 강의 정리 4"
categories:
  - "React"
toc: true
toc_label: "section 8"
toc_sticky: true
last_modified_at: 2022-12-26
---

## 학습 목표

- 지금까지 배운 내용을 적용할 시간(Time to apply what we learned thus far)

## 연습하기 : 연습 프로젝트 완료

이 섹션을 들을까 말까 고민이 됐다. 프로젝트를 해본 경험이 있었기에 건너뛰어도 상관은 없을 거라 생각했지만 강사님의 코드를 보고 본인의 고칠 코드는 많지 않을까 싶어서 들었다.

[기능 목록]

- 유저 이름과 나이를 입력하고 추가 버튼을 누르면 하단에 정보가 뜬다. 정상적으로 추가되었을 경우 입력 부분은 초기화된다.
- 만약 빈 칸으로 입력한다면, 경고창이 뜨게 한다. 이 때, 경고창은 OK 버튼을 누르거나 backdrop을 누르면 사라진다.
- 만약 나이에 음수를 입력한다면, 다른 경고창이 뜨게 한다.

### 사용자 정의 컴포넌트에 css 지정하기

전에 이걸 원했는데 찾아봐도 원하는 답을 못 찾았는데... 열심히 스타일을 하나 하나 넘겨준 지난 날들이 떠올랐다. 클래스 모듈도 넘겨줄 수 있다는 걸 생각하지 못했다.

```js
// 이 파일과 같은 module.css 파일에 스타일을 box 클래스로 지정한다.
<Card className={classes.box} />
```

```js
// Card.js
// 기존의 스타일과 같이 쓰고 싶다면 이렇게 props로 넘겨주면 간단했다.
<div className={`${classes.card} ${props.className}`} />
```

### 버튼 컴포넌트 만들기

만약 스타일은 같지만 기능이 다른 컴포넌트를 만들고자 할 때가 있다. 그럴 때마다 그 컴포넌트에 해당하는 페이지를 넘겨서 체크한 후 기능을 동작시켰는데 비효율적이었다. 이렇게 간단하게 사용할 수 있었다. type을 예외처리 해주는 것도 인상 깊었다. 나였으면 그냥 값을 받든 말든~ 오류만 없다면 신경 안썼을 텐데 이런 방식으로도 처리할 수 있구나를 알았다. 또한, 사용자 정의 컴포넌트의 경우 속성 이름을 똑같이 해도 무방했는데 괜히 왜일까? 아무생각 없이 길고 복잡한 이름을 일관성 없이 사용했던 것 같다. 일관성이 부족했다.

```js
// 스타일은 같으므로 Button.module.css에 작성한다.
// type은 넘겨줄 경우 type을 지정해주고 아닌 경우 'button'으로 지정해준다.
// onClick에 넣어줄 함수를 props로 받는다.
const Button = (props) => {
  return (
    <button
      className={classes.button}
      type={props.type || "button"}
      onClick={props.onClick}
    >
      {props.children}
    </button>
  );
};
```

### form의 입력값 저장, 초기화, 검증(validate)하기

입력값을 useState로 저장하는 방법이다.

```js
const [enteredData, setEnteredData] = useState(""); // 초깃값 빈 문자열

dataChangeHandler = (event) => {
  setEnteredData(event.target.value);
};

<input onChange={dataChangeHandler} />;
```

만약 input 값을 초기화하고 싶지 않다면 넘겨주지 않아도 된다. 그러나 값을 변경하고 싶다면 state를 넘겨주면 간단하다. 전에 설명한 양방향 바인딩의 개념이다.(learn2.md 참고)

```js
const [enteredData, setEnteredData] = useState(""); // 초깃값 빈 문자열

const addDataHandler = () => {
  // 초기화
  setEnteredUsername("");
  setEnteredAge("");
};

dataChangeHandler = (event) => {
  setEnteredData(event.target.value);
};

return (
  <form onSubmit={addDataHandler}>
    <input value={enteredData} onChange={dataChangeHandler} />
    {/* 초기화값 넘겨주기 위해 value로 저장!*/}
  </form>
);
```

입력값을 검증하기 위해서는 로직을 추가해주면 된다. `trim()`을 사용할 생각은 못했는데 좌우 공백을 제거해주는 함수이다. 그리고 return을 넣어서 함수를 탈출하면 된다.

입력값은 전부 문자열로 저장되므로 숫자형이랑 비교하고 싶다면 안전하게 하기 위해 변수 앞에 '+'를 붙여서 숫자형으로 타입을 변환시켜주는 것이 좋다.

```js
const addDataHandler = () => {
  if (enteredData.trim().length === 0) {
    return;
  }
  // 타입 변환
  if (+enteredData < 1) {
    return;
  }
};
```

### form으로 submit 받은 값들을 출력하기

전에는 프로젝트를 하면서 form 데이터를 다른 컴포넌트에 전달할 때 setState를 props로 넘겨줬는데 그러다보니 form 파일에서 데이터를 처리하는 함수가 있다던가 그런 경우가 많았다. 부모 파일에서 데이터를 저장하는 식으로 관리해도 나쁘지 않은 것 같다. 처리하는 함수를 한눈에 확인할 수 있기 때문이다.

```js
// Parent.js
const [data, setData] = useState([]);

// setState를 넘겨주는 것보다 함수를 넘겨준다.
const addDataHandler = (data) => {
  // 최신 버전을 업데이트 해주기 위해 함수형으로 작성한다.
  setData((preData) => {
    return [...preData, { data, id: Math.random().toString() }]; // id를 이렇게 random 함수로 생성하는 게 꼭 좋지는 않다. 겹칠 가능성이 있다.
  });
};

return (
  <div>
    <AddData onAddData={addDataHandler} /> {/* form 컴포넌트 */}
    <Data users={data} /> {/* 출력할 컴포넌트 */}
  </div>
);
```

```js
// AddData.js
const addDataHandler = () => {
  props.onAddData(data); // data 넘겨주기
};
```

### 에러 모달창 만들기

강사님은 상단에 두는 걸 좋아하시는 듯 했다. 위치는 자유긴 하다. 내용은 디자인마다 다르기에 생략했으며 어디에 넣었는지만 표시하였다. Modal을 App.js에 넣어도 되지만 강사님은 모달과 직접적인 관계가 있는 form에 넣었으며 나였어도 form에 넣었을 것 같다.

```js
// Modal.js
<div /> // backdrop
<Card /> // modal window
```

```js
// Form.js
<Modal /> // 위에서 만든 모달 컴포넌트
<form /> // Form
```

에러를 state로 관리하는게 조금 신기했다. 나였으면 손수 일일이 에러 버전 넘겨주고 그에 맞는 문자를 반환하게 했을 것이었다. 이런 방법을 써봐야겠다. 초깃값을 undefined로 설정하는 것이 중요하며, 에러 발생할 로직에 setError 구문을 추가해주면 된다. 강사님은 여기서 에러 객체를 추가했다.

```js
const [error, setError] = useState(); // 이 때 초깃값은 undefined이다. 에러가 없다는 의미!

// ...setError 넣은 함수 생략

return error && <Modal title={error.title} message={error.message} />;
```

모달창을 누르거나 button을 누르면 error를 없애는 함수를 넘겨주면 창이 닫힌다.

```js
// Form.js
const errorHandler = () => {
  setError(null);
};

return error && <Modal onConfirm={errorHandler} />;
```

```js
// Modal.js
<div onClick={props.onConfirm}/>
<Button onClick={props.onConfirm}/>
```

---

강의명

- Udemy : React 완벽 가이드
