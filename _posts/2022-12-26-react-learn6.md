---
title: "Reducer를 사용하여 Side Effects 처리 & Context API 사용하기"
categories:
  - "React"
toc: true
toc_label: "section 10"
toc_sticky: true
last_modified_at: 2022-12-27
---

## 학습 목표

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

즉, 구성 요소가 다시 렌더링되어 이러한 것들이 변경될 수 있는 경우에 추가해야 한다. ~~라고 강의에서 말하는데 본인은 프로젝트를 할 때, 그냥 어떤 것들이 변경되면 리렌더링 되었으면 좋겠다고 생각하는 그 어떤 것들만 작성했다.(수정: 이렇게 하면 안된다고 함~~)

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

자 이제 진짜로 reducer를 이용하여 만들어보자.

1. useReducer를 호출하고 react 함수 내에서 선언한다.

```js
import { useReducer } from "react";

const Login = () => {
  const [emailState, dispatchEmail] = useReducer();
};
```

2. useReducer에 넣을 Reducer 함수를 생성하고 초기 값을 넣어준다. 이 때, Reducer는 React 컴포넌트 함수 외부에서 선언 해야 한다.

```js
const emailReducer = (state, action) => {
  return { value: "", isValid: false };
};

const Login = () => {
  const [emailState, dispatchEmail] = useReducer(emailReducer, {
    value: "",
    isValid: null,
  });
};
```

#### state 사용 방법

useState와 동일하게 사용하면 된다.

```js
<input value={emailState.value} />
```

#### state 업데이트 하는 방법

useState와 동일하게 업데이트 하면 되지만 살짝 다르다.

```js
const emailReducer = (state, action) => {
  // 만약 값을 받은 action이라면 받은 액션 값으로 리턴
  if (action.type === "USER_INPUT") {
    return { value: action.value, isValid: action.value.includes("@") };
  }
  // 만약 검증만을 위한 action이라면 기존값 state의 값으로 리턴
  if (action.type === "INPUT_BLUR") {
    return { value: state.value, isValid: state.value.includes("@") };
  }

  return { value: "", isValid: false }; // 이도저도 아닌 기본값
};

const Login = () => {
  const [emailState, dispatchEmail] = useReducer(emailReducer, {
    value: "",
    isValid: null,
  });

  const emailChangeHandler = (event) => {
    // action을 넘긴다. 값을 넣어줘야 하므로 넘겨줌.
    dispatchEmail({ type: "USER_INPUT", value: event.target.value });

    // reducer의 상태에서 isValid 값 상태를 넘긴다.
    // 비밀번호도 reducer로 작성되었다고 가정함.
    setFormIsValid(emailState.isValid && passwordState.isValid);
  };

  const validateEmailHandler = () => {
    // action을 넘긴다.
    dispatchEmail({ type: "INPUT_BLUR" });
  };
};
```

### 디바운싱으로 만든 useEffect가 너무 많이 호출되는 문제점 해결하기

디바운싱은 좋은 방법이지만 만약 비밀번호가 7자리까지라는 유효성을 가지고 있을 때 문자 하나를 추가하는 경우에도 유효하지만 다시 useEffect가 실행된다. 이러한 문제 때문에 많이 호출된다는 생각을 가지고 있다.

이를 위해 객체 디스트럭처링 개념을 사용해볼 수 있다. 이는 객체의 특정 속성을 추출하는 것이다. 사용은 간단하게 중괄호를 열고 그 안의 속성을 입력한 후 별칭을 콜론 옆에 써준다. 별칭을 만들어주는 이유는 emailState도 isValid고, passwordState에서도 isValid로 같은 이름을 사용하기 때문에 만들어줬다.

```js
// 객체 디스트럭처링(Destructuring)
const { isValid: emailIsValid } = emailState;
const { isValid: passwordIsValid } = passwordState;

useEffect(() => {
  const identifier = setTimeout(() => {
    // 별칭 변수를 사용해주면 된다.
    setFormIsValid(emailIsValid && passwordIsValid);
  }, 500);

  return () => {
    clearTimeout(identifier);
  };
}, [emailIsValid, passwordIsValid]);
```

이렇게 되면 유효성이 바뀌지 않는 이상 재호출 되지 않는다는 장점이 있다. emailState.isValid로 사용하여도 똑같이 동작하지만 일반적으로는 디스트럭처링을 사용한다는 것이다. 어쨌거나 핵심은 전체 객체 대신 특정 속성을 종속성으로 전달한다는 점이 핵심이다.

### `useState()` vs `useReducer()`

|                   `useState()`                    |               `useReducer()`                |
| :-----------------------------------------------: | :-----------------------------------------: |
|               주요 state 관리 도구                |         더 강력함이 필요할 때 좋음          |
|            개별적인 state/data에 좋음             |          연관된 state/data에 좋음           |
| state 업데이트가 쉽고 종류가 적은 업데이트에 좋음 | 더 복잡한 state 업데이트가 있는 경우에 좋음 |

### Context API

다른 자식간의 props 전달이 불가능해 state 끌어올리기를 해야한다는 것을 떠올려보자. 만약 앱이 커진다면 props를 전달만 하고 사용하지 않는 컴포넌트가 생긴다. 물론 이것이 나쁜 것은 아니지만 많이 복잡한 앱의 경우 state를 경로를 찾기가 어려워진다. 긴 props 체인이 생긴다.

이를 위해서 리액트 내부적으로 state 저장소 역할을 하는 Context API가 있다. 이는 부모에서 받아오지 않고 직접 받아올 수 있는 기능을 한다. props 체인 없이 구현할 수 있다.

src 자식으로 폴더명은 context, state, store 아무거나 생성한다. 보통 store을 사용한다. 파일명은 자유롭게 해도 되지만 컴포넌트가 아니기 때문에 강사님은 케밥 표기법(kebab-case)을 사용하는 걸 추천한다고 한다.

createContext에서 반환받는 것은 컴포넌트가 되거나 컴포넌트를 감싸는 것이 될 것이기에 파스칼 표기법(PascalCase)으로 작성해준다.

```js
// auth-context.js
import { createContext } from "react";

// 기본값을 넘겨준다.
const AuthContext = createContext({
  isLoggedIn: false,
});

export default AuthContext;
```

앱에서 사용하려면 두 가지 작업을 해야 한다. 첫째는 공급, 둘째는 소비이다. 공급은 "나 여기 있어"라고 알려주는 역할을 하며 소비는 연동하고 리스닝하는 것이다.

#### 공급

JSX 코드로 감싸는 것이다. 필요한 곳에서 감싸면 된다.

```js
<AuthContext.Provider>{/* ... 컴포넌트들 */}</AuthContext.Provider>
```

#### 소비

이제 리스닝을 하는 것인데 이는 두 가지 방법이 있다. 소비자를 이용하거나 리액트 훅을 이용하는 방법이 있지만 일반적으로 리액트 훅 방식을 많이 사용한다.

소비자는 함수 형태로 중괄호 사이에 자식을 갖는다. 인수로 컨텍스트 데이터를 갖고 JSX 코드를 반환한다. 그리고 사용하고 싶은 곳에서 사용하면 된다.

컨텍스트 객체의 기본값은 공급자 없이 소비하는 경우에만 사용되므로 만약 변하지 않는다면 공급자가 필요없다. 따라서 소비자를 사용할 경우 공급자에서 값을 넘겨줘야 한다. 이렇게 하면 업데이트 될 때마다 값이 변경될 것이다. 물론 값에 함수도 넘겨줄 수 있다.

```js
// 데이터 넘겨줌
<AuthContext.Provider
  value={{ isLoggedIn: isLoggedIn, onLogout: logoutHandler }}
>
  {/* ... 컴포넌트들 */}
</AuthContext.Provider>
```

```js
// 이용될 데이터가 있는 곳
<AuthContext.Consumer>
  {(ctx) => {
    return <div>{ctx.isLoggedIn && <p>Login</p>}</div>;
  }}
</AuthContext.Consumer>
```

다음으로 리액트 훅인 `useContext()`를 이용하는 방법을 설명하겠다.

```js
import { useContext } from "react";

const ctx = useContext(AuthContext);

return <div>{ctx.isLoggedIn && <p>Login</p>}</div>; // 변수 사용법은 같음
```

두 가지 방법 중 어느 것을 사용하든 상관은 없다. 소비자를 이용하는 것이 틀린 것은 아니지만 리액트 훅이 좀 더 우아한 방법이라고 한다.

### 별도의 컨텍스트 관리 컴포넌트 만드는 법

공급자 컴포넌트를 만들고 export 해준다. 그 안에 로직과 작성하고 공급자 컴포넌트를 반환해준 뒤 index.js에서 이 컴포넌트로 감싸주면 된다. 컨텍스트 사용법은 똑같다.

```js
// index.js
root.render(
  <AuthContextProvider>
    <App />
  </AuthContextProvider>
);
```

```js
// auth-context.js
export const AuthContextProvider = (props) => {
  // 관련 로직 (리액트 훅 사용 가능 컴포넌트이기 때문에)

  return (
    <AuthContext.Provider
      value={{
        isLoggedIn: isLoggedIn,
        onLogout: logoutHandler,
      }}
    >
      {props.children}
    </AuthContext.Provider>
  );
};
```

### Context 제한사항

만약 자주 변경되는 곳에서 context를 사용하면 안된다. 매초마다 state가 변경되는 경우를 말한다. 왜냐하면 최적화되어 있지 않기 때문이다. 실제로 공식 리액트 팀원이 말한 것이다.

그런데 만약 앱 전체에 걸쳐서 전달하는데 state가 자주 변경되는 경우에는 어떻게 해야할까? 나중에 배우겠지만 Redux를 사용하면 된다.

다시 말하지만 긴 props 체인의 경우에는 Context를 사용하는 것이 좋지만 일반적인 경우까지 대체하는 것은 추천하지 않는다. props이 나쁜 것이 아니다. 그저 긴 props 체인이 생길 경우에만 context를 사용하길 권장한다.

### Hooks의 규칙

우리는 지금까지 4개의 훅을 배웠다. `useState()`,`useEffect()`, `useReducer()`, `useContext()`을 사용해봤다. 더 많은 훅이 있지만 아무튼 이젠 훅의 규칙을 배울 차례이다.

중요한 규칙이 2개 있다.

1. 리액트 함수에서만 호출해야한다.
   - React 컴포넌트 함수
   - 커스텀 훅
2. 최상위 수준에서만 호출해야 한다.
   - 중첩 함수(useEffect 내부 등)에서 호출 안됨
   - block statement(if문, for문 등)에서 호출 안됨

useEffect의 경우 항상 참조하는 모든 항목을 의존성으로 함수 내부에 추가 해야 한다. 그렇게 하지 않을 합당한 이유가 없다면 말이다.

### Forward Refs

이 훅은 Input과 명령형으로 상호작용할 수 있게 해준다. 이는 일반적인 리액트 패턴이 아니기 때문에 자주 해서는 안되지만 input이나 스크롤링과 같은 예외적인 곳에서는 아주 유용하다.

예를 들어 제출을 할 때 유효하지 않은 input이 있다면 focus 기능을 넣고 싶다고 가정해보자. input 내부에 useEffect 함수에 ref를 넣고 focus를 실행시킬 수 있다. 그러나 이 방법은 항상 두번째만 포커스 될 것이다. 첫번째는 이미 일시적으로 포커스 됐다가 넘어간 상황으로 보일 것이다. 이런 방식으로는 구현할 수 없다. 그렇다면 Input 컴포넌트에 ref를 사용하고 싶어질 것이다.

```js
const inputRef = useRef();

useEffect(() => {
  inputRef.current.focus();
}, []);
```

그래서 Input 컴포넌트를 ref 내장형처럼 사용할 수 있다. 마치 Input 컴포넌트가 input 태그인 마냥 참조할 수 있는 것이다. 이는 props.ref로 사용할 수 없다. 예약어이기 때문이다. 대신 useImperativeHandle라는 훅을 사용할 수 있는데 props나 state를 전달하지 않고 컴포넌트에서 무언가를 직접 호출하거나 조작해서 사용하게 해준다.

```js
// Input.js
import { useRef, useImperativeHandle, forwardRef } from "react";

// forwardRef로 감쌌지만 컴포넌트이다.
const Input = forwardRef((props, ref) => {
  const inputRef = useRef();

  const activate = () => {
    inputRef.current.focus();
  };

  useImperativeHandle(ref, () => {
    // 외부에서 접근 가능한 이름을 객체로 키에 이름, 값에 함수를 넘겨준다.
    return {
      focus: activate,
    };
  });
});
```

```js
// Login.js
const emailInputRef = useRef();
const passwordInputRef = useRef();

const submitHandler = () => {
  if (!emailIsValid) {
    emailInputRef.current.focus(); // Input.js에서 설정한 외부에서 접근 가능한 이름
  } else {
    passwordInputRef.current.focus();
  }
};

return (
  <form>
    <Input ref={emailInputRef} />
    <Input ref={passwordInputRef} />
  </form>
);
```

다시 말하지만 이 방식이 자주 사용되어서는 안된다. 그러나 인풋 트리거라던가 스크롤링 등 예외적으로 좋게 사용되는 상황은 있다.

## 궁금한 점

나는 프로젝트를 진행할 때 의존성 배열에 Side Effect 함수 안에 있는 것을 모두 적지 않았고, 적더라도 몇 개만 적었다. 그리고 심지어는 context를 넣어서 리렌더링 시킨 경우가 많은데 내가 한 방식은 잘못된건가?? 작동은 해서 별 의심 안했는데 좀 더 알아봐야 할 문제 같다.

위의 유효성 검사 코드에서도 굳이 입력시마다 검증 안해도 나는 제출시에 딱 한 번만 검증하게 의존성 배열에 button의 클릭 state를 만들어서 넣어줬다. 이런저런 방법이 있는건지 아니면 내 방식이 좋지 않은 코드인지는 모르겠다... 좀 더 조사해보기로 하자.

강의를 계속 듣다보니 바로 나왔다. 합당한 이유가 없는 이상 내 방식대로 하면 안된다고 한다. 어멋... 프로젝트 전부 다 수정해야겠다.

---

강의명

- Udemy : React 완벽 가이드
