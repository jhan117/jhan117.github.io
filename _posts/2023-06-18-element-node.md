---
title: "요소 노드 취득"
categories:
  - "JavaScript"
toc: true
toc_label: "요소 노드 취득"
toc_sticky: true
---

HTML의 구조나 내용 또는 스타일 들을 동적으로 조작하려면 먼저 요소 노드를 취득해야 한다.

## id를 이용한 요소 노드 취득

`Document.prototype.getElementById` 메서드는 인수로 전달한 id 어트리뷰트 값(이하 id 값)을 갖는 하나의 요소 노드를 탐색하여 반환한다. 이는 Document.prototype의 프로퍼티이므로 반드시 문서 노드인 document를 통해 호출해야 한다. 언제나 첫 번째 요소 노드만 반환하며 HTML 요소가 존재하지 않는 경우 null을 반환한다. 또한, HTML 요소에 id 어트리뷰트를 부여하면 id 값과 동일한 이름의 전역 변수가 암묵적으로 선언되고 해당 노드 객체가 할당되는 부수 효과가 있지만 동일한 이름의 전역 변수가 이미 선언되어 있으면 이 전역 변수에 노드 객체가 재할당 되지 않는다.

```html
<!DOCTYPE html>
<html>
  <body>
    <ul>
      <li id="apple">Apple</li>
      <li id="banana">banana</li>
    </ul>
    <script>
      // id 값이 'apple'인 요소 노드 탐색 및 반환
      const elem = document.getElementById("apple");
      // 취득한 요소 노드의 style.color 프로터피 값 변경
      elem.style.color = "red";
    </script>
  </body>
</html>
```

## 태그 이름을 이용한 요소 노드 취득

`Document.prototype/Element.prototype.getElementsByTagName` 메서드는 인수로 전달한 태그 이름을 갖는 모든 요소 노드들을 탐색하여 여러 개의 요소 노드 객체를 갖는 DOM 컬렉션 객체인 HTMLCollection 객체를 반환한다. 이는 유사 배열 객체이면서 이터러블이다. 또한, 모든 요소 노드를 취득하려면 인수로 '\*'를 전달한다. Document.prototype.getElementsByTagName 메서드는 document를 통해 호출하며 DOM 전체의 요소 노드를 탐색하여 반환한다. 하지만 Element.prototype.getElementsByTagName 메서드는 특정 요소 노드를 통해 호출하며, 특정 요소 노드의 자손 노드 중에서 요소 노드를 탐색하여 반환한다. 만약 요소가 존재하지 않는 경우 빈 HTMLCollection 객체를 반환한다.

```html
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits">
      <li>Apple</li>
      <li>banana</li>
    </ul>
    <ul>
      <li>HTML</li>
    </ul>
    <script>
      // DOM 전체에서 태그 이름이 li인 요소 노드를 모두 탐색하여 반환
      const lisFromDocument = document.getElementsByTagName("li");
      console.log(lisFromDocument); // HTMLCollection(3) [li, li, li]

      // ul#fruits 요소의 자손 노드 중에서 탐색하여 반환
      const fruits = document.getElementById("fruits");
      const lisFromFruits = fruits.getElementsByTagName("li");
      console.log(lisFromFruits); // HTMLCollection(2) [li, li]
    </script>
  </body>
</html>
```

## class를 이용한 요소 노드 취득

`Document.prototype/Element.prototype.getElementsByClassName` 메서드는 인수로 전달한 class 어트리뷰트 값(이하 class 값)을 갖는 모든 요소 노드들을 탐색하여 HTMLCollection 객체를 반환한다. 인수로 전달한 class 값은 공백으로 구분하여 여러 개의 class를 지정할 수 있다. Document.prototype.getElementsByClassName 메서드는 document를 통해 호출하며 DOM 전체의 요소 노드를 탐색하여 반환한다. 하지만 Element.prototype.getElementsByClassName 메서드는 특정 요소 노드를 통해 호출하며, 특정 요소 노드의 자손 노드 중에서 요소 노드를 탐색하여 반환한다. 만약 요소가 존재하지 않는 경우 빈 HTMLCollection 객체를 반환한다.

```html
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits">
      <li class="apple">Apple</li>
      <li class="banana">Banana</li>
    </ul>
    <div class="banana">Banana</div>
    <script>
      // DOM 전체에서 클래스 값이 'banana'인 요소 노드를 모두 탐색하여 반환
      const bananaFromDocument = document.getElementsByClassName("banana");
      console.log(bananaFromDocument); // HTMLCollection(3) [li.banana, div.banana]

      // ul#fruits 요소의 자손 노드 중에서 탐색하여 반환
      const fruits = document.getElementById("fruits");
      const bananaFromFruits = fruits.getElementsByClassName("banana");
      console.log(bananaFromFruits); // HTMLCollection [li.banana]
    </script>
  </body>
</html>
```

## CSS 선택자를 이용한 요소 노드 취득

CSS 선택자(selector)는 스타일을 적용하고자 하는 HTMl 요소를 특정할 때 사용하는 문법이다.

`Document.prototype/Element.prototype.querySelector` 메서드는 인수로 전달한 CSS 선택자를 만족시키는 **하나의** 요소 노드를 탐색하여 반환한다.

- 여러 개인 경우 첫 번째 요소 노드만 반환
- 존재하지 않는 경우 null을 반환
- CSS 선택자가 문법에 맞지 않는 경우 DOMException 에러가 발생

`Document.prototype/Element.prototype.querySelectorAll` 메서드는 인수로 전달한 CSS 선택자를 만족시키는 **모든** 요소 노드를 탐색하여 반환한다. 이는 여러 개의 요소 노드 객체를 갖는 DOM 컬렉션 객체인 NodeList 객체를 반환한다. 이 객체는 유사 배열 객체이면서 이터러블이다.

- 존재하지 않는 경우 빈 NodeList 객체를 반환
- CSS 선택자가 문법에 맞지 않는 경우 DOMException 에러가 발생

```html
<!DOCTYPE html>
<html>
  <body>
    <div id="fruits">
      <ul>
        <li class="apple">Apple</li>
        <li class="banana">Banana</li>
      </ul>
    </div>
    <div class="banana">Banana</div>
    <script>
      // class 어트리뷰트 값이 'banana'인 첫 번째 요소 노드를 탐색하여 반환
      const bananaFromDocument = document.querySelector(".banana");
      bananaFromDocument.style.color = "yellow";

      // #fruits 요소의 자손 노드 중에서 ul 요소의 자식 요소인 li 요소를 모두 탐색하여 반환
      const fruits = document.getElementById("fruits");
      const liFromFruits = fruits.querySelectorAll("ul > li");
      console.log(liFromFruits); // NodeList(2) [li.apple, li.banana]
    </script>
  </body>
</html>
```

CSS 선택자 문법을 사용하는 위의 두 메서드는 id, 태그, 클래스로 취득하는 메서드보다 다소 느린 것으로 알려져 있다. 하지만 CSS 선택자 문법을 사용하여 좀 더 구체적인 조건으로 요소 노드를 취득할 수 있고 일관된 방식으로 요소 노드를 취득할 수 있다는 장점이 있다. 따라서 id 컨트리뷰트가 있는 요소 노드를 취득하는 경우에는 getElementById 메서드를 사용하고 그 외의 경우에는 querySelector, querySelectorAll 메서드를 사용하는 것을 권장한다.

## 특정 요소 노드를 취득할 수 있는 지 확인

`Element.prototype.matches` 메서드는 인수로 전달한 CSS 선택자를 통해 특정 요소 노드를 취득할 수 있는지 확인한다. 이 메서드는 이벤트 위임을 사용할 때 유용하다.

```html
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits">
      <li class="apple">Apple</li>
      <li class="banana">Banana</li>
    </ul>
    <script>
      const $apple = document.querySelector(".apple");
      console.log($apple.matches("#fruits > li.apple")); // true
      console.log($apple.matches("#fruits > li.banana")); // false
    </script>
  </body>
</html>
```

---

출처 : 모던 자바스크립트 Deep Dive
