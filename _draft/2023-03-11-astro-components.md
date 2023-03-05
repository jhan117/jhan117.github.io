---
title: "컴포넌트 [Astro 문서 번역]"
categories:
  - "Astro"
toc: true
toc_label: "Components"
toc_sticky: true
---

Astro Components는 Astro 프로젝트의 기본 구성 요소입니다. client-side 런타임 없는 HTML 전용 템플릿 components입니다.

**만약 HTML을 안다면, 당신은 이미 첫 Astro component를 작성할 수 있을 만큼 충분히 알고 있습니다.**

Astro component 문법은 HTML 상위 집합(superset)입니다. 문법은 [HTML이나 JSX를 작성한 경험이 있는 사람이라면 친숙하게 느껴지도록 설계되었고](#astro와-jsx-차이점), components나 JavaScript 확장을 포함할 수 있도록 지원합니다. 파일 확장자로 Astro Component를 찾을 수 있습니다. : `.astro`

Astro Components는 매우 유연성이 있습니다. 종종, Astro component는 header나 프로필 card 같은 재사용 가능한 UI를 페이지에 포함합니다. 다른 경우, Astro component는 SEO를 쉽게 사용할 수 있는 일반적인 `<meta>` 태그 모음과 같이 HTML의 작은 조각을 포함할 수 있습니다. Astro components는 전체 페이지 레이아웃을 포함할 수도 있습니다.

Astro components에 대해 알아야 할 가장 중요한 것은 빌드 중에 HTML로 렌더링 하는 것입니다. component 내부에서 JavaScript 코드를 실행해도 사용자에게 보내는 최종 페이지에서는 제거된 상태로 모두 미리 실행됩니다. 그 결과 기본적으로 자바스크립트 footprint없이 추가되어 더 빠른 사이트가 됩니다.

## Component 구조

Astro component는 두 주요 부분으로 구성됩니다. : Component Script, Component Template. 각 부분은 서로 다른 일을 수행하지만 함께 사용하기 쉽고 빌드 하기 원하는 게 무엇이든지 충분히 표현하는 프레임워크를 제공하는 것을 목표로 합니다.

```html
---
// Component Script (JavaScript)
---

<!-- Component Template (HTML + JS Expressions) -->
```

다른 components 안에 components를 사용하여 점점 더 고급 UI를 빌드 할 수 있습니다. 예를 들어, `Button` component는 아래와 같이 `ButtonGroup` component를 생성하는 데 사용될 수 있습니다.

```js
---
import Button from './Button.astro';
---

<div>
  <button title="Button 1" />
  <button title="Button 2" />
  <button title="Button 3" />
</div>
```

### Component Script

Astro는 코드 펜스(code fence)(`---`)를 사용해 Astro component의 component script를 식별합니다. 이전에 Markdown을 써본 적이 있다면, 이미 *frontmatter*라고 불리는 비슷한 개념에 익숙할 것입니다. component script에 대한 Astro의 아이디어는 이 개념에서 직접적으로 영감을 받았습니다.

component script를 사용하여 template 렌더링 하는 데 필요한 JavaScript 코드를 작성할 수 있습니다. 이는 다음을 포함합니다:

- 다른 Astro components 가져오기
- React 같은 다른 프레임워크 component 가져오기
- JSON 파일 같은 데이터 가져오기
- API 또는 데이터베이스에서 content fetch 하기
- template에서 참조할 변수 만들기

```jsx
---
import SomeAstroComponent from '../components/SomeAstroComponent.astro';
import SomeReactComponent from '../components/SomeReactComponent.jsx';
import someData from '../data/pokemon.json';

// `<X title="Hello, World" />`와 같은 전달된 component props에 접근하기
const {title} = Astro.props;
// 개인 API이나 데이터베이스에서도 외부 데이터를 fetch하기
const data = await fetch('SOME_SECRET_API_URL/users').then(r => r.json());
---
<!-- 여기에 template -->
```

코드 펜스는 당신이 작성한 JavaScript가 "펜스 된" 것을 보장하도록 설계되었습니다. frontend 애플리케이션으로 빠져나가거나 사용자의 손에 들어가지 않을 것입니다. 사용자의 브라우저에서 끝날까 봐 걱정하지 않고 비싸거나 민감한(개인적인 데이터베이스에서 요청한 것 같이) 코드를 안전하게 작성할 수 있습니다.

> **TIP**  
> component script에 TypeScript을 작성할 수도 있습니다.

### Component Template

component script 아래에 component template이 있습니다. component template은 component의 HTML 출력을 결정합니다.

여기에 일반 HTML을 작성하면 import 되고 사용되는 Astro 페이지에서 HTML을 component가 렌더링 합니다.

그러나 Astro의 component template 문법은 JavaScript 표현식, import된 component 및 특별한 Astro 지시어도 지원합니다. component script에서 (페이지 빌드 시간에) 정의된 데이터와 값은 동적으로 만들어진 HTML을 생성하기 위해 component template에서 사용될 수 있습니다.

```jsx
---
// 여기에 component script!
import ReactPokemonComponent from '../components/ReactPokemonComponent.jsx';
const myFavoritePokemon = [/* ... */];
---
<!-- HTML 코멘트 지원! -->

<h1>Hello, world!</h1>

<!-- component script에서 온 props와 다른 변수들 사용: -->
<p>My favorite pokemon is: {Astro.props.title}</p>

<!-- hydrate에 대한 `client:` 지시어와 함께 다른 components 포함 -->
<ReactPokemonComponent client:visible />

<!-- JSX와 유사한, HTMl와 JavaScript 표현식 혼합: -->
<ul>
  {myFavoritePokemon.map((data) => <li>{data.name}</li>)}
</ul>

<!-- template 지시어를 사용하여 여러 문자열 또는 objects에서 클래스 이름 작성! -->
<p class:list={["add", "dynamic", {classNames: true}]} />
```

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

지역 변수는 HTML 요소와 컴포넌트 둘 다에 속성 값을 넘겨 중괄호 안에서 사용될 수 있습니다.

```jsx
---
const name = "Astro";
---
<h1 class={name}>Attribute expressions are supported</h1>

<MyComponent templateLiteralNameAttribute={`MyNameIs${name}`} />
```

> 주의
> HTML 속성은 문자열로 바뀔 수 있고, HTML 요소에 함수나 object를 전달하는 것이 불가능합니다. 예를 들어, Astro 컴포넌트 안에 HTML 요소에 이벤트 핸들러를 할당할 수 없습니다.
>
> ```jsx
> function handleClick () {
>   console.log("button clicked!");
> }
> ---
> <!-- ❌ This doesn't work! ❌ -->
> <button onClick={handleClick}>Nothing will happen when you click me!</button>
> ```
>
> 대신, vanilla JavaScript에서 했던 것 처럼 이벤트 핸들러를 추가한 client-side 스크립트를 사용합니다.
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

Astro는 JSX 논리 연산자와 삼항 연산자를 사용해 HTML를 조건부로 표시할 수 있습니다.

### 동적 태그

또한 HTML 태그 이름이나 컴포넌트 import에 변수를 설정함으로써 동적 태그를 사용할 수 있습니다.

동적 태그를 사용할 때:

- 변수 이름은 대문자여야 합니다. 예를 들어, `element`가 아닌 `Element`를 사용합니다. Astro는 HTML 태그 문자 그대로로 변수 이름을 렌더하기 위해 시도할 것입니다.
- 직접작용은 지원하지 않습니다. client:\* hydration directives를 사용할 때, Astro는 어떤 컴포넌트가 생산을 위한 번들하는지 알필요가 있고 동적 태그 패턴은 작업으로부터 이를 예방합니다.

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

- [Astro DOCS - Astro Components](https://docs.astro.build/en/core-concepts/astro-components/)

```

```
