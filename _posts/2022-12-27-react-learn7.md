---
title: "음식 주문 앱(Food order App) 만들기"
categories:
  - "React"
toc: true
toc_label: "section 11"
toc_sticky: true
last_modified_at: 2022-12-28
---

## 학습 목표

우리가 배운것을 적용해봅시다.

[기능 목록]

- 음식 목록이 있으며 장바구니에 음식을 여러번 추가할 수 있고 수량도 정할 수 있다.
- 장바구니를 누르면 창이 뜨고 음식 수량을 변경할 수 있다.
- 장바구니 창에서 취소 버튼을 누르거나 backdrop을 누르면 창이 사라진다.

## 연습 프로젝트: 음식 주문앱을 만들어보자(Building a Food order App)

### 이미지 또는 SVG 추가하는 법

참고로 img 태그에 svg 파일 import 해서 사용해도 된다.

```js
// 이미지
import mealsImage from "../../assets/meals.jpg";

<img src={mealsImage} />;
```

```js
// SVG
// CartIcon.js
const CartIcon = () => {
  return <svg></svg>;
};

// 사용하고 싶은 파일
import CartIcon from "../Cart/CartIcon";

<CartIcon width="10" />;
```

강사님은 svg 경우 따로 파일로 만들어서 하셨는데 만약 svg파일로 되어 있는 경우 이런 방식도 있다. 프로젝트 할 때 찾아서 유용하게 썼었다.

```js
import { ReactComponent as Icon } from "../";

<Icon width="10" />;
```

만약 컴포넌트로 불렀다면 바꾸고 싶은 요소를 props로 넘겨줘서 커스텀 할 수 있다.

[React에서 svg 활용법에 대해서 아주 잘 설명해놓은 블로그](https://velog.io/@juno7803/React-React%EC%97%90%EC%84%9C-SVG-%ED%99%9C%EC%9A%A9%ED%95%98%EA%B8%B0)

### bind 함수 사용하기

`bind()`는 함수를 사전에 구성한다. 그래서 함수가 실행될 때 받을 인수를 미리 구성할 수 있다.

```js
<Item onRemove={removeHandler.bind(null, item)} />
```

이론은 알겠고 언제 쓰이는가에 대해서 알아보자. 보통 bind는 this와 메서드 내부의 중첩 함수나 콜백 함수의 this가 불일치하는 문제를 해결할 때 사용하는 걸로 알고 있는데 여기서는 신기하게 인수를 미리 구성하기 위해 사용하였다. bind가 명시적으로 함수를 호출 해야 하는 특성(불러야 호출된다는 뜻)이 있는데 이를 이용해서 미리 인수를 바인딩해준 것이다.

이렇게 사용하면 아래 Item 컴포넌트에 인수로 전달해줄 item을 props로 따로 전달해 호출하지 않아도 된다는 장점이 있다.

```js
// Item.js
<button onClick={props.onAdd}>+</button>
```

참고로 바인딩이란 식별자와 값을 연결하는 과정을 의미한다.

### findIndex 함수 사용하여 특정 조건에 특정 속성만 업데이트하기

ES6에서 도입되었다. `findIndex()`는 배열의 요소를 순회하면서 인수로 전달된 콜백 함수를 호출하여 반환값이 true인 첫 번째 요소의 인덱스를 반환한다. 존재하지 않는다면 -1을 반환한다.

```js
const users = [
  { id: 1, name: "Lee" },
  { id: 2, name: "Kim" },
];

// id가 2인 요소의 인덱스를 구한다.
users.findIndex((user) => user.id === 2); // → 1
```

콜백함수는 findIndex 메서드를 호출한 요소값과 인덱스, findIndex 메서드를 호출한 배열 자체, 즉 this를 순차적으로 전달받을 수 있다.

자 이제 직접 사용해보자. 예를 들어 음식을 추가했을 때 같은 음식인 경우에도 새로운 목록으로 추가되는 것이 아닌 수량만 변경되었으면 좋겠다고 생각해보자.

나였다면 filter 함수를 사용해 복잡한 로직을 내부에 썼을 것 같다. 그러나 이 방법은 콜백 함수가 복잡해지는 문제점이 있는데 강사님은 findIndex 함수를 이용하여 직관적으로 작성하셨다. 참고로 reducer 함수에서 작성하였으니 코드를 읽을 때 참고하자.

```js
// index 찾기
const existingItemIndex = state.items.findIndex(
  (item) => item.id === action.item.id
);
// index로 존재하는 아이템 찾기
const existingItem = state.items[existingItemIndex];

let updatedItems;
// 아이템 존재 여부
if (existingCartItem) {
  // 기존 값은 복사해주고 amount 값만 추가해주기
  const updatedItem = {
    ...existingCartItem,
    amount: existingCartItem.amount + action.item.amount,
  };

  // 업데이트 할 배열을 복사하고 위에서 구한 index 이용하여 위의 값으로 대체하기
  updatedItems = [...state.items];
  updatedItems[existingCartItemIndex] = updatedItem;
} else {
  // 새로운 아이템은 그대로 추가해주기
  updatedItems = state.items.concat(action.item);
}
```

### filter로 배열에서 특정 아이템 삭제하기

장바구니의 아이템 개수가 1개일 때 마이너스 버튼을 누르면 목록에서 삭제하고 싶다고 생각해보자. 이 때 filter 함수를 유용하게 쓸 수 있을 것이다. 참고로 새로운 배열이나 객체를 반환한다.

```js
// id가 같으면 false로 배열에서 삭제됨
updatedItems = state.items.filter((item) => item.id !== action.id);
```

---

강의명

- Udemy : React 완벽 가이드
