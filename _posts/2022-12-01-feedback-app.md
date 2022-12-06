---
title: "React.js로 피드백 앱 구현 프로젝트"
categories:
  - "project"
toc: true
toc_label: "프로젝트 과정"
toc_sticky: true
last_modified_at: 2022-12-01
---

## New Feedback Page

![AddFeedbackForm 구현도](https://user-images.githubusercontent.com/78463832/204954878-da50beb6-8deb-4beb-bebe-2a2dab88756d.png)

```
📂 component
├── 📂 feedback
│   ├── 📄 FeedbackForm
│   ├── 📄 InputForm
│   ├── 📄 SelectForm
│   ├── 📄 TextareaForm
│   └── 📄 BtnsConForm
└── 📂 ui
    ├── 📄 btn
    └── 📂 select
        ├── 📄 Select
        ├── 📄 OptionList
        └── 📄 OptionItem
```

- FeedbackForm.js <-- edit은 select 추가
  - heading : h2
  - InputForm.js : label + input
  - SelectForm.js : label + `Select`
    - Select.js : p + `OptionList` <-- MainBar, Edit에도 재사용
      - OptionList.js : ul + `OptionItem`
        - OptionItem.js : li > p + svg
  - TextareaForm.js : label + textarea
  - BtnsConForm.js : `Btn` + `Btn` <-- Edit은 btn 3개
    - Btn.js : button <-- 여러 Button에 사용

### ✅ FeedbackForm.js

- New, Edit Page 사용
- Input 계열 스타일 유사
- custom
  - svg : 상단 아이콘
  - heading : 제목

### ✅ InputForm.js

- New, Edit Page 사용
- custom
  - value

### ✅ SelectForm.js

- New, Edit Page 사용
- custom
  - state : category, status
  - l Category > p Choose ~ feedback
  - l Update Status > pChange ~ state

#### ✅ Select.js

- suggestions, New, Edit Page 사용
- custom
  - p style
    - Update Status, Category
    - Sort by : span -> Sort by : ,p -> Most ~
  - icon arrow style
    - Update Status, Category
    - Sort by
  - OptionList value -> options
  - Option Modal

#### ✅ OptionList.js

- select에 선택된 값 넘겨주기
  - setSelected
- ulStyle

#### ✅ OptionItem.js

- liStyle
- pStyle
- check icon

### ✅ TextareaForm.js

- New, Edit Page 사용
- detail의 comment에서 textarea 스타일과 유사
- custom
  - value

### ✅ BtnsConForm.js

- New, Edit Page 사용
- custom
  - New : Add Feedback, Cancel
  - Edit : Save Changes, Cancel, Delete

#### ✅ Btn.js

- custom

  - type : 'submit'
  - color : 'purple', 'indigo'
  - colorHsl : hsl

- sort, add, edit

### 문제점

- valid 검사 중에 오류 메시지가 한 곳에만 사라지게 하고 싶은데 양쪽에서 사라지는 문제 발생

#### 원인

`props.isSubmit && !props.isDesc` 식으로 뜨게 만드는데 흠 isSubmit이 입력을 하면 true로 바뀌는 바람에 사라짐!

다른 방안을 생각해보자...

- empty가 언제 떴으면 좋을까?
  - 미입력된 채로 btn 클릭하면 ✅
  - 입력하다가 다시 지우면
  - 입력 후부터 체크

* 언제 사라지면?
  - 입력시 --> 둘 다 사라짐

해결 -> isSubmit을 입력시 true 해제

-> 문제 2 : 입력시 갑자기 저장되는 문제
-> 저장하기 전에 false로 변경
해결 안됨

생각 2 로직을 다시 구상해야 겠음

-> submit clicked state를 하나 더 만드는 건 어떰?
-> 입력시 안 사라짐... -> if 순서 바꿈

해결 완료

## CRA로 media query 세팅하는 법

```js
function isMatch() {
  const query = "(min-width: 40em)";
  return window.matchMedia(query).matches;
}
```

오히려 너무 컴포넌트를 세부적으로 분해를 해버려서 사용하기 어려워진 것 같다.
