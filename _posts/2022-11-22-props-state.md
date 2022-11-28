---
title: "React props vs state"
categories: React
---

## props

props는 컴포넌트의 입력값입니다. props는 부모 컴포넌트로부터 자식 컴포넌트로 전달된 데이터입니다.

props는 읽기 전용이라는 것에 주의하세요. props는 어떤 방식으로든 수정해서는 안 됩니다.

사용자의 입력 또는 네트워크 응답에 반응하여 어떤 값을 수정해야 한다면 state를 사용하세요.

## props.children

모든 컴포넌트에서 props.children를 사용할 수 있습니다. props.children은 컴포넌트의 여는 태그와 닫는 태그 사이의 내용을 포함합니다.

Class로 정의된 컴포넌트에서는 this.props.children을 사용합니다.

## state

컴포넌트와 관련된 일부 데이터가 시간에 따라 변경될 경우 state가 필요합니다. 예를 들어, Checkbox 컴포넌트는 isChecked state가 필요할 수 있으며, NewsFeed 컴포넌트는 fetchedPosts를 컴포넌트의 state를 통해 계속 주시하려고 할 수 있습니다.

## props와 state

state와 props의 가장 중요한 차이점은 props는 부모 컴포넌트로부터 전달받지만, state는 컴포넌트에서 관리된다는 것입니다. 컴포넌트는 props를 변경할 수 없지만, state는 변경할 수 있습니다.

데이터가 변경되는 각 특정한 부분에 대해, 해당 상태(state)를 “소유”하는 컴포넌트는 하나만 존재해야 합니다. 서로 다른 두 컴포넌트의 상태를 동기화하려고 하지마세요. 대신, 공통 상태를 두 컴포넌트의 공통 조상으로 끌어올리고 해당 데이터를 두 컴포넌트에 props로 전달하세요.

---

출처 : [React 기술 용어 모음 - React](https://ko.reactjs.org/docs/glossary.html#%EC%A0%9C%EC%96%B4-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8-vs-%EB%B9%84%EC%A0%9C%EC%96%B4-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8)
