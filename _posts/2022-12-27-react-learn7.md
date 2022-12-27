---
title: "음식 주문 앱(Food order App) 만들기"
categories:
  - "React"
toc: true
toc_label: "section 11"
toc_sticky: true
last_modified_at: 2022-12-27
---

## 학습 목표

**연습 프로젝트: 음식 주문앱을 만들어보자(Building a Food order App)**

우리가 배운것을 적용해봅시다.

[기능 목록]

- 음식 목록이 있으며 장바구니에 음식을 여러번 추가할 수 있고 수량도 정할 수 있다.
- 장바구니를 누르면 창이 뜨고 음식 수량을 변경할 수 있다.
- 장바구니 창에서 주문 버튼을 누르면 콘솔에 Ordering...이 뜨게 만든다. 취소 버튼을 누르면 창이 사라진다.

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

---

강의명

- Udemy : React 완벽 가이드
