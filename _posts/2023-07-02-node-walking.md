---
title: "노드 탐색"
categories:
  - "JavaScript"
toc: true
toc_label: "노드 탐색"
toc_sticky: true
---

노드 탐색 프로퍼티는 모두 접근자 프로퍼티다. 단, 노드 탐색 프로퍼티는 setter없이 getter만 존재하여 참조만 가능한 읽기 전용 접근자 프로퍼티다. 읽기 전용 접근자 프로퍼티에 값을 할당하면 아무런 에러 없이 무시된다. 그리고 공백(white space) 문자는 텍스트 노드를 생성한다. 이를 공백 텍스트 노드라고 한다. 따라서 노드를 탐색할 때는 공백 문자가 생성한 공백 텍스트 노드에 주의해야 한다. 공백 문자를 제거하면 공백 텍스트 노드를 생성하지 않지만 가독성이 좋지 않으므로 권장하지 않는다.

```html
<ul id="fruits"><li
class="apple"></li></ul>
```

## 자식 노드 탐색

- `Node.prototype.childNodes` : 자식 노드 모두 탐색 NodeList 반환 (텍스트 노드 포함)
- `Element.prototype.children` : 자식 노드 중 요소 노드만 모두 탐색 HTMLCollection 반환 (텍스트 노드 미포함)
- `Node.prototype.firstChild` : 첫 번째 자식 노드 반환 (텍스트 노드이거나 요소 노드)
- `Node.prototype.lastChild` : 마지막 자식 노드 반환 (텍스트 노드이거나 요소 노드)
- `Element.prototype.firstElementChild` : 첫 번째 자식 요소 노드 반환 (요소 노드)
- `Element.prototype.lastElementChild` : 마지막 자식 요소 노드 반환 (요소 노드)

## 자식 노드 존재 확인

- `Node.prototype.hasChildNodes` : 자식 노드가 존재하면 true, 존재하지 않으면 false 반환 (텍스트 노드 포함)
- `children.length` or `childElementCount` : 자식 노드 중 텍스트 노드가 아닌 요소 노드 존재하는지 확인

## 부모 노드 탐색

- `Node.prototype.parentNode` : 텍스트 노드는 DOM 트리의 최종단 노드인 리프 노드(leaf node)이므로 부모 노드가 텍스트 노드인 경우는 없다.

## 형제 노드 탐색

- `Node.prototype.previousSibling` : 부모 노드가 같은 형제 노드 중에서 자신의 이전 형제 노드를 탐색해 반환 (요소 노드 또는 텍스트 노드)
- `Node.prototype.nextSibling` : 부모 노드가 같은 형제 노드 중에서 자신의 다음 형제 노드를 탐색해 반환 (요소 노드 또는 텍스트 노드)
- `Element.prototype.previousElementSibling` : 부모 노드가 같은 형제 노드 중에서 자신의 이전 형제 노드를 탐색해 반환 (요소 노드)
- `Element.prototype.nextElementSibling` : 부모 노드가 같은 형제 노드 중에서 자신의 다음 형제 노드를 탐색해 반환 (요소 노드)

---

출처 : 모던 자바스크립트 Deep Dive
