---
title: "React 강의 정리 6"
categories:
  - "React"
toc: true
toc_label: "section 10"
toc_sticky: true
last_modified_at: 2022-12-26
---

## 학습 목표

**고급 : 리듀서(Reducer)를 사용하여 부작용 처리 & 컨텍스트 API 사용하기**

- (사이드) 이펙트 다루기 (Working with (Side) Effects)
- Reducers를 이용해 더 복잡한 State 관리하기 (Managing more Complex State with Reducers)
- Context로 앱 수준 또는 컴포넌트 수준 State 관리하기 (Managing App-Wide or Component-Wide State with Context)

## 고급 : 리듀서(Reducer)를 사용하여 부작용 처리 & 컨텍스트 API 사용하기

### Side Effects

Effect와 Side Effect 둘 다 같은 의미로 쓰이는 용어다. 그래서 이게 뭘까?

기본적으로 React는 UI를 렌더링하고 사용자 입력에 반응하는 게 주요 업무이다. 그 외를 Side Effect라고 하는데 한국어로는 부작용이라고 한다. 예를 들어 브라우저 저장소에 데이터를 저장하거나 백엔드 서버에 HTTP 요청을 보내는 등을 Side Effects라고 한다. 일반적인 컴포넌트 평가 밖에서 일어나는 것이다.

다시 리액트가 작동되는 방식을 생각해보자. 리액트는 state가 변할 때마다 함수가 재호출된다. 그런데 만약 Side Effect를 state로 바로 불러온다면 무한루프에 빠질 것이다. 왜냐면 응답을 받을 때마다 state가 변경될 거고 그러면 다시 재호출 되면 다시 요청을 하게 되고 또 다시 응답을 받고 무한 루프에 빠진다.

그래서 Side Effect는 직접적으로 함수에 호출해선 안된다. 버그나 무한루프에 빠지거나 http 요청이 무수히 많아질 수 있다는 단점이 있다.

이를 위해 리액트는 useEffect()라는 훅을 제공한다.

`useEffect(() => { ... }, [ dependencies ]);`

첫 번째 인자는 Side Effect 코드가 들어간 함수이고 두 번째 인자는 의존성 배열이다.

### `useEffect()`

함수가 재평가된 후에 의존성에 따라 useEffect가 실행된다는 점이 제일 중요하다. 즉, 의존성 배열이 없다면 호출할 때 딱 한 번만 실행된다. 만약 의존성 배열을 적지 않았다면 그냥 함수 안에 useEffect 없이 적은것이나 마찬가지다.

로그인을 예를 들어 보겠다. 백엔드 서버에 저장할 수 있겠지만 우리는 간단하게 접근하기 쉬운 로컬스토리지에 저장을 하고 로그인 여부를 검증하고 싶다고 가정하자.

```js
// 로그인 시 로그인 여부 0, 1
const loginHandler = () => {
  localStorage.setItem("isLoggedIn", "1"); // key와 value
  setIsLoggedIn(true);
};
```

로그인을 했을 때 1로 변한다. 여기까지는 문제 없다. 그러나 새로고침을 했을 때 로그인 여부를 검사하게 하고 싶다. 함수 안에 그대로 넣으면 무한 루프에 빠지게 되는 문제가 생긴다.

```js
// 값을 변수에 저장하고 체크하는 로직
const getInfo = localStorage.getItem("isLoggedIn");
if (getInfo === "1") {
  setIsLoggedIn(true);
}

const loginHandler = () => {
  localStorage.setItem("isLoggedIn", "1");
  setIsLoggedIn(true);
};
```

위와 같이 작성하면 검사를 하는 도중 1을 만나 state가 업데이트 되기에 다시 재호출이 되고 그러면 또 다시 변수에 저장해서 1임을 확인하면 state가 또 업데이트 되어 다시 재호출이 된다. 즉, 무한 루프가 된다.

이 때, useEffect를 사용하여 해결할 수 있다.

```js
useEffect(() => {
  const storedUserLoggedInInformation = localStorage.getItem("isLoggedIn");

  if (storedUserLoggedInInformation === "1") {
    setIsLoggedIn(true);
  }
}, []);

const loginHandler = () => {
  localStorage.setItem("isLoggedIn", "1");
  setIsLoggedIn(true);
};
```

그렇다면 의존성이 있는 경우는 어떤 예가 있을까?

로그인을 할 때 이메일과 비밀번호의 유효성을 검사할 때를 예를 들 수 있겠다. 물론 이 때 무한 루프가 생기지 않기 때문에 useEffect를 사용하지 않아도 기능적으로는 문제 없어보인다. 그러나 버그가 생길 수 있다는 문제점이 있다. Side Effect 함수에서 사용하는 것들을 의존성에 추가하면된다. 이 때 setState는 리액트에 의해 절대 변경되지 않도록 보장되기 때문에 제거해도 된다.

잘 와닿지 않을 것 같아 useEffect를 사용하지 않는 코드와 비교해보겠다. 기존에는 의존성 중 하나가 변경될 때마다 다시 실행시키는 코드를 작성했다.

```js
// useEffect 없는 코드
// 이메일 체크
const emailChangeHandler = (event) => {
  setEnteredEmail(event.target.value);

  setFormIsValid(
    event.target.value.includes("@") && enteredPassword.trim().length > 6
  );
};

// 비밀번호 체크
const passwordChangeHandler = (event) => {
  setEnteredPassword(event.target.value);

  setFormIsValid(
    event.target.value.trim().length > 6 && enteredEmail.includes("@")
  );
};
```

```js
// useEffect 사용하는 코드
useEffect(() => {
  // 유효성 검사
  setFormIsValid(
    enteredEmail.includes("@") && enteredPassword.trim().length > 6
  );
}, [enteredEmail, enteredPassword]);

const emailChangeHandler = (event) => {
  setEnteredEmail(event.target.value);
};

const passwordChangeHandler = (event) => {
  setEnteredPassword(event.target.value);
};
```

이렇게되면 갑자기 헷갈릴 수도 있다. 뭐지? Side Effect는 타이머라던가, HTTP 요청이라던가... 그런 것이라고 설명을 해놓고 갑자기 state를 업데이트 하는 걸 Side Effect 함수에 넣었다. 그러나 State 여부로 판단하는 것이 아닌 작동 방식을 봐야 한다. 키 입력을 듣고 입력된 데이터를 저장하는 것도 Side Effect에 포함하며, 그리고 그에 대한 응답으로 다른 액션을 실행하는 것도 포함된다. 즉, 키에 대한 응답으로 해당 폼의 유효성을 확인하고 업데이트 하는 것 또한 Side Effect라고 할 수 있다. 이들은 사용자 입력 데이터의 Side Effect이다.

정리하자면 어떤 것이든 무언가에 대한 "응답"으로 실행되는 모든 코드를 Side Effect라고 하며 useEffect는 이들을 다루는데 도움이 된다.

그렇다면 종속 배열에 무엇을 추가해야 할까?

모든 상태 변수와 함수를 포함해야 한다. 그러나 몇 가지 예외가 있다.

- 상태 업데이트 기능을 추가할 필요가 없다. (`setState`)
- '내장'API 또는 함수를 추가할 필요가 없다. (ex. `fetch()`, `localStorage`)
- 구성 요소 외부에서 정의한 변수나 함수를 추가할 필요없다. (useEffect 훅 밖에서 선언한 변수 또는 함수는 작성해도 영향을 끼치지 않는다.)

즉, 구성 요소가 다시 렌더링되어 이러한 것들이 변경될 수 있는 경우에 추가해야 한다. 라고 강의에서 말하는데 본인은 프로젝트를 할 때, 그냥 어떤 것들이 변경되면 리렌더링 되었으면 좋겠다고 생각하는 그 어떤 것들만 작성했다.

### Cleanup

그러나 위에서 작성한 코드도 완벽하지 않다. 왜냐하면 입력될 때마다 함수가 호출되기에 불필요한 네트워크 트래픽을 일으킨다. 그래서 이럴 경우에는 사용자가 적극적으로 타이핑 중일 때는 검증하지 않고 멈출 때를 기다려서 호출하는 방식이 더 좋을 것이다. 이를 디바운싱(그룹화)이라고 한다.

```js
useEffect(() => {
  setTimeout(() => {
    setFormIsValid(
      enteredEmail.includes("@") && enteredPassword.trim().length > 6
    );
  }, 500);
}, [enteredEmail, enteredPassword]);
```

그런데 이는 0.5초 지연되어 보일 뿐 별다른 효과는 없어 보인다. 그 이유는 타이머가 계속 실행되서 그렇다. useEffect는 이를 위해 cleanup 함수를 반환할 수 있다. 클린업 프로세스로써 실행된다. 다음 번에 이 함수를 실행하기 전에 말이다. 정확히 실행되기 전(처음 실행 제외)에 이 클린업 함수가 실행된다. 즉, DOM에서 unmounted 될 때 다른 말로는 컴포넌트가 재사용될 때마다 실행된다.

새로운 타이머를 설정하기 전에 마지막 타이머를 지우는 것이다. 이렇게 하면 사용자가 빠르게 입력하게 되면 계속해서 타이머를 지우니 실행되지 않고 입력을 멈추면 마지막 타이머를 지우지 않으니 연속으로 호출되는 타이머 중 마지막에 호출되는 타이머만 즉, 딱 한 번만 발동이 되는 원리다. 실제로 작동시켜보면 응답이 살짝 느려 보일 수 있는데 밀리초를 미세 조정하면 된다.

```js
useEffect(() => {
  const identifier = setTimeout(() => {
    setFormIsValid(
      enteredEmail.includes("@") && enteredPassword.trim().length > 6
    );
  }, 500);

  return () => {
    clearTimeout(identifier);
  };
}, [enteredEmail, enteredPassword]);
```

### `useReducer()`

state 관리를 도와주므로 `useState()`와 약간 비슷하다. 그러나 더 많은 기능이 있으며 복잡한 state에 유용하다. 만약 복잡한 state의 경우에 `useState()`를 사용한다면 사용 및 관리가 어려워지거나 오류가 발생하기 쉽다. 나쁘거나 비효율적이거나 버그가 생길 수 있는 코드가 되기 쉽다. 이럴 땐 `useState()` 대신 `useReducer()`를 사용할 수 있다.

그렇다면 매일 `useReducer()`를 사용하면 되는 것 아니냐고 말할 수 있지만 그렇지는 않다. 사용이 복잡하고 설정이 좀 더 필요하기 때문에 보통의 경우에는 `useState()`를 사용하는 것을 권장한다.

예를 들어 아까의 기능에서 useEffect를 어떤 이유에서든 사용하기 싫다고 가정해보자. 아래의 코드처럼 작성해야 할 것이다. (아까 작성한 코드랑 같음) 이런 경우 리액트가 state를 스케쥴링하는 방식 때문에 enteredPassword가 최신 상태가 업데이트 되지 않았는데 setFormIsValid에 의해 저장될 수도 있다. 그래서 최신 상태를 유지하기 위해 함수형을 사용하고 싶을 것이다. 그러나 사용할 수 없다. 본인의 이전 state만 가져올 수 있기 때문에 다른 state에 의존하는 setFormIsValid는 최신 상태를 유지할 수 없다. 마찬가지로 validateEmailHandler 함수도 항상 최신 상태의 state를 가져올 수 없다.

이처럼 state를 보니 함께 속하는 State들이 존재하는 경우, 이는 email을 입력받은 것과 검증 여부 State들을 말한다. 또한 setFormIsValid, setEmailIsValid처럼 다른 state에 의존하여 업데이트를 하는 경우에 `useReducer()`를 사용하기 좋은 환경이다.

```js
const [enteredEmail, setEnteredEmail] = useState("");
const [emailIsValid, setEmailIsValid] = useState();

const emailChangeHandler = (event) => {
  setEnteredEmail(event.target.value);

  setFormIsValid(
    event.target.value.includes("@") && enteredPassword.trim().length > 6
  );
};

const validateEmailHandler = () => {
  setEmailIsValid(enteredEmail.includes("@"));
};
```

이런 경우 `useReducer()`말고도 사실 `useState()`로 결합해서 객체로 설정해 해결할 수 있지만 복잡하고 커지고 여러가지 관련된 state가 결합된 경우라면 `useReducer()`를 추천한다.

`const [state, dispatchFn] = useReducer(reducerFn, initialState, initFn);`

state는 기존의 state이고, setState 대신 dispatchFn으로 액션을 수행한다. 그 액션은 reducerFn으로 넘겨주며 state의 함수 형태로 업데이트 하는 것과 유사하지만 액션이 있다. 초깃값은 initialState, 초기함수는 initFn 좀 더 복잡한 경우에 사용한다.

## 궁금한 점

나는 프로젝트를 진행할 때 의존성 배열에 Side Effect 함수 안에 있는 것을 모두 적지 않았고, 적더라도 몇 개만 적었다. 그리고 심지어는 context를 넣어서 리렌더링 시킨 경우가 많은데 내가 한 방식은 잘못된건가?? 작동은 해서 별 의심 안했는데 좀 더 알아봐야 할 문제 같다.

위의 유효성 검사 코드에서도 굳이 입력시마다 검증 안해도 나는 제출시에 딱 한 번만 검증하게 의존성 배열에 button의 클릭 state를 만들어서 넣어줬다. 이런저런 방법이 있는건지 아니면 내 방식이 좋지 않은 코드인지는 모르겠다... 좀 더 조사해보기로 하자.

---

강의명

- Udemy : React 완벽 가이드
