---
title: "Astro 문법 [Astro 문서 번역]"
categories:
  - "Astro"
toc: true
toc_label: "Syntax"
toc_sticky: true
---

**만약 HTML을 안다면, 당신은 이미 첫 Astro component를 작성할 수 있을 만큼 충분히 알고 있습니다.**

Astro component 문법은 HTML 상위 집합(superset)입니다. 문법은 [HTML이나 JSX를 작성한 경험이 있는 사람이라면 친숙하게 느껴지도록 설계되었고](#astro와-jsx-차이점), components나 JavaScript 확장을 포함할 수 있도록 지원합니다.

## JSX 같은 표현식

Astro component 내에서 frontmatter component script 안에 지역 JavaScript 변수를 정의할 수 있습니다. 그런 다음 JSX와 같은 표현식을 사용하여 이러한 변수를 component의 HTML template에 삽입할 수 있습니다.

> **동적 vs 반응적**  
> 이 방법을 사용하면, frontmatter에서 계산된 동적 값을 포함할 수 있습니다. 그러나 일단 포함되면 이 값들은 반응적이지 않고 절대로 바뀌지 않습니다. Astro component은 빌드 시 한 번만 실행되는 template입니다.
>
> [Astro와 JSX의 차이점](#astro와-jsx의-차이점)에 대한 자세한 예는 아래를 참조하세요.

### 변수

지역 변수는 중괄호 문법을 사용해서 HTML에 추가할 수 있습니다:

```jsx
---
const name = "Astro";
---
<div>
  <h1>Hello {name}!</h1>  <!-- 출력 <h1>Hello Astro!</h1> -->
</div>
```

### 동적 속성들

지역 변수는 HTML 요소와 component 모두에 속성 값을 전달하기 위해 중괄호 안에서 사용될 수 있습니다:

```jsx
---
const name = "Astro";
---
<h1 class={name}>Attribute expressions are supported</h1>

<MyComponent templateLiteralNameAttribute={`MyNameIs${name}`} />
```

> **주의**
> HTML 속성은 문자열로 변환되므로 함수나 object를 HTML 요소로 전달할 수 없습니다. 예를 들어 Astro 컴포넌트 안의 HTML 요소에는 이벤트 핸들러를 할당할 수 없습니다:
>
> ```jsx
> function handleClick () {
>   console.log("button clicked!");
> }
> ---
> <!-- ❌ 작동하지 않습니다! ❌ -->
> <button onClick={handleClick}>Nothing will happen when you click me!</button>
> ```
>
> 대신, vanilla JavaScript에서 했던 것처럼 client-side 스크립트를 사용해 이벤트 핸들러를 추가합니다:
>
> ```jsx
> ---
> ---
> <button id="button">Click Me</button>
> <script>
>   function handleClick () {
>     console.log("button clicked!");
>   }
>   document.getElementById("button").addEventListener("click", handleClick)
> </script>
> ```

### 동적 HTML

지역 변수는 동적으로 생성된 HTML 요소를 생성하기 위해 JSX 같은 함수에서 사용될 수 있습니다.

```jsx
---
const items = ["Dog", "Cat", "Platypus"];
---
<ul>
  {items.map((item) => (
    <li>{item}</li>
  ))}
</ul>
```

Astro는 JSX 논리 연산자와 삼항 연산자를 사용해 HTML를 조건부로 표시할 수 있습니다.

```jsx
---
const visible = true;
---
{visible && <p>Show me!</p>}

{visible ? <p>Show me!</p> : <p>Else show me!</p>}
```

### 동적 태그

변수를 HTML 태그 이름이나 component import로 설정해 동적 태그를 사용할 수도 있습니다.

```jsx
---
import MyComponent from "./MyComponent.astro";
const Element = 'div'
const Component = MyComponent;
---
<Element>Hello!</Element> <!-- <div>Hello!</div> 로 렌더 -->
<Component /> <!-- <MyComponent />  로 렌더 -->
```

동적 태그를 사용하는 경우:

- 변수 이름은 대문자로 표시해야 합니다. 예를 들어 `element`가 아닌 `Element`를 사용합니다. 그렇지 않으면 Astro는 변수 이름을 HTML 태그 문자 그대로 렌더링 하려고 할 것입니다.
- 직접 작용은 지원되지 않습니다. [client:\* hydration directives](https://docs.astro.build/en/core-concepts/framework-components/#hydrating-interactive-components)를 사용할 때, Astro는 production을 위해 번들화할 component를 알아야 하며 동적 태그 패턴으로 인해 이 작업이 수행되지 않습니다.

### Fragments & 다수 요소들

Astro 컴포넌트 템플릿은 JavaScript나 JS와 달리 모든 곳에 <div>나 <>로 감쌀 필요 없이 다수의 요소를 렌더할 수 있습니다.

그러나, 동적으로 생성한 다수의 요소에서 표현을 사용할 때, JavaScript나 JS에서 했던 것처럼 Fragment 안에 이 요소들을 감싸야 합니다. Astro는 <Fragemnt></Fragment> 나 약칭 <></> 둘다 사용하는 걸 지원합니다.

Fragments는 또한 set:\* directives를 추가할 떄 요소를 감싸는걸 피할 떄 유용할 수 있습니다. 다음의 예시에서:

### Astro와 JSX의 차이점

Astro 컴포넌트 문법은 HTML의 상위 집합입니다. 이는 HTML이나 JSX 경험이 있는 사람이면 누구나 친숙하게 느끼도록 설계되었습니다. 그러나 .astro 파일들과 jsx 사이에 몇가지 핵심 차이점이 있습니다.

#### 속성

Astro에서 JSX에서 사용된 camelCase 대신에 모든 HTML 속성은 ㅍ준 kebab-case 형식을 사용합니다. class로 작업되는 React에서 지원되지 않습니다.

#### 코멘트

Astro에서 표준 HTML 코멘트나 JavaScript-style ㅋㅗ멘트를 사용할 수 있습니다.

> 주의
> HTML-style 콤ㅔㄴ트는 브라우저 DOM에 포함될 수 있습니다. 반면에 JS 것은 스킵될 수 있습니다. TODO 메시지나 다른 개발-only 표현을 남기기 위해서, 대신에 JavaScript-style 코멘트를 사용하길 원할지도 모릅니다.

---

- [Astro DOCS - Astro Syntax](https://docs.astro.build/en/core-concepts/astro-syntax/)
