---
layout: post
title: "React 생명주기 메서드"
categories: React
---

## 생명주기 메서드란?

생명주기 메서드(Lifecycle method)는 컴포넌트의 각각의 단계에서 실행되는 커스텀 기능입니다. 컴포넌트가 만들어지고 DOM에 삽입될 때(mounting), 컴포넌트가 업데이트될 때 및 컴포넌트가 DOM에서 마운트 해제될 때(unmounted) 혹은 제거될 때 사용할 수 있는 기능을 제공합니다.

### Mounting

아래 메서드들은 컴포넌트의 인스턴스가 생성되어 DOM 상에 삽입될 때에 순서대로 호출된다.

- **constructor()**
- **render()**
- **componentDidMount()**
- static getDerivedStateFromProps()

### Updating

props 또는 state가 변경되면 갱신이 발생합니다. 아래 메서드들은 컴포넌트가 다시 렌더링될 때 순서대로 호출됩니다.

- **render()**
- **componentDidUpdate()**
- static getDerivedStateFromProps()
- shouldComponentUpdate()
- getSnapshotBeforeUpdate()

### Unmounting

아래 메서드는 컴포넌트가 DOM 상에서 제거될 때에 호출됩니다.

- **componentWillUnmount()**

---

출처 : [컴포넌트 생명주기 - React](https://ko.reactjs.org/docs/react-component.html#the-component-lifecycle)
