---
title: "리액트로 Forms & Inputs 다루기"
categories:
  - "React"
toc: true
toc_label: "section 16"
toc_sticky: true
last_modified_at: 2022-12-30
---

## 학습 목표

- What's Complex About Forms?
- Handling Inputs & Forms with React
- Simplifications

## Handling Forms & User Input: Working with Values, Validation & State

### What's Complex About Forms?

Forms과 inputs은 서로 다른 상태를 나타낼 수 있다.

- 하나 또는 그 이상의 inputs이 유효하지 않다.
  - 입력 관련한 에러 메시지 출력
  - 문제되는 입력 강조 표시
  - Form을 제출하거나 저장할 수 없게 한다.
- 모든 inputs이 유효하다.
  - Form을 제출하거나 저장할 수 있게 한다.

그렇다면 언제 검증할까?

- Form이 제출될 때
  - 경고하기 전에 사용자가 유효한 값을 입력할 수 있다.
  - 불필요한 경고를 피하지만 너무 늦게 피드백을 준다.
- input이 포커스를 잃었을 때
  - 경고하기 전에 사용자가 유효한 값을 입력할 수 있다.
  - 사용자가 입력하지 않은 손대지 않은 폼에 대해서 매우 유용하다.
  - 사용자가 고치는 중에 유효한지 알려줄 수 없다는 단점이 있다.
- 키를 입력할 때
  - 유효한 값을 입력하기 전에 사용자에게 경고한다.
  - 유효하지 않은 inputs의 경우 보다 직접적인 피드백을 받을 수 있다.
  - 입력하지도 않았는데 사용자에게 유효하지 않다고 경고를 보낸다는 단점이 있다.

### 사용자 입력 값 가져오기

두 가지 방법이 있다. 하나는 State를 이용하는 방법, 또 하나는 Ref를 이용하는 방법이 있다. 상황에 따라 다른데 만약 입력값을 초기화하고 싶다면 State를 사용하는 것이 좋다. 그리고 만약 입력마다 업데이트 받고 싶으면 State, 제출할 때 딱 한 번 받으면 되면 Ref를 사용하면 된다.

### Handling Inputs & Forms with React

입력할 때, 포커스를 잃을 때, 제출할 때 유효성을 검사하도록 하고 버튼을 비활성화한다.

입력할 때는 `onChange`로 포커스를 잃었을 때는 `onBlur`, 제출할 때는 `onSubmit`을 이용하여 작성할 수 있다.

[코드 참고](https://github.com/jhan117/react-practice-projects/blob/section16/section16/src/components/SimpleInput.js)

### 더 나은 자료

[커스텀 훅으로 Form 관리하는 법](https://academind.com/tutorials/reactjs-a-custom-useform-hook)
[FORMIK-Form 라이브러리](https://formik.org/)

---

강의명

- Udemy : React 완벽 가이드
