---
title: "클래스 컴포넌트"
categories:
  - "React"
toc: true
toc_label: "section 13"
toc_sticky: true
last_modified_at: 2022-12-28
---

## 학습 목표

- 무엇을 & 왜?
- 클래스 기반 컴포넌트 다루기 (Working with Class-based Components)
- Error Boundaries

## 컴포넌트를 구축하는 다른 방법: 클래스 컴포넌트

### What & Why?

React 16.8 이전에는 hook이 없었기 때문에 다들 클래스 컴포넌트만 사용했었다. 그러나 16.8 이후에는 hook이 생겼고 이는 클래스에서 못 쓴다. 최근에는 함수형 컴포넌트를 많이 쓰는 추세라고 하지만 그래도 클래스를 사용하는 곳도 있다.

### Class-based Components 추가하기

함수형에서 클래스로 변환하는 법은 간단하다. 일단 class를 생성하고 Component에서 render라는 프로토타입 메서드를 이용해 JSX 코드를 반환한다. 만약 props를 사용하고 싶다면 React.Component를 상속받아 확장한 뒤 `this.props`로 쓰면 된다.

```javascript
// 함수형
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

// 클래스
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

### State & Event

State를 사용하는 방법은 생성자에서 인스턴스를 생성하고 초기화하면 된다. 이 때, `this.state`라는 이름을 사용해야 한다. 초기화는 객체만 가능하다. 그리고 업데이트 할 때는 `this.setState()`를 이용하면 된다. 이는 함수형과 달리 병합을 알아서 해주기에 만약 객체에 두 변수가 있을 때 하나만 업데이트 해줘도 덮어쓰기 안되니 걱정하지 않아도 된다. 함수형이랑 같이 함수 형식으로 업데이트 해줄 수 있다. state값을 사용하는 방법은 `this.state.[state명]`으로 사용하면 된다.

클릭 이벤트 등을 작성하는 방법은 먼저 콜백 함수를 프로토타입 메서드에 생성하는 것을 추천한다. 사용할 때는 this를 바인딩 해줘야 한다. bind는 바로 호출하지 않기 때문에 bind 함수를 이용해 this를 넘겨주면 된다.

```javascript
// 함수형
function Welcome() {
  const [showUsers, setShowUsers] = useState(true);

  const toggleUsersHandler = () => {
    setShowUsers((curState) => !curState);
  };

  return (
    <button onClick={toggleUsersHandler}>
      {showUsers ? "Hide" : "Show"} Users
    </button>
  );
}

// 클래스
class Welcome extends React.Component {
  constructor() {
    super();
    this.state = { showUsers: true }; // 무조건 객체
  }

  toggleUsersHandler() {
    this.setState((curState) => ({
      showUsers: !curState.showUsers,
    }));
  }

  render() {
    return (
      <button onClick={toggleUsersHandler.bind(this)}>
        {this.state.showUsers ? "Hide" : "Show"} Users
      </button>
    );
  }
}
```

### 생명주기 메서드

클래스에는 `useEffect()`가 없다. 그 대신 생명주기 메서드로 사용한다.

`componentDidMount() == useEffect(..., [])`

useEffect가 최초 한 번 실행되는 것이랑 같다. 컴포넌트가 mount될 때 한 번 부른다.

`componentDidUpdate(prevProps, prevState, snapshot) == useEffect(..., [someValue])`

useEffect에 있는 의존성 배열에 항목이 있는 경우랑 같다. 컴포넌트가 update될 때 한 번 부른다.

이 때, 의존성 항목이 업데이트 될 때이므로 if문을 생성해 이전값과 비교를 해줘야 한다. 그렇지 않으면 무한 루프에 빠지니 조심하자.

`componentWillUnmount() == useEffect(() => {return () => {...}}, []) // cleanup 함수`

useEffect의 cleanup 함수랑 같다. 컴포넌트가 unmount되기 전에 바로 부른다.

### 클래스 컴포넌트에서 Context 사용하기

Context 사용 문법은 똑같다. 공급자도 똑같고 소비자도 똑같이 사용할 수 있다.

소비를 할 때 2가지 방법을 사용할 수 있는데 하나는 아까 말한 소비자로 사용할 수 있고 또 다른 하나는 완전한 방법은 아니지만 context가 1개일 때만 사용할 수 있다.

```javascript
// 정적 메서드
static contextType = UsersContext;

// 사용
this.context.users
```

### 클래스 컴포넌트 vs 함수형 컴포넌트

어떤 것을 사용하든 선택이다. 그러나 만약 결정해야하는 기로에 서있다면 다음을 판단해보자.

일단 함수형 컴포넌트를 추천한다. 그러나 만약 클래스 컴포넌트를 더 선호하거나 프로젝트에서 사용해야하거나 Error Boundaries를 빌드할 때는 클래스 컴포넌트를 추천한다.

### Error Boundaries

함수형 컴포넌트에는 없는 기능이다. 만약 어떤 컴포넌트에서 에러가 발생시키는 구문을 작성했다고 하자. 근데 이 에러를 처리하고 싶고 부모 컴포넌트에서 관리하고 싶다. 그렇지만 기존의 JS 구문인 try...catch로 사용할 수 없다. 왜냐하면 JSX코드에서 에러를 인식하지 못하기 때문이다. 이를 위해서 에러 경계를 사용해 해결할 수 있다. 이렇게 하면 앱을 중단시키지 않고 에러를 처리할 수 있는 장점이 있다.

1. 에러 경계 컴포넌트를 만들고 `componentDidCatch(error, info)`를 이용하여 에러를 가져오고 하고 싶은 동작을 작성한다.

```js
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }

  componentDidCatch(error, info) {
    // Display fallback UI
    this.setState({ hasError: true });
    // You can also log the error to an error reporting service
    logErrorToMyService(error, info);
  }

  render() {
    if (this.state.hasError) {
      // You can render any custom fallback UI
      return <h1>Something went wrong.</h1>;
    }
    return this.props.children;
  }
}
```

2. 발생시키고 싶은 자식 컴포넌트 주변에 에러 경계 컴포넌트로 감싼다.

```js
<ErrorBoundary>
  <MyWidget />
</ErrorBoundary>
```
