---
title: "JS closure"
categories: JavaScript
---

## 정의

외부 함수보다 중첩 함수가 더 오래 유지되는 경우 중첩 함수는 이미 생명 주기가 종료한 외부 함수의 변수를 참조할 수 있다. 이러한 중첩 함수를 클로저(closure)라고 부른다.

## 원리

### 요약

외부 함수보다 더 오래 생존한 중첩 함수는 외부 함수의 생존 여부(실행 컨텍스트의 생존 여부)와 상관없이 자신이 정의된 위치에 의해 결정된 상위 스코프를 기억한다. 이처럼 중첩 함수 내부에서는 상위 스코프를 참조할 수 있으므로 상위 스코프의 식별자를 참조할 수 있고 식별자의 값을 변경할 수 있다.

### 상세

```javascript
// ①
function outer() {
  const x = 10;
  const inner = function () {
    console.log(x);
  }; // ②
  return inner;
}

const innerFunc = outer(); // ③
innerFunc(); // ④ 10
```

환경을 간단하게 표시했다. 이해를 돕기 위해 몇 가지 명칭을 변경했고 세부적인 내용은 뺐다.

#### outer 함수 생성 및 호출

![closure1](https://user-images.githubusercontent.com/78463832/202891476-b8757a0f-27fd-4320-86ef-8dbbda600897.png)

outer 함수의 상위 스코프인 전역 렉시컬 환경을 저장한다.

![closure2](https://user-images.githubusercontent.com/78463832/202891496-d70c1ffc-fda8-458d-a95d-cecf60d2bdae.png)

outer 함수를 호출하면 실행 컨텍스트 스택에 푸쉬되고 outer 렉시컬 환경의 외부 렉시컬 환경인 전역 렉시컬 환경이 참조된다. 그리고 inner 함수의 상위 스코프인 outer 렉시컬 환경을 저장한다. outer 함수가 inner 함수를 반환할 때 전역 변수에 inner 함수가 저장이 되고 outer 실행 컨텍스트는 스택에서 제거된다.

이때, outer 함수는 실행 컨텍스트 스택에서 제거되지만 outer 함수의 렉시컬 환경까지 소멸하는 것은 아니다. outer 함수의 렉시컬 환경은 inner 함수 내부 슬롯에 의해 참조되고 있고 inner 함수는 전역 변수에 참조되고 있으므로 가비지 컬렉션의 대상이 되지 않기 때문이다.

#### inner 함수 호출

![closure3](https://user-images.githubusercontent.com/78463832/202891498-362b72d8-e50f-4003-b17f-a564d2569fc9.png)

inner 함수를 호출하면 inner 함수의 실행 컨텍스트가 생성되고 실행 컨텍스트 스택에 푸시된다. 그리고 렉시컬 환경의 외부 렉시컬 환경에 대한 참조에는 inner 함수 객체의 내부 슬롯에 저장되어있는 참조값이 할당된다.

## 사용

클로저는 상태(state)가 의도치 않게 변경되지 않도록 안전하게 은닉(information hiding)하고 특정 함수에게만 상태 변경을 허용하여 상태를 안전하게 변경하고 유지하기 위해 사용한다.

출처 :

- [모던 자바스크립트 Deep Dive 388p](http://www.yes24.com/Product/Goods/92742567) - 24 클로저
