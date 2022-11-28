---
title: "React 제어 컴포넌트 vs 비제어 컴포넌트"
categories: React
---

## 제어 컴포넌트 vs 비제어 컴포넌트

React는 두 가지 방식으로 form 입력을 처리합니다.

React에 의해 입력값이 제어되는 엘리먼트를 제어 컴포넌트(controlled component) 라고 합니다. 사용자가 제어 컴포넌트에 데이터를 입력하면 변경 이벤트 핸들러가 호출되고 코드가 (업데이트된 값으로 다시 렌더링에 의해) 입력의 유효 여부를 결정합니다. 다시 렌더링하지 않으면 form 엘리먼트는 변경되지 않은 상태로 유지됩니다.

비제어 컴포넌트(uncontrolled component)는 form 엘리먼트가 React 외부에서 작동하는 것처럼 작동합니다. 사용자가 form 필드(input box, dropdown 등)에 데이터를 입력하면 업데이트된 정보가 React에서 별도 처리할 필요 없이 엘리먼트에 반영됩니다. 그러나, 이는 특정 필드가 특정 값을 갖도록 강제할 수 없다는 의미이기도 합니다.

대부분은 controlled component를 사용해야 합니다.

---

출처 : [React 기술 용어 모음 - React](https://ko.reactjs.org/docs/glossary.html#%EC%A0%9C%EC%96%B4-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8-vs-%EB%B9%84%EC%A0%9C%EC%96%B4-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8)
